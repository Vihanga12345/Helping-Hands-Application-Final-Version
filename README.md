## Helping Hands – Cross‑platform Flutter App

Helping Hands connects helpees (clients) with helpers (service providers) for household tasks in Sri Lanka. The app supports Helpee, Helper, and Admin roles, and runs on Web, Android, and Windows.

### Tech stack
- Flutter 3.35.x, Dart 3.9
- Supabase (auth, database, functions) – configured via `lib/config/environment.dart`
- Firebase (Messaging/Notifications – mobile only)
- Google Maps (via `google_maps_flutter`/web)
- State management: `provider`, routing with `go_router`

---

## 1) Project layout
- App root: `helping_hands_app/`
  - Entry point: `lib/main.dart`
  - Routes: `lib/services/navigation_service.dart`
  - Config and env: `lib/config/environment.dart`
  - Pages: `lib/pages/**`
  - Services: `lib/services/**`
  - Android configs: `android/` (place `google-services.json` here)
  - Web entry: `web/`

---

## 2) Prerequisites
- Flutter SDK installed and on PATH (stable channel)
- For Web: Google Chrome
- For Android builds: Android Studio + SDK + a connected device or emulator
- For Windows desktop builds: Visual Studio with “Desktop development with C++” workload

Check your setup:
```bash
flutter doctor
```

---

## 3) Quick start (run the app)
From the repository root:
```bash
cd helping_hands_app
flutter pub get
flutter run -d chrome
```

Other targets (optional):
- Windows desktop (requires Visual Studio):
```bash
flutter run -d windows
```
- Android (device/emulator required):
```bash
flutter run -d android
```

Tips:
- If Chrome isn’t detected, run `flutter devices` to verify.
- If Windows build complains, install Visual Studio + C++ workload.

---

## 4) Environments and configuration
The app reads environment values via `lib/config/environment.dart` using Dart defines. Defaults for development are provided so the app runs out-of-the-box.

Common defines (only needed when you want to override defaults or switch env):
- `ENVIRONMENT` = `development` | `staging` | `production`
- Supabase:
  - `SUPABASE_URL_DEV` / `SUPABASE_URL_STAGING` / `SUPABASE_URL_PROD`
  - `SUPABASE_ANON_KEY_DEV` / `SUPABASE_ANON_KEY_STAGING` / `SUPABASE_ANON_KEY_PROD`
- Firebase:
  - `FIREBASE_API_KEY_DEV` / `FIREBASE_API_KEY_STAGING` / `FIREBASE_API_KEY_PROD`
  - `FIREBASE_PROJECT_ID` (optional; dev default exists)
- Google Maps:
  - `GOOGLE_MAPS_API_KEY_DEV` / `..._STAGING` / `..._PROD`

Example (Chrome, staging):
```bash
flutter run -d chrome \
  --dart-define=ENVIRONMENT=staging \
  --dart-define=SUPABASE_URL_STAGING=YOUR_STAGING_URL \
  --dart-define=SUPABASE_ANON_KEY_STAGING=YOUR_STAGING_ANON_KEY \
  --dart-define=FIREBASE_API_KEY_STAGING=YOUR_FIREBASE_KEY \
  --dart-define=GOOGLE_MAPS_API_KEY_STAGING=YOUR_MAPS_KEY
```

Android Firebase setup:
- Ensure `android/app/google-services.json` exists (already included for development). If you change Firebase projects, replace this file accordingly.

Web build note:
- If you face WebAssembly interop issues on web, try a plain JS/web build:
  - PowerShell (temporary for the session):
    ```powershell
    $env:FLUTTER_WEB_USE_SKWASM="false"
    flutter run -d chrome
    ```

---

## 5) Accessing user instances (roles)
The app supports three roles with separate flows. You can navigate via the app UI or directly to routes when running on web.

Start flow (all users):
- `/#/` → Intro screens → User selection

Helpee:
- Auth: `/#/helpee-auth`
- Login: `/#/helpee-login`
- Register: `/#/helpee-register`
- Home: `/#/helpee/home`
- Menu: `/#/helpee/menu`

Helper:
- Auth: `/#/helper-auth`
- Login: `/#/helper-login`
- Register (multi-step): `/#/helper-register`, `/#/helper-register-2`, `/#/helper-register-3`
- Home: `/#/helper/home`
- Menu: `/#/helper/menu`

Admin:
- Login: `/#/admin/login`
- Home: `/#/admin/home`
- Management (examples):
  - Jobs Pending: `/#/admin/jobs/pending`
  - Jobs Ongoing: `/#/admin/jobs/ongoing`
  - Jobs Completed: `/#/admin/jobs/completed`
  - Users (Helpers): `/#/admin/users/helpers`
  - Users (Helpees): `/#/admin/users/helpees`
  - Analytics: `/#/admin/analytics`
  - Reports: `/#/admin/reports`

Credentials:
- For development, you can register Helpee/Helper accounts via the app.
- Admin accounts must exist in your Supabase database according to your schema. Ask your team for admin credentials or create one in your backend.

---

## 6) Building the Android APK
From the app directory:
```bash
cd helping_hands_app
flutter clean
flutter pub get
flutter build apk --release
```

The generated APK will be at:
```
build/app/outputs/flutter-apk/app-release.apk
```

Signing (optional but recommended for distribution):
1. Create a keystore (one-time):
   - Windows PowerShell:
     ```powershell
     keytool -genkey -v -keystore C:\keystores\helpinghands.jks -alias helpinghands -keyalg RSA -keysize 2048 -validity 10000
     ```
2. Create `android/key.properties`:
   ```
   storeFile=C:\keystores\helpinghands.jks
   storePassword=YOUR_STORE_PASSWORD
   keyAlias=helpinghands
   keyPassword=YOUR_KEY_PASSWORD
   ```
3. Rebuild release APK:
   ```bash
   flutter build apk --release
   ```

---

## 7) Building for Web
```bash
cd helping_hands_app
flutter build web
```
Output: `build/web/` (can be hosted on any static site host). If you hit WebAssembly/interop issues, see the Web note in Section 4.

---

## 8) Useful scripts and commands
- Fetch deps: `flutter pub get`
- Analyze/lints: `flutter analyze`
- Run unit/widget tests: `flutter test`
- List devices: `flutter devices`

---

## 9) Troubleshooting
- Visual Studio not installed (Windows desktop builds): install VS + Desktop C++ workload.
- Android build issues: run `flutter doctor`, accept licenses in Android SDK Manager; ensure a device/emulator is connected.
- Web build errors about `dart:html`/`dart:js`: temporarily disable SKWASM as shown above.
- Env errors in console at app start: set the required `--dart-define` values or stick to `development` defaults.

---

## 10) Contact / Ownership
- Project owner: Vihanga
- Footer text in menus shows: “By Vihanga”.

---

Happy building!
