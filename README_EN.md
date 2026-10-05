# ReynardDefaultBrowser

English | [简体中文](./README.md)

A jailbreak tweak that turns [Reynard](https://github.com/minh-ton/reynard-browser) (a Gecko-based browser for iOS) into your **default browser** — links that would normally open in Safari are intercepted and handed to Reynard instead, with a toggle in Settings.

This repository is a fork of [guacforlife/ReynardDefault](https://github.com/guacforlife/ReynardDefault) that **fixes the upstream Settings crash on iPadOS 15.x** (tapping the ReynardDefault entry killed Settings, making it impossible to configure). The fix has also been submitted upstream: [PR #10](https://github.com/guacforlife/ReynardDefault/pull/10).

## Features

- **Safari redirect** — hooks `FBSystemServiceOpenApplicationRequest` in SpringBoard to swap open requests to Reynard
- **Per-app redirect sources** — in Reynard's own settings (**Settings → General → Default Browser Redirect**), check any installed app (TrollStore installs included): that app's web link opens go to Reynard. If an app depends on browser hand-offs (e.g. OAuth sign-ins), just uncheck it
- **Quick switches** — the system Settings list shows the master switch plus individual Safari / Chrome / Firefox / Brave switches (they edit the same data as the in-app picker)
- **Control Centre toggle** — quick on/off via CCSupport (optional, controls the master switch)

A redirect happens only when the master switch is on **and** the link's source app is checked **and** the link is http(s). Non-web app deep links are never touched. Safari is checked by default.

## Compatibility

- Deployment target **iOS 14.0+**; rootless and rootful variants; arm64 / arm64e
- The hooked API exists on iOS 13–16, and the inline switch renders on every iOS version PreferenceLoader supports — including the iPadOS split-view Settings where the upstream bundle crashed
- **Tested: iPad Pro (2021) · iPadOS 15.1 · rootless** ✅ (upstream 1.5.0 crashed on tap in this environment)

| Device | OS | Jailbreak | Variant | Status |
|--------|-----|-----------|---------|--------|
| iPad Pro (2021) | iPadOS 15.1 | rootless | rootless (`iphoneos-arm64`) | ✅ Working (upstream crashed here) |
| iPhone 14 Pro Max | 16.3.1 | Dopamine (Roothide) | rootless (`iphoneos-arm64`) | Working (tested upstream) |
| iPhone 7 | 15.7.7 | Dopamine | rootless (`iphoneos-arm64`) | Working (tested upstream) |
| iPhone 7 Plus | 14.8.1 | Taurine 1.1.7-3 | rootful (`iphoneos-arm`) | Working (tested upstream) |

> 💡 **Check before installing:** Reynard's jailbroken/TrollStore builds embed the `com.apple.developer.web-browser` entitlement, so on some iOS versions Reynard already appears natively under *Settings → Safari → Default Browser App* — if it's listed there, you don't need this tweak at all.

## Installing

Grab the deb for your jailbreak scheme from [Releases](https://github.com/TsangAsuna/ReynardDefaultBrowser/releases):

- `*_iphoneos-arm64_rootless.deb` — rootless (Dopamine / Roothide / palera1n rootless)
- `*_iphoneos-arm_rootful.deb` — rootful (unc0ver / palera1n rootful / Taurine)

Same package id as upstream (`com.guacforlife.reynarddefault`) with a higher version, so it **upgrades cleanly over upstream** and keeps your existing toggle state. After installing, enable the "ReynardDefault" switch in the main Settings list.

## Building

Requires [Theos](https://theos.dev/).

```bash
export THEOS=~/theos
make rootless   # Dopamine / Roothide
make rootful    # unc0ver / palera1n rootful
make both       # builds both variants side-by-side
```

GitHub Actions builds both variants on every push and publishes them to Releases on tags.

## License

GPL-3.0 — see [LICENSE](LICENSE). The original tweak was created by [guacforlife](https://github.com/guacforlife).
