# HomeopathyOne Flutter App

A comprehensive homeopathic health companion app built with Flutter.

## Architecture

Clean Architecture with feature-first folder layout:

```
lib/
├── core/           # App-wide utilities, theme, router, DI, constants
├── domain/         # Entities, repository contracts, use-cases
├── data/           # Data sources (remote/local), models, repository impls
├── presentation/   # Feature UI: screens, widgets, Riverpod providers
└── services/       # Platform services (notifications, analytics, etc.)
```

## Tech Stack

| Concern | Package |
|---|---|
| State Management | flutter_riverpod |
| Navigation | go_router |
| Networking | dio |
| Local Storage | hive_flutter, shared_preferences |
| Secure Storage | flutter_secure_storage |
| Authentication | firebase_auth, google_sign_in |
| Payments | flutter_stripe, razorpay_flutter, pay |
| UI | flutter_svg, cached_network_image, lottie, shimmer |
| DI | get_it |
| FP utilities | dartz |
| Code Gen | freezed, json_serializable |

## Getting Started

```bash
flutter pub get
flutter pub run build_runner build --delete-conflicting-outputs
flutter run
```

## Flavours

| Flavour | Bundle ID | Notes |
|---|---|---|
| debug | in.homeopathyone.app.debug | Dev / staging |
| release | in.homeopathyone.app | Production |

## Contributing

Follow the Clean Architecture conventions in `lib/`. Run `flutter analyze` and `flutter test` before submitting PRs.
