# Auto_GDGC_Updates

Public update channel for Auto_GDGC. This repository is intended to contain only update metadata and published APK release assets.

## Recommended repository layout

- Keep Android source code in a **separate private repository**.
- Keep this repository **public** so the installed app can download the manifest and APK release without a GitHub login.
- Never upload keystores, passwords, access tokens, or other secrets to this repository.

## Required configuration

In this public repository, configure:

### Actions variable

- `SOURCE_REPO`: full name of the private Android source repository, e.g. `owner/Auto_GDGC_Source`.

### Actions secrets

- `SOURCE_REPO_TOKEN`: fine-grained token with read-only Contents access to the private source repository.
- `DEBUG_KEYSTORE_BASE64`: Base64 encoding of the exact `~/.android/debug.keystore` used to sign the APK already installed on the phone.

Do not paste these values into issues, commits, chat, or source files.

## Debug signing compatibility

Android permits an app update only when the new APK is signed with the same signing certificate as the installed APK. If you currently install debug builds, preserve the original debug keystore. Android Studio normally keeps it at `~/.android/debug.keystore`.

The workflow restores that file to the standard location on the build runner and uses Gradle's debug signing configuration. This is suitable for a personal/testing app, not recommended for public distribution. Consider switching to a dedicated release keystore only when you can also migrate/reinstall the app or otherwise preserve signing continuity.

## Release workflow

After the source repo and secrets are configured:

1. Open **Actions**.
2. Select **Build and publish Auto_GDGC**.
3. Choose **Run workflow**.
4. Increase `version_code` for every release; it must be greater than the version installed on the device.
5. Enter a new `version` and release notes.

The workflow builds a complete signed APK, publishes it as a GitHub Release asset named `Auto_GDGC.apk`, computes its SHA-256, and updates `update.json`.

## Current limitation: full APK only

The current release workflow publishes a full APK. The `update.json` manifest is a server-side metadata file; the Android app must also implement the update checker, SHA-256 verification, download, and Android package-installer flow.

Delta/patch delivery is not enabled yet. It requires a defined patch format, an exact base-APK/version match, patch reconstruction, verification of the resulting APK hash and signing certificate, and a fallback to the full APK. Do not set a patch URL until that client-side implementation has been added and tested.
