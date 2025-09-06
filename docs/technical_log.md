# Stem Checker: Technical Log

## 1. Core Application Architecture

The application is composed of two main targets: the main `StemChecker` application and a `CheckStemsExtension` (a Finder Sync Extension).

### `StemChecker` (Main App)

- **Language:** Swift
- **UI Framework:** SwiftUI
- **Audio Engine (`AudioEngine.swift`):**
  - Built on `AVFoundation` and `AVAudioEngine`.
  - Manages a collection of `AVAudioPlayerNode` instances, one for each audio file.
  - Connects all player nodes to the engine's `mainMixerNode` to ensure synchronized output.
  - Handles loading, playing, and stopping/resetting audio files.
- **URL Handling:**
  - The app registers a custom URL scheme: `stemchecker://`.
  - An `AppDelegate` is used to capture incoming URLs from the operating system.
  - A singleton `URLHandler` class parses these URLs to extract file paths, which are then published to the UI using `@Published` properties.
- **View (`ContentView.swift`):**
  - A simple SwiftUI view that displays the list of loaded stems and provides "Play"/"Stop" buttons.
  - Observes the `URLHandler` and automatically loads new stems when they are received from the Finder extension.

### `CheckStemsExtension` (Finder Sync Extension)

- **Purpose:** To add a "Check Stems" item to the Finder's right-click context menu.
- **Implementation (`FinderSync.swift`):**
  - The menu item is only displayed when the user's selection consists exclusively of audio files (verified using `UniformTypeIdentifiers`).
  - When triggered, it constructs a `stemchecker://open?path=...` URL containing the file paths of all selected items.
  - It then uses `NSWorkspace.shared.open()` to launch the main application with this custom URL.

## 2. Key Development Milestones

1.  **Prototype:** Successfully built a standalone macOS app that could load and play multiple audio files in sync.
2.  **Finder Extension:** Added a `FinderSync` extension target to the project.
3.  **URL Scheme:** Implemented a custom URL scheme to allow the extension to communicate with and launch the main app.
4.  **App Delegate:** Replaced the initial SwiftUI `.onOpenURL` modifier (which was causing persistent build failures) with a more robust `AppDelegate` to handle incoming URLs.

## 3. Current Issue: Finder Extension Registration Failure

Despite a successful build, the `CheckStemsExtension` is failing to appear in **System Settings → Privacy & Security → Extensions → Finder**. This prevents the extension from being enabled and used.

### Troubleshooting Steps Taken:

1.  **Deployment Target:** Verified that the macOS Deployment Target for both the main app and the extension targets are set to `12.0` or higher, both in the Target settings and the overarching Project settings.
2.  **Target Membership:** Confirmed that no main app files (like `StemCheckerApp.swift`) are mistakenly included in the extension's target membership.
3.  **Clean Build:** Performed multiple clean builds (`Shift + Command + K`) to remove stale artifacts.
4.  **Derived Data:** Deleted the entire project's Derived Data folder to force Xcode to perform a completely fresh build and re-indexing.
5.  **Build Order ("Two-Step"):** Ran the `StemChecker` scheme first, followed by the `CheckStemsExtension` scheme, to ensure the main app bundle was registered before the extension.
6.  **Manual Registration (`pluginkit`):** Used the command-line tool `pluginkit -a <path-to-app>` to attempt to force the system to register the extension.

Despite these exhaustive steps, the system still does not list the extension. This suggests the issue is a deep, intermittent caching or permissions problem within macOS itself, rather than a simple configuration error in the project. A system reboot is the recommended next step after the `pluginkit` command.
