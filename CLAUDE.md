# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

An iOS (iPhone) app from 2012 bundling four crack-me keygen solvers plus a configurable random password generator. **Discontinued** — no new features or fixes planned. Do not suggest migrating to Swift, ARC, Storyboards, or SwiftUI unless explicitly asked; the value of the repo is as a historical reference.

See `.github/copilot-instructions.md` for the same guidance phrased for Copilot, and `README.md` for feature/algorithm detail.

## Build / run

- Open `Project/To Key.xcodeproj` in Xcode; build/run with `Cmd+R` on a simulator or device.
- Targets iOS SDK 3.0+ and uses manual retain/release, so it likely needs an older Xcode to compile unmodified.
- No tests, no lint, no CI. Manual verification in the simulator is the only validation path.
- Pre-built `Build/To Key.app` and `Build/To Key.ipa` exist — treat `Build/` as artifacts, not source.

## Conventions that bite

- **Manual retain/release, no ARC.** Balance every `retain`/`alloc` with `release`; the existing `dealloc` methods are the pattern. (Note: the keygen methods have unreachable `[stringOut release]` after `return` — preserved as-is.)
- **`DWORD` = `unsigned long`**, typedef'd per file, used for all keygen math to mimic the original 32-bit x86 register semantics. Keep it for any keygen arithmetic.
- **4-space indentation, camelCase methods.** Match surrounding style.
- **XIB-only UI.** Each view controller has a paired `.xib`; do not introduce programmatic layout or Storyboards.

## Architecture

- Master-detail under a `UINavigationController` (set up in `AppDelegate.m`). `MasterViewController` is a `UITableView` whose rows come from `OptionsList.plist` — name, description, icon per entry. Add menu entries there, never hardcode them.
- **Keygen dispatch is by title string.** `DetailViewController.m`'s `textView:shouldChangeTextInRange:` selects the algorithm by string-comparing `self.title` against literal challenge names (`@"~EvilCode's Keygen-Me #1"`, `@"KeygenMe #1"`, `@"Ghost Keygen-me"`, `@"Muckis Crackme #2"`). Adding a keygen requires both an `OptionsList.plist` entry **and** a matching `self.title` branch plus its method — the two are coupled only through that exact string.
- `PasswordViewController.m` keeps the four sliders' total ≤ 20 (enforced in `ChangeSlider:`) and emits 15 passwords per run via `rand()` over a pool concatenated from the enabled character classes.

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body "<imperative description>"`
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body "<description>"`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
