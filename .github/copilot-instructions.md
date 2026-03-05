# Copilot Instructions for To Key

## Project Overview

**To Key** is a discontinued iOS application (last updated 2012-03-29) that bundles keygen solvers for classic reverse-engineering crack-me challenges together with a configurable random password generator. It was built as an educational exercise to study serial key algorithms and iOS mobile development using Objective-C on the UIKit framework.

> **Status:** Discontinued. The repository is preserved as a historical reference. No new features or bug fixes are planned.

---

## Repository Structure

```
.
├── Build/                        # Pre-built .app and .ipa bundles
│   ├── To Key.app
│   └── To Key.ipa
├── Imgs/                         # Source PSD and PNG assets (icons, backgrounds, table headers/footers)
├── Project/                      # Xcode project and all source files
│   ├── To Key.xcodeproj/         # Xcode project configuration (project.pbxproj)
│   └── To Key/
│       ├── main.m                # Application entry point
│       ├── AppDelegate.h/.m      # App lifecycle; sets up UINavigationController
│       ├── MasterViewController.h/.m/.xib  # UITableView master list (crack-mes + password tool)
│       ├── DetailViewController.h/.m/.xib  # Keygen input/output view (4 algorithm implementations)
│       ├── PasswordViewController.h/.m/.xib # Password generator with 4 vertical sliders
│       ├── OptionsList.plist     # Data source defining all menu entries (names, icons, descriptions)
│       ├── To Key-Info.plist     # Bundle configuration (version 1.0, bundle ID, icon files)
│       ├── To Key-Prefix.pch    # Precompiled header (UIKit + Foundation)
│       ├── en.lproj/             # English localization strings
│       └── *.png                 # App icons, backgrounds, per-author avatar icons
├── CONTRIBUTING.md               # Historical build information
├── LICENSE                       # MIT License
└── README.md                     # Project documentation
```

---

## Technology Stack

| Component         | Technology                                          |
|-------------------|-----------------------------------------------------|
| Language          | Objective-C (manual retain/release — no ARC)        |
| Framework         | UIKit (iOS SDK 3.0+)                                |
| UI                | XIB/NIB files                                       |
| Build system      | Xcode project (`project.pbxproj`)                   |
| Target platform   | iOS (iPhone), ARMv7                                 |
| Orientation       | Portrait only                                       |
| IDE               | Xcode (older version targeting iOS SDK 3.0+)        |

---

## Build, Test, and Run Commands

> **Note:** This project uses manual retain/release (MRR) memory management and targets iOS SDK 3.0+. It may require an older version of Xcode to compile without modifications. There is no automated test suite.

### Open and Build (Historical)

1. Open `Project/To Key.xcodeproj` in Xcode
2. Select an iOS Simulator or connected device as the build target
3. Build and run with `Cmd+R`

### Pre-built Artifacts

A pre-built `.app` bundle and `.ipa` file are available in the `Build/` directory and can be installed directly on a compatible device or simulator without rebuilding.

There are **no lint, test, or CI/CD commands** — this project predates any automated tooling for this codebase.

---

## Architecture and Design Patterns

- **Navigation-based UI:** Master-detail architecture using `UINavigationController` with a `UITableView` master list.
- **Plist-driven data model:** All crack-me entries and password generator entries are defined in `OptionsList.plist` (name, description, icon per entry). No external data fetching.
- **Manual memory management (MRR):** Uses `retain`/`release`/`autorelease` — no ARC. Always follow MRR rules when modifying Objective-C code.
- **`DWORD` type:** `unsigned long` is aliased as `DWORD` for all keygen computations, matching original x86 crack-me 32-bit register sizes.

### Keygen Algorithms (in `DetailViewController.m`)

All four algorithms iterate over ASCII character values of the input string, applying bitwise and arithmetic operations:

| Keygen             | Operations                                         | Output format                      |
|--------------------|----------------------------------------------------|------------------------------------|
| EvilCode #1        | Left-shift 2, +82; second key = key × 2748         | `~Evil-<HEX>-<HEX>-Code<HEX>`     |
| Boonz #1           | Subtract ASCII + constant (+25), cube              | `Bon-<HEX>-<HEX>-41720F48`        |
| Ghost (Dinamico)   | XOR with 10, +2 per char accumulator               | `C<DEC>`                           |
| Muckis Crackme #2  | Left-shift-4 / right-shift-5 + XOR + add; square diff from 789999 | `CM2-<HEX>-<HEX>` |

### Password Generation (in `PasswordViewController.m`)

- Builds a character pool by concatenating subsets of lowercase, uppercase, digits, and special characters based on slider values.
- Uses `rand()` to select characters from the pool.
- Generates 15 passwords per invocation.
- Four sliders are constrained so their sum never exceeds 20.

---

## Development Workflow

Since this project is discontinued and has no CI/CD pipeline:

1. Open `Project/To Key.xcodeproj` in Xcode.
2. Make changes to source files in `Project/To Key/`.
3. Build with `Cmd+B` to check for compilation errors.
4. Run with `Cmd+R` on a simulator or device.
5. Commit changes with a descriptive message.

---

## Coding Conventions

- **Language:** Objective-C with manual retain/release (no ARC, no Swift).
- **Memory management:** Always balance `retain`/`release`; use `autorelease` pools where appropriate.
- **UI:** XIB files for view layout — do not introduce programmatic layout or Storyboards.
- **Data source:** Add new menu entries to `OptionsList.plist` — do not hardcode entries in view controllers.
- **Types:** Use `DWORD` (`unsigned long`) for keygen intermediate computations to match original 32-bit target semantics.
- **Formatting:** Follow existing Objective-C code style in the project (2-space indentation, method names in camelCase).

---

## Common Tasks

### Adding a New Keygen Algorithm

1. Add an entry to `OptionsList.plist` under the keygen section (name, description, icon).
2. Implement the algorithm in `DetailViewController.m` by adding a new case in the keygen dispatch logic.
3. Build and run to verify.

### Modifying the Password Generator

1. Adjust slider logic or character pools in `PasswordViewController.m`.
2. Ensure the slider sum constraint (max 20 total characters) is preserved.

---

## Notes for AI Assistants

- This is a **historical/archived project** — avoid suggesting rewrites to Swift, ARC, or modern iOS APIs unless explicitly requested.
- There is no test suite; manual verification via Xcode simulator is the only validation method.
- The `Build/` directory contains pre-built artifacts and should not be modified by source code changes.
- All UI is XIB-based — do not introduce Storyboards or SwiftUI.
