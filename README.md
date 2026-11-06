# GPS Spoofer for Android: fake GPS location without root

**GPS Spoofer** changes the GPS location your Android phone reports to apps. Pick a city or tap the map, walk with a joystick, follow a route, and hide the mock location from apps that check. No root, no Magisk, no Xposed.

- Website and download: https://stonerphysics.net/gpspoofer
- Google Play edition: https://play.google.com/store/apps/details?id=com.gpspoofer.app
- Android 8.0 and later

This repository is the public documentation: how Android mock location works, how apps detect it, and how GPS Spoofer is set up. The app itself is closed source.

## Editions

| | Play edition | Full edition |
|---|---|---|
| Price | $4.99 on Google Play | $10 once (no subscription) |
| Change GPS location, joystick, routes, speed | Yes | Yes |
| Hide mock location from apps (Hide from Apps, Container mode) | No | Yes |
| ADB stealth (hide Developer Options and the mock setting) | No | Yes |

## How Android mock location works

On an unrooted phone, an app can only supply a fake location if it is chosen under **Settings → Developer options → Select mock location app**. Android then marks every location from that app as mock:

- `Location.isMock()` on Android 12 (API 31) and later
- `Location.isFromMockProvider()` on older versions
- a `mockProvider` / `isMock` entry in `Location.getExtras()`

Apps can also read two system settings: `development_settings_enabled` (Developer options is on) and `mock_location` (a mock location app is set). Games, dating apps and family-tracking apps use these checks to reject fake locations.

More: [How to hide mock location on Android](https://stonerphysics.net/hide-mock-location-android)

## Setup (no root)

1. Install GPS Spoofer.
2. Turn on Developer options: Settings → About phone → tap **Build number** seven times.
3. Under **Select mock location app**, choose GPS Spoofer.
4. Search a city or tap the map, then press **Spoof**.

### Optional: ADB stealth (Full edition)

One time, from a computer with ADB:

```
adb shell pm grant com.gpspoofer.app android.permission.WRITE_SECURE_SETTINGS
```

This lets GPS Spoofer clear the `mock_location` and `development_settings_enabled` settings that apps read. It does not root the phone.

### Hiding the mock flag (Full edition)

- **Hide from Apps** patches the target app with [LSPatch](https://github.com/LSPosed/LSPatch) so `isMock()`, `isFromMockProvider()` and the extras report the location as real. The patched app is re-signed, so Google Sign-In does not work inside it.
- **Container mode** runs the target app inside a container that hands it the location without the mock flag. No patching; the app's data is separate from a normal install.

These are mechanisms, not a guarantee that a particular app cannot detect spoofing.

## FAQ

**Does a VPN change your GPS location?** No. A VPN changes your IP address. Apps that read GPS (Tinder, Life360, Snapchat Map, Pokémon GO) still see your real position. [IP vs GPS explained](https://stonerphysics.net/does-vpn-change-gps-location)

**Can you fake GPS without Developer options?** Not on an unrooted phone; you can turn it on once and hide it. [Details](https://stonerphysics.net/fake-gps-without-developer-options)

**Is a GPS spoofer illegal?** Radio GPS jammers and spoofing transmitters are illegal to make, sell or operate in the US (47 U.S.C. § 302a(b)). A phone location app transmits no radio signal. Many apps forbid spoofing in their terms. [Phone apps vs radio jammers](https://stonerphysics.net/is-gps-spoofing-illegal)

**Does it need root?** No.

## Guides

- [Fake GPS location on Android](https://stonerphysics.net/fake-gps-location-android)
- [Fake GPS on Android without root](https://stonerphysics.net/fake-gps-no-root-android)
- [Best fake GPS app for Android](https://stonerphysics.net/best-fake-gps-app-android)
- [Pokémon GO spoofer for Android](https://stonerphysics.net/pokemon-go-spoofer-android)
- [Freeze or fake your Life360 location](https://stonerphysics.net/life360-fake-location-android)
- [Change your Tinder location on Android](https://stonerphysics.net/change-tinder-location-android)

## Support

support@stonerphysics.net · [stonerphysics.net](https://stonerphysics.net)

Android and Google Play are trademarks of Google LLC. Pokémon GO, Tinder, Life360 and other names are trademarks of their owners; Stoner Physics LLC is not affiliated with them. Spoofing can break an app's terms of service and get an account suspended.
