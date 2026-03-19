Build, install, and launch an iOS app on the connected physical device.

## Usage
`/run-device <app>` where `<app>` is `neus` or `nudge`.

## App Configuration

| App | Scheme | Bundle ID | Project Path |
|-----|--------|-----------|--------------|
| neus | Neus | com.neus.app | ~/Dev/neus/Neus/Neus.xcodeproj |
| nudge | App (Dev) | com.hbklenterprises.nudge | ~/Dev/nudge-fullstack/mobile-app/ios/App/App.xcodeproj |

## Instructions

Parse `$ARGUMENTS` to determine which app to build. If empty or not one of `neus`/`nudge`, show an error with the valid app names and stop.

Then execute these steps in order, reporting success/failure at each step:

### Step 1 — Detect Device
Run `xcrun devicectl list devices --filter 'connectionProperties.transportType == "wired"' 2>&1` to find the connected iPhone.
Parse the UDID and device name from the output. If no wired device is found, fall back to:
- **Device**: iPhone Maalik
- **UDID**: `00008140-00166106260B001C`

### Step 2 — Build
Run `xcodebuild` with these flags:
```
xcodebuild \
  -project <PROJECT_PATH> \
  -scheme "<SCHEME>" \
  -destination "platform=iOS,id=<UDID>" \
  -configuration Debug \
  -allowProvisioningUpdates \
  build
```
Use a 10-minute timeout. If the build fails, show the relevant error output and stop.

### Step 3 — Find the .app
After a successful build, locate the built `.app` bundle. Run:
```
xcodebuild \
  -project <PROJECT_PATH> \
  -scheme "<SCHEME>" \
  -configuration Debug \
  -showBuildSettings \
  | grep -E '^\s*(BUILT_PRODUCTS_DIR|FULL_PRODUCT_NAME)\s'
```
Combine `BUILT_PRODUCTS_DIR/FULL_PRODUCT_NAME` to get the full `.app` path.

### Step 4 — Install
Run:
```
xcrun devicectl device install app --device <UDID> "<APP_PATH>"
```
If installation fails, show the error and stop.

### Step 5 — Launch
Run:
```
xcrun devicectl device process launch --device <UDID> <BUNDLE_ID>
```
Report whether the app launched successfully.
