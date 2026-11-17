# Auto_GDGC_Updates

Public update channel for the Auto_GDGC Android app.

## Important setup

1. Do not upload `debug.keystore`, passwords, tokens, or private signing material to this public repository.
2. The currently installed debug APK can only be updated by an APK signed with the **same debug certificate**. Back up the exact keystore and obtain its passwords before moving to CI signing.
3. The included workflow expects a Gradle release signing configuration to read:
   - `SIGNING_STORE_FILE`
   - `SIGNING_STORE_PASSWORD`
   - `SIGNING_KEY_ALIAS`
   - `SIGNING_KEY_PASSWORD`
4. Add those values as repository Actions secrets before running the workflow.
5. The workflow assumes the Android app source is in this repository. It does not currently contain that source.

## Update manifest

`update.json` is a metadata example only. The app must implement a compatible checker, hash verification, APK download, and Android package installer flow before this manifest can update devices.

## Delta updates

A delta cannot be blindly applied to an APK by concatenating files. It requires a defined binary-diff format, exact base APK/version matching, reconstruction, SHA-256 verification, and signature verification. Keep `forceFullApk: true` until that pipeline is implemented and tested.
