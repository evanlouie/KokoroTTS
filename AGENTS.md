# Repository Guidelines

## Project Structure & Module Organization

KokoroTTS is a macOS SwiftUI application. App source lives in `KokoroTTS/`, with the entry point in `KokoroTTSApp.swift`, UI in `ContentView.swift` and `HelpView.swift`, app/menu integration in `AppDelegate.swift`, and the TTS state engine in `KokoroTTSModel.swift` plus `KokoroTTSModel+*.swift` feature extensions. Assets and localized strings are under `KokoroTTS/Assets.xcassets` and `*.xcstrings`. Large model data lives in `Resources/`; these files are Git LFS-managed and must stay available in the app bundle. Xcode project metadata is in `KokoroTTS.xcodeproj/`.

## Build, Test, and Development Commands

- `git lfs install && git lfs pull`: fetches `Resources/kokoro-v1_0.safetensors` and voice data after cloning.
- `open KokoroTTS.xcodeproj`: opens the app for normal Xcode development.
- `xcodebuild -project KokoroTTS.xcodeproj -scheme KokoroTTS -configuration Debug -destination 'platform=macOS' build`: builds locally from the command line.
- `xcodebuild -project KokoroTTS.xcodeproj -scheme KokoroTTS -configuration Release -derivedDataPath build -destination 'platform=macOS' CODE_SIGN_IDENTITY="-" CODE_SIGNING_REQUIRED=NO CODE_SIGNING_ALLOWED=NO`: mirrors the unsigned CI release build.

## Coding Style & Naming Conventions

Use Swift 5 conventions with two-space indentation, `UpperCamelCase` types, and `lowerCamelCase` properties and methods. Keep SwiftUI views small enough to scan; move model, playback, audio, media-control, and preprocessing behavior into targeted `KokoroTTSModel+Feature.swift` extensions. Prefer `// MARK: -` sections for larger files. Keep user-visible strings in the localization catalogs when practical.

## Testing Guidelines

There is no dedicated test target in the current project. Before submitting changes, at minimum run the Debug `xcodebuild` command above and manually exercise text input, service invocation, playback controls, voice selection, saving audio, and menu bar visibility. If adding tests, create an XCTest target named `KokoroTTSTests` and name files after the feature under test, for example `TextPreprocessingTests.swift`.

## Commit & Pull Request Guidelines

Recent commits use short, imperative summaries such as `Fix menu-bar Dock-icon lockout...`, `Add menu bar icon...`, and `Bump version to 1.3.0`. Keep commits focused and describe the user-visible behavior changed. Pull requests should include a concise summary, testing performed, linked issues when applicable, and screenshots or screen recordings for UI changes. Call out any Git LFS, localization, signing, or bundle-resource changes explicitly.

## Security & Configuration Tips

Do not commit personal signing identities, provisioning profiles, or local Xcode settings. Keep generated archives, DMGs, and derived data out of git. Verify LFS-tracked model files are real binaries, not pointer files, before release builds.
