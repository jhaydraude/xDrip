# Antigravity Command Toolbox

This is our living runbook for the xDrip+ stack. You MUST consult this toolbox before executing shell commands to avoid guessing syntax or running into known environmental deprecations.

## Gradle Builds (APK Generation)
If the project needs to be built natively via the terminal, use the `ProdDebug` variants to prevent Dagger/ProGuard errors on local terminals unless `Release` keys are explicitly provided.

### Compile wear module:
```powershell
.\gradlew :wear:assembleProdDebug
```

### Compile app module:
```powershell
.\gradlew :app:assembleProdDebug
```

> **Warning**: Gradle builds on Android Studio synced projects frequently contain mismatched AGP versions or `google-services` plugins. If you hit `Unsupported class file major version` (Java 21/25 mismatch) or `Failed to apply plugin 'com.google.gms.google-services'`, halt compilation and either ask the user to hit "Sync Project" in their IDE, or use `.properties` overrides.

## Git Operations
### Check working tree status:
```powershell
git status -s
```

### Safely stash Android Studio auto-updates before a code manipulation:
```powershell
git stash
# do operations
git stash pop
```

## IDE Sync Resets
If Android Studio configurations are blocking Gradle, direct the user to click "Sync Project with Gradle Files" (the elephant icon in their IDE) whenever a `build.gradle` file is altered manually.
