# ReynardDefault

A jailbreak tweak that redirects Safari URL opens to [Reynard](https://github.com/minh-ton/reynard-browser), a Firefox-based browser for iOS.

When enabled, any link that would normally open in Safari is intercepted at the SpringBoard level and opened in Reynard instead.

> **About this fork (TsangAsuna/ReynardDefault):** the upstream preference pane crashed the Settings app on iPadOS 15.1 (tapping the ReynardDefault entry killed Settings, making it impossible to configure). This fork removes the preference **bundle** entirely and ships the toggle as a native inline switch in the Settings list instead — tapping it no longer loads any bundle, so the crash path is gone. Same redirect mechanism, same preference suite (`enabled` in `com.guacforlife.reynarddefaultprefs`), same Control Centre toggle. Install v1.5.1+ to upgrade over upstream (same package id).

## Features

- **Safari redirect** — hooks `FBSystemServiceOpenApplicationRequest` in SpringBoard to swap the bundle identifier
- **Settings toggle** — a crash-proof inline `PSSwitchCell` in the main Settings list (no preference bundle, nothing to load when tapped)
- **Control Centre toggle** — quick toggle via CCSupport module (optional)

## Requirements

- A jailbroken iOS device — supports **rootless** (Dopamine/Roothide) and **rootful** (unc0ver, palera1n rootful)
- [Reynard](https://github.com/minh-ton/reynard-browser) browser installed
- [PreferenceLoader](https://github.com/PoomSmart/PreferenceLoader) (for the Settings switch)
- [CCSupport](https://moreinfo.thebigboss.org/moreinfo/depiction.php?file=ccsupportDp) (optional, only for the Control Centre toggle)

## Installing

Grab the deb for your jailbreak scheme from [Releases](https://github.com/TsangAsuna/ReynardDefault/releases):

- `*_iphoneos-arm64.deb` — rootless (Dopamine, Roothide, palera1n rootless)
- `*_iphoneos-arm.deb` — rootful (unc0ver, palera1n rootful, Taurine)

> **Bonus:** Reynard's Jailbroken/TrollStore builds already ship the `com.apple.developer.web-browser` entitlement. On some iOS versions this alone makes Reynard appear in *Settings → Safari → Default Browser App*. Check there first — if Reynard is listed, you can set it natively and may not need this tweak at all.

## Building

Requires [Theos](https://theos.dev/).

```bash
export THEOS=~/theos
cd ReynardDefault
make rootless   # Dopamine / Roothide
make rootful    # unc0ver / palera1n rootful
make both       # builds both variants side-by-side
```

Builds land in `packages/` as `*_iphoneos-arm64.deb` (rootless) and `*_iphoneos-arm.deb` (rootful). Rootless debs installed via Sileo on Roothide are auto-patched; manual installs need the Roothide Patcher app first.

GitHub Actions builds both variants automatically on every push (see `.github/workflows/build.yml`).

## Tested Environments

| Device | iOS | Jailbreak | Variant | Status |
|--------|-----|-----------|---------|--------|
| iPhone 14 Pro Max | 16.3.1 | Dopamine (Roothide) | rootless (`iphoneos-arm64`) | Working |
| iPhone 7 | 15.7.7 | Dopamine | rootless (`iphoneos-arm64`) | Working |
| iPhone 7 Plus | 14.8.1 | Taurine 1.1.7-3 | rootful (`iphoneos-arm`) | Working |
| iPad (split-view Settings) | 15.1 | rootless | rootless (`iphoneos-arm64`) | Settings crash fixed in v1.5.1 (upstream crashed on tap) |

Tweak injection via ElleKit (rootless) or substrate (rootful) on arm64 / arm64e.

## License

This project is licensed under the GNU General Public License v3.0. See [LICENSE](LICENSE) for details.
