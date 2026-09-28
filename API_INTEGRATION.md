# M3SB APON SDK integration

The project uses the M3SB APON iOS SDK **v1.1.0** (Signature v3) in `ThreeOneOSFive/helpers/APONLicenseSDK.swift`. The v1.1 client sends the runtime dylib count and names with `/api/sdk/verify` and `/api/sdk/check`, so the API can apply the package's configured injection policy. The app fails closed for patch application until a fresh server response explicitly returns `allow_inject: true`; restore operations remain available.

The project already has its own Swift license screen and activation flow. Therefore this integration uses the SDK's Swift layer and does not add the separate Objective-C tweak bridge/UI, which would duplicate the app's existing gate. The provided archive's Objective-C files also refer to headers not included in that archive; ask the SDK owner for a complete Objective-C package if that layer is required.

## Private credentials

Do **not** commit package credentials. `Info.plist` now reads `APON_PACKAGE_TOKEN` and `APON_HMAC_SECRET` from Xcode build settings; the values are not stored in this source tree.

### Local macOS build

```bash
export APON_PACKAGE_TOKEN='(set privately from the API owner)'
export APON_HMAC_SECRET='(set privately from the API owner)'
./build_unsigned.sh
```

### GitHub Actions

Add repository Actions secrets named `APON_PACKAGE_TOKEN` and `APON_HMAC_SECRET`. The `build.yml` and `build-api-ipa.yml` workflows pass them to Xcode without writing them to the repository.

The package is `ATH IPA` (package ID 106), using the owner-generated configuration for bundle ID `com.apple.mobile.MobileHouseArrest`. Keep the bundle ID and package configuration aligned with the API owner's dashboard. The API remains authoritative for license status and whether injection is allowed.

## Build validation

Build on macOS with Xcode. This sandbox is Linux and does not include `xcodebuild`, so the archive/IPA must be compiled and tested on macOS. A successful unsigned build still needs to be signed using the intended authorized deployment method before installation.
