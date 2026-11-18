# Tidegate / Shreem — Android Releases

This repository hosts **only installable Android builds** for the Tidegate / Shreem visitor-management app. The application source code is kept in a private repository.

## Download the latest app

The stable install link used by the admin portal's QR code is:

```text
https://github.com/Khelit/tidegate-releases/releases/latest/download/shreem.apk
```

Every new APK must be attached to a release with the asset filename exactly **`shreem.apk`**. GitHub's `releases/latest` redirect then always resolves to the newest non-draft, non-prerelease build, so a printed QR keeps working.

## Publishing a build

Build the APK with Shorebird from the private Flutter repository (a regular `flutter build` APK cannot receive OTA patches):

```powershell
.\scripts\shorebird.ps1 release -Platform android -Artifact apk
Copy-Item build\app\outputs\flutter-apk\app-release.apk shreem.apk
```

Then publish it here:

```powershell
gh release create v1.0.0-build1 shreem.apk `
  --repo Khelit/tidegate-releases `
  --title "Shreem 1.0.0+1" `
  --notes "First Shorebird-enabled Android build."
```

Use pre-releases for test builds; `releases/latest` skips them.

## How updates work

- **Dart-only bug fixes** are delivered by Shorebird Code Push. Users do not reinstall and do not rescan the QR; the app downloads the patch and applies it the next time it starts.
- **Native code, plugins, assets, permission changes, or Flutter upgrades** require a new APK release here. Existing installs must install the new APK once.
- Never publish debug-signed or locally modified APKs. Use the artifact produced by the Shorebird release command.
