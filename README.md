<h1 align="center">To Key</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/to-key/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/to-key.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/to-key/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/to-key.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

An iOS application that bundles keygen solvers for classic reverse-engineering crack-me challenges together with a configurable random password generator. Built as an educational exercise in 2012 to study serial key algorithms and mobile development with Objective-C on the UIKit framework.

**Language:** Objective-C (UIKit) | **IDE:** Xcode | **Status:** Discontinued (2012-03-29)

## Features

- **Crack-me key generators** -- solves four classic keygen challenges by reimplementing their registration algorithms:
  - **EvilCode's Keygen-Me #1** -- iterates over each character, applies a left-shift by 2 and adds 82, then derives a second key by multiplying with 2748; outputs the serial in `~Evil-<HEX>-<HEX>-Code<HEX>` format
  - **Boonz KeygenMe #1** -- subtracts each ASCII value and adds a correction constant (+25), then cubes the intermediate key; outputs the serial in `Bon-<HEX>-<HEX>-41720F48` format
  - **Ghost Keygen-me (Dinamico)** -- applies XOR with 10 and adds 2 per character in an accumulator loop; outputs the serial in `C<DEC>` format
  - **Muckis Crackme #2** -- uses combined left-shift-4 / right-shift-5 with XOR and addition per character, then squares the difference from a magic constant (789999); outputs the serial in `CM2-<HEX>-<HEX>` format
- **Password generator** -- produces up to 15 random passwords at once with configurable character composition:
  - Four independent sliders controlling the count of lowercase letters, uppercase letters, digits, and special characters
  - Total password length capped at 20 characters with a live counter
  - Vertical slider UI (rotated 270 degrees via `CGAffineTransform`)
- **Navigation-based UI** -- master-detail architecture with a `UINavigationController`, `UITableView` master list loaded from a plist data source, and two detail screens (keygen input/output and password configuration)
- **Plist-driven data model** -- all crack-me entries and password program entries are defined in `OptionsList.plist`, with name, description, and icon fields per entry

## Technologies

| Component | Technology |
|-----------|------------|
| Language | Objective-C (manual retain/release, no ARC) |
| Framework | UIKit (iOS SDK 3.0+) |
| UI | XIB/NIB files (MainWindow, MasterViewController, DetailViewController, PasswordViewController) |
| Build system | Xcode project (`project.pbxproj`) |
| Target architecture | ARMv7 |
| Orientation | Portrait only |

## Project Structure

```
Project/
  To Key.xcodeproj/          # Xcode project configuration
  To Key/
    main.m                   # Application entry point
    AppDelegate.h/.m         # App lifecycle, sets up UINavigationController
    MasterViewController.h/.m/.xib  # Table view listing crack-mes and password tools
    DetailViewController.h/.m/.xib  # Keygen input/output view with 4 algorithm implementations
    PasswordViewController.h/.m/.xib  # Password generator with 4 vertical sliders
    OptionsList.plist         # Data source defining all menu entries (names, icons, descriptions)
    To Key-Info.plist         # Bundle configuration (version 1.0, bundle ID, icon files)
    To Key-Prefix.pch        # Precompiled header (UIKit + Foundation)
    en.lproj/                # English localization strings
    *.png                    # App icons, backgrounds, per-author avatar icons
Imgs/                        # Source PSD and PNG assets (icons, backgrounds, table headers/footers)
Build/                       # Pre-built .app bundle
```

## Technical Details

### Keygen Algorithms

All four algorithms operate on the ASCII character values of the input string. They iterate character by character, accumulating an intermediate key value using bitwise operations (shifts, XOR) and arithmetic. The final serial key is formatted as a hexadecimal string with algorithm-specific prefixes and delimiters. The `DWORD` type (`unsigned long`) is used for all key computations, matching the 32-bit register sizes of the original x86 crack-me targets.

### Password Generation

The password generator builds a character pool by concatenating subsets of lowercase (`a-z`), uppercase (`A-Z`), numeric (`0-9`), and special characters (`!@#$%...`) based on slider values. It then uses `rand()` to select characters from this combined pool, generating 15 passwords per invocation. The four sliders are constrained so their sum never exceeds 20.

## Installation

1. Open `Project/To Key.xcodeproj` in Xcode
2. Select an iOS Simulator or connected device as the build target
3. Build and run (Cmd+R)

**Note:** This project uses manual retain/release (MRR) memory management and targets iOS SDK 3.0+. It may require an older version of Xcode to compile without modifications.

## Contributing

This project is no longer actively maintained. You may submit small bug fixes or documentation improvements via Pull Request, but reviews may be infrequent and contributions are not guaranteed to be merged.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
