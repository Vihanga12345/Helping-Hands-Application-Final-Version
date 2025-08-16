# Helping Hands - Flutter Mobile Application

A comprehensive Flutter mobile application that connects people who need help with skilled helpers in their community.

## Overview

Helping Hands is a mobile platform designed to bridge the gap between people who need assistance and skilled helpers who can provide services. The app features separate interfaces for helpers and helpees, along with a comprehensive job matching system.

## Features

### For Helpees (People Seeking Help)
- **User Registration & Authentication** - Secure account creation and login
- **Job Request Creation** - Post detailed job requests with requirements
- **Helper Search & Discovery** - Browse and filter available helpers
- **Real-time Job Tracking** - Track job status from pending to completion
- **Payment Integration** - Secure payment processing
- **Rating & Review System** - Rate helpers after job completion
- **Calendar Integration** - Schedule and manage appointments
- **AI Assistant** - Get help and guidance through AI-powered chat

### For Helpers (Service Providers)
- **Multi-step Registration** - Comprehensive profile setup with skills and experience
- **Job Request Management** - Browse, accept, and manage job requests
- **Profile Management** - Maintain professional profiles with portfolio
- **Calendar Management** - Schedule and track appointments
- **Activity Tracking** - Monitor pending, ongoing, and completed jobs
- **Rating System** - Build reputation through customer reviews
- **Private & Public Requests** - Handle both direct and open market requests

### Common Features
- **Real-time Notifications** - Stay updated on job status changes
- **In-app Messaging** - Communicate directly within the platform
- **Activity History** - Complete job history and analytics
- **Support System** - Comprehensive help and support features

## Technology Stack

- **Framework**: Flutter 3.x
- **Language**: Dart
- **State Management**: Provider
- **UI Components**: Custom Material Design components
- **Authentication**: Firebase Auth (planned)
- **Database**: Supabase (planned)
- **Push Notifications**: Firebase Cloud Messaging (planned)

## Project Structure

```
lib/
├── main.dart                 # Application entry point
├── pages/
│   ├── common/              # Shared pages (intro, auth, etc.)
│   ├── helpee/              # Helpee-specific pages
│   └── helper/              # Helper-specific pages
├── widgets/
│   ├── common/              # Shared UI components
│   ├── popups/              # Modal and popup components
│   └── ui_elements/         # Reusable UI elements
├── services/                # API and business logic services
├── models/                  # Data models
├── utils/                   # Utility functions and constants
└── constants/               # App-wide constants
```

## Current Development Status

✅ **Phase 1 Complete**: Full UI/UX Implementation
- All 81 pages implemented with complete UI
- Consistent design system across the application
- Responsive layouts for various screen sizes
- Custom animations and transitions

🔄 **Phase 2 In Progress**: Backend Integration
- Supabase database setup
- Firebase authentication integration
- Real-time features implementation
- API integration

## Pages Implemented

### Common Pages (5/5)
- Start Page
- Intro Pages (1-3)
- User Selection Page

### Helpee Pages (28/28)
- Authentication & Registration
- Home & Navigation
- Job Management (Request, Search, Tracking)
- Profile Management
- Payment System
- Support & Help

### Helper Pages (28/28)
- Multi-step Registration Process
- Home & Dashboard
- Job Request Management
- Profile & Portfolio Management
- Calendar & Activity Tracking
- Rating & Review System

### Popup Notifications (20/20)
- Account management popups
- Job status update notifications
- Payment confirmations
- System notifications

## Getting Started

### Prerequisites
- Flutter SDK (3.0.0 or higher)
- Dart SDK (2.17.0 or higher)
- Android Studio / VS Code
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Vihanga12345/helping-hands-flutter-app.git
   cd helping-hands-flutter-app
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Run the application**
   ```bash
   flutter run
   ```

### Development Setup

1. **Check Flutter installation**
   ```bash
   flutter doctor
   ```

2. **Enable development mode**
   ```bash
   flutter config --enable-web
   ```

3. **Run tests**
   ```bash
   flutter test
   ```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Development Guidelines

- Follow Flutter/Dart best practices
- Maintain consistent code formatting using `dart format`
- Write comprehensive commit messages
- Update documentation for new features
- Test thoroughly before submitting PRs

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

Project maintainer: [Vihanga12345](https://github.com/Vihanga12345)

## Acknowledgments

- Flutter team for the amazing framework
- Material Design for UI/UX guidelines
- Contributors and testers who helped improve the application

---

**Note**: This application is currently in active development. Some features may not be fully functional until backend integration is complete. 