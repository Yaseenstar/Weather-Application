# 🌤️ Weather Application

### A beautiful, feature-rich Flutter weather app with offline caching, live GPS, city search, smart notifications & AI weather assistant

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.10.4+-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Riverpod](https://img.shields.io/badge/Riverpod-State_Mgmt-00B4D8?style=for-the-badge)](https://riverpod.dev)
[![SQLite](https://img.shields.io/badge/SQLite-Offline_Cache-003B57?style=for-the-badge&logo=sqlite&logoColor=white)](https://pub.dev/packages/sqflite)
[![Gemini](https://img.shields.io/badge/Gemini-AI_Assistant-8E44AD?style=for-the-badge&logo=google&logoColor=white)](https://pub.dev/packages/flutter_gemini)
[![Version](https://img.shields.io/badge/Version-1.0.0-brightgreen?style=for-the-badge)](https://github.com/Talhaarif326/Weather-Application)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](https://github.com/Talhaarif326/Weather-Application/blob/main/LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://android.com)

---

## 📸 Screenshots

| Home Screen | Weekly Forecast | Forecast Expanded | Settings |
|---|---|---|---|
| ![Home](screenshots/home_screen.jpeg) | ![Weekly](screenshots/weekly_forecast.jpeg) | ![Expanded](screenshots/weekly_expanded.jpeg) | ![Settings](screenshots/settings.jpeg) |

---

## 🌟 Overview

A production-grade Flutter weather app that delivers real-time weather data using the **OpenWeatherMap OneCall 3.0 API**. Built with a clean Riverpod state management architecture, it works seamlessly **online and offline** — showing cached data with a "last updated" timestamp when there's no internet connection.

The app is designed around real user needs: it asks for your name once (first launch only), remembers your saved cities, lets you toggle between °C and °F across every screen instantly, and includes a built-in **AI weather assistant** powered by Google Gemini.

---

## ✨ Features

### 🌍 Location & Search
- **Live GPS** — auto-detects your location on launch with 3-attempt permission retry
- **City Search** — real-time dropdown suggestions as you type (powered by geocoding)
- **Manage Saved Locations** — save, view, and delete cities from the settings screen
- Saved cities load cached weather instantly, refresh from API when online

### 🌡️ Weather Data
- Current temperature, feels like, humidity, UV index, pressure, visibility
- **48-hour hourly forecast** with weather icons (SVG)
- **7-day weekly forecast** with expandable daily detail cards
- Sunrise & sunset times, wind data, and weather alerts
- All data from **OpenWeatherMap OneCall 3.0**

### 📦 Offline Caching
- Full weather response cached locally using **SQLite** (sqflite)
- Opens with last known data when offline — no blank screens
- Orange **"Offline — Last updated: X min ago"** banner shown automatically
- Cache auto-updates silently when internet is restored

### 🌡️ Temperature Unit Toggle
- Switch between **°C and °F** from Settings — no API re-fetch needed
- Uses a centralized `TempConverter` utility — consistent across all screens
- Toggle persists in state and updates every temperature display instantly

### 🔔 Smart Notifications
- Weather update notification with current city + temperature
- Weather alerts notification when active alerts are present in API response
- Toggle notifications and alerts independently from Settings
- Uses `flutter_local_notifications` ^17.2.0

### 🤖 AI Weather Assistant (Gemini)
- Dedicated **Weather Assistant** screen powered by **Google Gemini**
- Chat interface where users can ask natural-language questions about their weather
- On first open, automatically sends a weather summary request — no typing required
- Full current weather context (current conditions, 48-hour hourly, 7-day weekly) is sent to Gemini on the first message, so all follow-up questions stay in context without re-fetching
- Persistent **chat history** maintained throughout the session for multi-turn conversations
- Weather data passed directly from the Riverpod `weatherProvider` — always in sync with what's on screen
- Responses use familiar units (°C/°F, km/h) consistent with the rest of the app
- Glassmorphism-style input bar and bubble UI matching the app's design language
- Accessible from other screens via a navigation tap — opens with an automatic greeting

### 👤 First-Launch Welcome
- Name screen shown only once — stored in SQLite `users` table
- Subsequent launches skip straight to the home screen
- Your name displayed throughout the app

---

## 🛠️ Tech Stack

| Package | Version | Purpose |
|---|---|---|
| `flutter_riverpod` | ^3.2.1 | State management (StateNotifier) |
| `riverpod` | ^3.1.0 | Core Riverpod library |
| `sqflite` + `path` | ^2.3.3 / ^1.9.0 | SQLite offline caching & user data |
| `connectivity_plus` | ^6.0.3 | Internet connection detection |
| `geolocator` | ^14.0.2 | Live GPS location |
| `geocoding` | ^4.0.0 | City name ↔ coordinates conversion |
| `http` | ^1.6.0 | OpenWeatherMap API calls |
| `flutter_gemini` | latest | Google Gemini AI chat integration |
| `flutter_local_notifications` | ^17.2.0 | Push notifications |
| `flutter_dotenv` | ^6.0.0 | API key management via `.env` |
| `flutter_svg` | ^2.3.0 | SVG weather condition icons |
| `intl` | ^0.20.2 | Date & time formatting |
| `cupertino_icons` | ^1.0.8 | iOS-style icons |

---

## 🗄️ Database Schema

```sql
users           → id, name                            -- 1 row max (first launch only)
locations       → id, city_name, lat, lon, is_current -- GPS + saved cities
weather_cache   → id, location_id, json_data, last_updated
```

---

## 📁 Project Structure

```
lib/
├── core/
│   └── utils/
│       └── temp_converter.dart           # Kelvin → °C/°F conversion
├── database/
│   └── db_helper.dart                    # All SQLite logic (singleton)
├── models/
│   └── weather_model.dart                # App state model
├── providers/
│   ├── weather_provider.dart             # StateNotifier — GPS, API, cache, search
│   └── icons_colors_provider.dart        # Weather condition → icon/color mapping
├── screens/
│   ├── welcome_screen.dart               # First-launch name entry
│   ├── main_screen.dart                  # Bottom navigation host
│   ├── home_screen.dart                  # Current weather + search
│   ├── weather_screen.dart               # Weekly forecast
│   ├── gemini_screen.dart                # AI weather assistant (Gemini chat)
│   └── setting_screen.dart               # Preferences, locations, notifications
└── widgets/
    ├── chat_message_bubble.dart          # Chat bubble for Gemini screen
    ├── hours_card_widget.dart
    ├── weekly_weather_card_widget.dart
    ├── today_weather_detail_widget.dart
    └── ten_days_weather_detail_widget.dart
```

---

## 🔑 API Setup

This app uses two external APIs — both require keys.

### OpenWeatherMap
1. Sign up at [openweathermap.org](https://openweathermap.org/)
2. Go to **API Keys** in your account dashboard
3. Make sure **One Call API 3.0** is enabled for your key

### Google Gemini
1. Get an API key from [Google AI Studio](https://aistudio.google.com/)
2. Add it to your `.env` file (see below)

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/Talhaarif326/Weather-Application.git
cd Weather-Application
```

### 2. Create a `.env` file in the project root

```env
apiKey=your_openweathermap_api_key_here
geminiKey=your_google_gemini_api_key_here
```

### 3. Add notification permission (Android 13+)

In `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```

### 4. Enable core library desugaring

In `android/app/build.gradle`:

```groovy
compileOptions {
    isCoreLibraryDesugaringEnabled = true
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}

dependencies {
    coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.4")
}
```

### 5. Install dependencies & run

```bash
flutter pub get
flutter run
```

---

## 🔄 Data Flow

```
App Launch
└── Check SQLite users table
    ├── Name exists  → MainScreen (home)
    └── No name      → WelcomeScreen → save name → MainScreen

fetchWeather()
└── Check internet (connectivity_plus)
    ├── Offline → load SQLite cache → show offline banner
    └── Online  → GPS location (geolocator)
                   → OpenWeatherMap OneCall 3.0 API (http)
                   → cache response in SQLite (sqflite)
                   → update all screens via Riverpod state

GeminiScreen (AI Assistant)
└── Reads weatherProvider state (current + hourly + weekly)
    └── isClickedFromOtherScreen = true → auto-sends weather summary prompt
        └── First message bundles full weather context → Gemini API
            └── Follow-up messages use chat history (no re-fetch needed)
```

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. Fork the repo
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Make your changes and commit: `git commit -m "feat: add your feature"`
4. Push to your fork: `git push origin feat/your-feature`
5. Open a Pull Request

Please keep PRs focused — one feature or fix per PR.

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](https://github.com/Talhaarif326/Weather-Application/blob/main/LICENSE) for details.

---

## 👤 Author

**Talha Arif**
- GitHub: [@Talhaarif326](https://github.com/Talhaarif326)

---

Built with ❤️ using Flutter, OpenWeatherMap & Google Gemini

⭐ Star this repo if you found it useful!
