# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Knock Flutter SDK - a client-side Flutter library to interact with user-facing Knock features, such as in-app notification feeds and push notifications. The library provides cross-platform support for iOS (APNS) and Android (FCM) push notifications.

## Key Architecture

### Core Components

- **lib/src/knock.dart**: Main SDK entry point, manages authentication and client initialization
- **lib/src/api_client.dart**: HTTP API client for Knock REST endpoints
- **lib/src/feed_client.dart**: Real-time WebSocket feed client using Phoenix channels
- **lib/src/preferences_client.dart**: Manages user notification preferences
- **lib/src/user_client.dart**: User management and authentication

### Platform Integration

- **ios/Classes/**: Swift implementation for iOS-specific features (APNS)
- **android/src/**: Kotlin implementation for Android-specific features (FCM)
- **pigeons/messages.dart**: Platform channel message definitions using Pigeon code generation

### Data Models

All models use Freezed for immutable objects and JSON serialization:
- Located in `lib/src/model/`
- Generated files: `*.freezed.dart` and `*.g.dart`

## Common Development Commands

### Build and Code Generation

```bash
# Generate Freezed models and JSON serialization
flutter pub run build_runner build

# Generate platform channel messages (Pigeon)
flutter pub run pigeon --input pigeons/messages.dart

# Install dependencies
flutter pub get

# Analyze code
flutter analyze

# Run tests
flutter test

# Run a specific test file
flutter test test/src/api_client_test.dart
```

### Example App Commands

```bash
# Run example app on iOS simulator
cd example
flutter run -d ios

# Run example app on Android emulator
cd example
flutter run -d android
```

## Testing Approach

- Unit tests located in `test/` directory mirroring `lib/` structure
- Mock generation using Mockito (see `test/src/mocks.dart`)
- Test utilities for date/time and retry logic in `test/src/util/`

## Push Notification Setup

### iOS (APNS)
- Requires push notification capability enabled in Xcode
- Development team and bundle identifier configuration needed
- AppDelegate.swift handles push registration

### Android (FCM)
- Requires Firebase project setup
- Add google-services.json to android/app/
- Gradle configuration for Google Services plugin

## Release Process

1. Update CHANGELOG.md with release notes
2. Update version in pubspec.yaml
3. Create PR for review
4. After merge, use Slack command: `/release status knock-flutter`

## Important Notes

- Generated files (*.g.dart, *.freezed.dart, Messages.g.*) are checked into version control
- Uses `very_good_analysis` for linting with custom rules in analysis_options.yaml
- API keys must be public keys (starting with 'pk_'), not secret keys
- WebSocket connections use Phoenix channels for real-time updates