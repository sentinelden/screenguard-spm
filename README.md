# ScreenGuard SDK for Swift Package Manager

ScreenGuardSDK as a Swift package: one library product backed by the signed binary
XCFramework. iOS 17 or later. Each Sentinel Den SDK has a package of its own,
so a project downloads only the SDKs it adds, and your licence has to cover
each one you add.

## 1. Give SwiftPM a download token

The binary is served to licensed accounts only. In your dashboard at
https://sentinelden.com/dashboard, create a download token under "Package
managers and CI" (it is shown once). SwiftPM signs in with your account email
as the user name and the token as the password. Add them to `~/.netrc` (Xcode
and the `swift` command both read it):

```
machine api.sentinelden.com
  login you@example.com
  password sdt_YOUR_DOWNLOAD_TOKEN
```

Your licence key does not work here: it ships inside your app, so it is not a
download credential.

Then `chmod 600 ~/.netrc`. On CI, write the same three lines from your secret
store before resolving packages.

Instead of `~/.netrc`, the `swift` command on macOS also reads an internet
password for `api.sentinelden.com` from the keychain.

## 2. Add the package

In Xcode: File > Add Package Dependencies, enter
`https://github.com/sentinelden/screenguard-spm`, and add the ScreenGuardSDK product.

In a `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/sentinelden/screenguard-spm", from: "2.7.0"),
],
targets: [
    .target(name: "App", dependencies: [
        .product(name: "ScreenGuardSDK", package: "screenguard-spm"),
    ]),
]
```

Then `import ScreenGuardSDK` and configure it as https://sentinelden.com/docs/screenguard
describes.

## Versions

Each tag of this package is a release of ScreenGuardSDK, and pins that release's
zip by its SHA-256 checksum. SwiftPM refuses a download whose checksum does
not match. Releases are listed at https://sentinelden.com/changelog.

## If resolving fails

- **401**: the email or download token in `~/.netrc` is wrong, the token was
  replaced or revoked in the dashboard, or the line is for a different host.
- **403**: the licence does not cover ScreenGuard, or it has lapsed. Check it in
  your dashboard at https://sentinelden.com/dashboard.
- **checksum mismatch**: tell us at https://sentinelden.com/contact; do not
  work around it.

Android SDKs install from the Sentinel Den Maven repository with the same
email and download token; see https://sentinelden.com/quickstart#android.
