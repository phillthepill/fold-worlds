# Fold Worlds

Living pixel-art worlds: weather, water, animals and day/night, all simulated and touchable.

- **Home-screen app (iPhone, iPad, any phone):** https://phillthepill.github.io/fold-worlds/
  Open it in Safari → Share → **Add to Home Screen**. It opens full screen and works offline.
- **Wallpapers:** https://phillthepill.github.io/fold-worlds/wallpapers.html
  Stills of every world every two hours, a Shortcut recipe that changes your wallpaper with the time of day,
  and short loops for Live Photo lock screens.
- **Android live wallpaper:** built separately (Fold Worlds APK).
- **iOS app:** `ios-app/`, built by GitHub Actions (`.github/workflows/ios.yml`).

## iOS app: how it gets built

Every push that changes `ios-app/` or `docs/index.html` (or a manual run from the Actions tab) builds the app on a Mac runner:

1. **Unsigned build (always).** Checks that it compiles and publishes `FoldWorlds-unsigned.ipa` on the
   [`ios-latest` release](../../releases/tag/ios-latest). Install it with **Sideloadly** or **AltStore** on a computer,
   signing with your own free Apple ID (free-ID installs need re-signing every 7 days).
2. **TestFlight (once set up).** With a paid Apple Developer account ($99/yr), add these repository secrets
   (Settings → Secrets and variables → Actions) and every build is signed and uploaded to TestFlight automatically:

   | Secret | Where it comes from |
   |---|---|
   | `APPLE_TEAM_ID` | developer.apple.com → Membership → Team ID |
   | `ASC_KEY_ID` | App Store Connect → Users and Access → Integrations → App Store Connect API → new key (role: App Manager) |
   | `ASC_ISSUER_ID` | Same page, "Issuer ID" |
   | `ASC_KEY_P8_BASE64` | The downloaded `AuthKey_XXXX.p8`, base64-encoded (`base64 -i AuthKey_XXXX.p8`) |

   Before the first upload, create the app in App Store Connect (My Apps → + → New App) with bundle ID
   `net.foldworlds.app` (or set a repository variable `BUNDLE_ID` to your own). Then install **TestFlight** on the
   iPhone and accept the build.

The iOS app shows the same page as the home-screen app, bundled inside it, full screen, and keeps the screen awake
while it's open.

## Layout

- `docs/`: the website (GitHub Pages): `index.html` (the worlds), `wallpapers.html`, `wall/<world>/<hour>.jpg`, `loops/<world>.mp4`.
- `ios-app/`: Swift app (XcodeGen `project.yml`).
