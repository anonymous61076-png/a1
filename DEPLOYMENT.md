# HomeopathyOne – Production Deployment & Release Guide

Comprehensive deployment guide for releasing the **HomeopathyOne** cross-platform mobile application to the **Google Play Store** (Android SDK 21+) and **Apple App Store** (iOS 12+).

---

## 1. Pre-Deployment Configuration Checklist

- [ ] **Environment Keys**:
  - Copy `.env.example` to `.env` or set `--dart-define` parameters during CI/CD.
  - Verify live `RAZORPAY_KEY_ID` (for India INR transactions).
  - Verify live `STRIPE_PUBLISHABLE_KEY` (for international transactions).
  - Confirm `EXCHANGE_RATE_API_URL` and `API_BASE_URL`.
- [ ] **Push Notifications (FCM)**:
  - Place `google-services.json` in `android/app/`.
  - Place `GoogleService-Info.plist` in `ios/Runner/`.
- [ ] **Android Keystore**:
  - Generate release keystore:
    ```bash
    keytool -genkey -v -keystore upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias homeopathyone_release_key
    ```
  - Create `android/key.properties` from `android/key.properties.example`.
- [ ] **iOS Provisioning & Signing**:
  - Configure Apple Developer Team ID and App Store Provisioning Profile in `ios/ExportOptions.plist`.

---

## 2. Generating Release Builds

### A. Android App Bundle (`.aab`) for Google Play Console
```powershell
# Windows PowerShell
.\scripts\build_android_release.ps1

# Linux / macOS
chmod +x ./scripts/build_android_release.sh
./scripts/build_android_release.sh
```
- **Output Artifact**: `build/app/outputs/bundle/release/app-release.aab`
- **Optimizations Applied**:
  - R8 Code Minification & Resource Shrinking (`minifyEnabled true`, `shrinkResources true`).
  - Proguard obfuscation rules ([android/app/proguard-rules.pro](android/app/proguard-rules.pro)).
  - Debug symbol extraction (`--split-debug-info`).

### B. iOS Archive (`.ipa`) for Apple App Store / TestFlight
```bash
chmod +x ./scripts/build_ios_release.sh
./scripts/build_ios_release.sh
```
- **Output Artifact**: `build/ios/ipa/homeopathy_one.ipa`
- Upload via Transporter or Xcode Organizer.

---

## 3. Play Store & App Store Compliance

### Android (SDK 21+ to 34):
- Target SDK is configured to **34** (Android 14 ready).
- MultiDex enabled for broad Android 5.0+ (API 21) backward compatibility.
- Permissions requested: `INTERNET`, `CAMERA`, `RECORD_AUDIO`, `ACCESS_FINE_LOCATION`, `POST_NOTIFICATIONS`.

### iOS (iOS 12.0+):
- Privacy usage strings provided in `ios/Runner/Info.plist`:
  - `NSCameraUsageDescription` (Video consultations).
  - `NSMicrophoneUsageDescription` (Voice / Video consultations).
  - `NSLocationWhenInUseUsageDescription` (Auto-currency detection).
  - `NSPhotoLibraryUsageDescription` (Prescription and report uploads).

---

## 4. Error Logging & Monitoring

- **Crashlytics**: `CrashlyticsService.instance` traps framework and platform-level uncaught exceptions.
- **Custom Error Boundary**: `CustomErrorBoundary` catches layout render exceptions in release builds to prevent user-facing crashes.
- **Specialized Monitoring**: Automatic logging for payment gateway timeouts and currency fallback activations.
