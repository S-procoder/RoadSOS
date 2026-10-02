# RoadSOS — Offline Crash Emergency Assistant

<div align="center">

<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=700&size=26&duration=2800&pause=700&color=FF1744&center=true&vCenter=true&width=950&lines=Crash+Emergency+Prototype;Smart+SOS+Assistant;Offline+First+Response" alt="RoadSOS Banner" />

<br/>

![Android](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge\&logo=android\&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-Jetpack%20Compose-7F52FF?style=for-the-badge\&logo=kotlin\&logoColor=white)
![Offline](https://img.shields.io/badge/Offline--First-Local%20Emergency%20Flow-00C853?style=for-the-badge)
![SQLite](https://img.shields.io/badge/Database-SQLite-2563EB?style=for-the-badge)
![Prototype](https://img.shields.io/badge/Status-Prototype-FF9800?style=for-the-badge)

</div>

---

## About This Fork

This repository is a maintained fork of the original RoadSOS project.

- Original Creator: Ashutosh Mishra
- Original Repository: https://github.com/ashu-mishra06/RoadSOS

All design credit, project ownership, and original development work belong to Ashutosh Mishra and the original project team. This fork is maintained for project continuity, documentation cleanup, and code upkeep without claiming ownership of the original project.

For the original project, please visit the upstream repository above.

---

## The Idea

**RoadSOS** is an Android emergency-response application designed to reduce the delay between a possible road accident and the first human response.

It listens for crash-like events, starts a false-alarm countdown, and if the user does not cancel, it moves into emergency mode.

From there, RoadSOS can send SOS messages to saved contacts, attach current or last-known location, optionally attempt an emergency call, show nearby offline emergency services, and save the event history for review.

```text
Detect → Countdown → Alert → Locate → Assist → Record
```

This project is focused on a practical emergency workflow for early assistance, while keeping the system clearly marked as a prototype rather than a certified emergency dispatch solution. 

Note : This is not a Certified Emergency Medical Tool this is a 
---

## Why This Matters

After an accident, the victim may be unconscious, injured, shocked, or unable to unlock the phone. In many cases, help is delayed not because help is unavailable, but because no one knows the accident has happened.

RoadSOS focuses on that first-response gap.

It is designed around three practical questions:

```text
Can the phone notice something unusual?
Can the user cancel if it is a false alarm?
Can the phone still send useful emergency context if the user cannot respond?
```

---

## Core Flow

<div align="center">

<pre>
Crash-like sound / Manual SOS
        ↓
Countdown starts
        ↓
User can cancel
        ↓
Emergency mode
        ↓
SMS to emergency contacts
        ↓
Current or last-known location
        ↓
Optional emergency call
        ↓
Offline nearby services
        ↓
Local emergency history
</pre>

</div>

---

## What Works in Prototype 1

| Module               | Implementation                                  |
| -------------------- | ----------------------------------------------- |
| Crash-like detection | Audio monitoring service with ML helper         |
| Manual SOS           | Large emergency button with countdown           |
| False alarm handling | User can cancel before emergency action         |
| Emergency contacts   | Multiple saved contacts supported               |
| SMS alert            | Automatic SMS attempt to saved contacts         |
| Auto-call            | Optional emergency call setting                 |
| Location fallback    | Current location or saved last-known location   |
| Offline help         | SQLite-based emergency service lookup           |
| History              | Local emergency event log                       |
| Profile              | Medical profile with editable details           |
| UI                   | Jetpack Compose with dark/light mode            |
| Language             | Multilingual support for major Indian languages |

---

## System Architecture

<div align="center">

<pre>
┌──────────────────────────────┐
│        Jetpack Compose UI     │
└───────────────┬──────────────┘
                 ↓
┌──────────────────────────────┐
│          ViewModels           │
│ Emergency / Location / Map    │
│ Profile / Settings / History  │
└───────────────┬──────────────┘
                 ↓
┌──────────────────────────────┐
│        Local Data Layer       │
│ DataStore + SQLite Assets DB  │
└───────────────┬──────────────┘
                 ↓
┌──────────────────────────────┐
│       Emergency Managers      │
│ SMS / Call / Status / History │
└───────────────┬──────────────┘
                 ↓
┌──────────────────────────────┐
│     Background Monitoring     │
│ Audio Service + ML Helper     │
└──────────────────────────────┘
</pre>

</div>

---

## Crash Detection Pipeline

<div align="center">

<pre>
AudioMonitoringService
        ↓
AudioRecorderManager
        ↓
Audio chunks
        ↓
TensorFlow helper
        ↓
Crash-like score
        ↓
EmergencyEventManager
        ↓
Emergency countdown
</pre>

</div>

The detection layer is intentionally described as **crash-like sound detection**, not certified crash detection. This keeps the system grounded and honest about its current prototype status.

---

## Location Strategy

Location is one of the most important parts of an emergency alert.

RoadSOS does not silently track users when location is off. Instead, it uses a fallback strategy:

<div align="center">

<pre>
Current location available
        ↓
Use current location

Current location unavailable
        ↓
Use saved last-known location

No saved location
        ↓
Send alert with location unavailable warning
</pre>

</div>

Emergency messages clearly label whether the location is current, recent last-known, old last-known, or unavailable.

---

## Emergency SMS

RoadSOS sends emergency SMS alerts to all saved emergency contacts.

Example SMS:

```text
RoadSOS EMERGENCY ALERT

Possible accident detected.

Name: Ashutosh
Blood Group: B+

Location Type: Recent last-known location
Location: https://maps.google.com/?q=23.xxxx,77.xxxx
Last updated: 6 minutes ago
Warning: location may be slightly old.

Please call the user immediately and contact emergency services if unreachable.
```

The goal is simple: give contacts enough information to act quickly.

---

## Offline Emergency Lookup

RoadSOS includes an offline SQLite database stored inside app assets.

The map screen can show nearby emergency services using current or last-known location.

Supported emergency categories include:

```text
Hospitals
Police stations
Vehicle repair services
Tow / puncture support
Other emergency-related services
```

This makes the app useful even when internet-based search is unavailable.

---

## Emergency History

Every completed emergency is stored locally.

RoadSOS records:

| Field             | Meaning                             |
| ----------------- | ----------------------------------- |
| Time              | When the emergency happened         |
| Trigger           | Manual SOS or automatic detection   |
| Location type     | Current, last-known, or unavailable |
| Coordinates       | Location used during emergency      |
| SMS status        | SMS attempt result                  |
| Call status       | Auto-call result                    |
| Auto-call setting | Whether auto-call was enabled       |

This creates a small audit trail for testing, debugging, and review.

---

## App Modules

```text
RoadSOS
│
├── Home
│   ├── SOS button
│   ├── Countdown overlay
│   ├── Protection status
│   └── SMS / call / location status
│
├── Map
│   ├── Emergency radar
│   ├── Offline service lookup
│   └── Navigation intents
│
├── Profile
│   ├── Medical details
│   ├── Multiple emergency contacts
│   └── Language selection
│
├── Settings
│   ├── Auto-call toggle
│   ├── Permission health
│   ├── Theme toggle
│   └── Emergency history access
│
└── History
    ├── SOS event logs
    ├── Trigger source
    └── Emergency action status
```

---

## Tech Stack

| Layer           | Technology                      |
| --------------- | ------------------------------- |
| Language        | Kotlin                          |
| UI              | Jetpack Compose                 |
| Architecture    | ViewModel-based MVVM style      |
| Local Storage   | DataStore                       |
| Offline DB      | SQLite asset database           |
| Audio Capture   | AudioRecord                     |
| ML Helper       | TensorFlow Lite helper          |
| Background Work | Foreground service              |
| Navigation      | Navigation Compose              |
| Location        | Android Location APIs           |
| SMS             | SmsManager                      |
| Call            | Android call intent             |
| Maps            | External map/navigation intents |

---

## Simplified Project Structure

```text
RoadSOS
│
├── app/src/main
│   ├── AndroidManifest.xml
│   ├── assets
│   │   ├── crash_model.tflite
│   │   ├── labels.csv
│   │   └── roadsos_india.db
│   │
│   └── java/com/example/roadsos
│       ├── MainActivity.kt
│       ├── navigation
│       ├── ui
│       │   ├── components
│       │   └── screens
│       │       ├── home
│       │       ├── map
│       │       ├── profile
│       │       ├── settings
│       │       ├── history
│       │       └── onboarding
│       │
│       ├── viewmodel
│       ├── data
│       │   ├── local
│       │   ├── profile
│       │   ├── settings
│       │   ├── location
│       │   ├── history
│       │   └── repository
│       │
│       ├── service
│       ├── ml
│       ├── audio
│       ├── sms
│       └── utils
```

---

## Required Permissions

```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.SEND_SMS" />
<uses-permission android:name="android.permission.CALL_PHONE" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MICROPHONE" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

---

## Getting Started

### Prerequisites

- **Android Studio** (latest version recommended)
- **JDK 11 or higher**
- **Android SDK 29+** (API level 29 or higher)
- **Android Device or Emulator** running Android 9+ (API 29+)

### Step 1: Clone the Repository

Clone from the original repository:

```bash
git clone https://github.com/ashu-mishra06/RoadSOS.git
cd RoadSOS
```

Or clone from this fork:

```bash
git clone https://github.com/S-procoder/RoadSOS.git
cd RoadSOS
```

### Step 2: Open in Android Studio

1. Open **Android Studio**
2. Select **File** → **Open** 
3. Navigate to the RoadSOS folder and select it
4. Wait for Gradle to sync (this may take a few minutes)

### Step 3: Sync Gradle

Once the project opens:

```text
Android Studio will prompt you to sync Gradle
→ Click "Sync Now" when prompted
→ Wait for the build to complete
```

### Step 4: Set Up an Android Device/Emulator

#### Option A: Use Android Emulator

1. Open **Device Manager** in Android Studio
2. Create a new Virtual Device (if you don't have one)
   - Device: Pixel 4 or similar
   - Android Version: 9+ (API 29+)
3. Start the emulator

#### Option B: Use a Physical Device

1. Connect your Android phone via USB
2. Enable **Developer Mode** on your phone:
   - Go to **Settings** → **About Phone**
   - Tap **Build Number** 7 times
   - Go back to **Settings** → **Developer Options**
   - Enable **USB Debugging**

### Step 5: Build and Run

1. Select your device/emulator from the top toolbar
2. Click the **Run** button (green play icon)
   - Or press `Shift + F10` on Windows/Linux or `Ctrl + R` on Mac
3. Wait for the app to build and install

You should see the RoadSOS app launch on your device/emulator.

---

## Testing the App

### First Launch

When you first open the app:

1. **Fill in Your Profile:**
   - Name
   - Blood group
   - Medical details (optional)

2. **Add Emergency Contacts:**
   - Add at least one contact's phone number
   - This is who will receive the SOS alert

3. **Grant Permissions:**
   - Audio recording
   - Location access
   - SMS sending
   - Phone calling
   - Notifications

4. **Verify Permission Status:**
   - Go to Settings tab
   - Check that all required permissions are enabled

### Testing Manual SOS

1. On the **Home** tab, tap the large **SOS** button
2. A countdown will start (typically 30 seconds)
3. **Test Cancel:** Tap the screen to cancel before the countdown ends
4. **Test Emergency Trigger:** Let the countdown complete
5. Check the app logs or **History** tab to see if SMS/call attempts were made

### Testing Crash Detection (Optional)

1. Keep the app running in the background
2. Play a loud crash-like sound near your phone's microphone
3. If detected, the app will show a countdown
4. Cancel or let it trigger an emergency alert

### Testing Location Fallback

1. Turn location **ON** and let the app run for a minute
2. The app will save your current location
3. Turn location **OFF**
4. Trigger an SOS alert
5. Check **History** to see if the saved location was used

### Offline Testing

1. Turn off **WiFi and Mobile Data**
2. Keep location services **ON**
3. Trigger an SOS alert
4. The app should still function and show nearby offline services from its local database

---

## Building a Debug APK

To create an APK file you can share or install on multiple devices:

```bash
./gradlew assembleDebug
```

The APK will be located at:

```text
app/build/outputs/apk/debug/app-debug.apk
```

You can then:
1. Transfer this APK to any Android device
2. Enable "Install from Unknown Sources" in Settings
3. Install and run the app

---

## Building a Release APK (Advanced)

For a release build (requires signing configuration):

```bash
./gradlew assembleRelease
```

---

## Troubleshooting

### Gradle Sync Issues

- Clear Gradle cache:
  ```bash
  ./gradlew clean
  ```
- Invalidate Android Studio cache: **File** → **Invalidate Caches** → **Invalidate and Restart**

### App Crashes on Launch

- Check that all required permissions are granted
- Check Android Studio **Logcat** for error messages
- Ensure your device/emulator is running Android 9+ (API 29+)

### Permissions Not Requested

- On Android 6+, the app should request permissions at runtime
- If not appearing, go to **Settings** → **Apps** → **RoadSOS** → **Permissions** and enable manually

### SMS Not Sending (Emulator)

- The Android Emulator cannot send real SMS
- To test SMS functionality, use a physical device or a service like Firebase Cloud Messaging

### Location Not Updating

- Ensure location is enabled on your device
- For emulator: Open **Extended Controls** → **Location** and set a test location

---

## Prototype Boundaries

RoadSOS is functional as a prototype, but it is not a certified emergency response system.

Known boundaries:

```text
Audio-only detection may cause false positives.
Some real crashes may not produce detectable sound.
SMS status means send attempt, not delivery confirmation.
Android background restrictions may affect monitoring on some phones.
Auto-calling must be handled carefully.
Location cannot be fetched if location is off and no last-known location exists.
Offline service quality depends on database quality.
```

---

## Responsible Design Choices

RoadSOS is built around safety-first assumptions:

```text
Do not alert instantly
→ Use countdown

Do not pretend old location is live
→ Label last-known location clearly

Do not depend fully on internet
→ Use local database and local storage

Do not silently track users
→ Respect location permissions

Do not rely on only one contact
→ Support multiple emergency contacts
```

---

## Demo Line

```text
RoadSOS is an offline-first Android crash emergency prototype that detects crash-like events locally, starts a false-alarm countdown, sends SOS alerts with current or last-known location, optionally calls emergency services, and helps users get immediate support even without internet access.
```

---

## Team Fuzeppers

| Member                | Role                                            |
| --------------------- | ----------------------------------------------- |
| Ashutosh Mishra       | Original creator, DB, Frontend, App Integration and Documentation |
| Satvik Jain           | ML and Presentation                             |
| Arpit Singh Bhadoriya | UI Integration                                  |
| Vivek Jangela         | Frontend and App Integration                    |

---

## Disclaimer

RoadSOS is a prototype built for learning, demonstration, and research.

It is **not a certified emergency response system** and should not be used as a replacement for official emergency services without real-world validation, legal review, regulatory approval, and safety testing.

---

## Copyright Notice

© 2026 Ashutosh Mishra / Team Fuzeppers. All rights reserved.

This project is publicly available for review, educational use, and project continuity. Unauthorized copying, redistribution, modification, commercial use, or claiming this project as your own is not permitted without written permission.

---

<div align="center">

### RoadSOS

**Because every second after a crash matters.**

<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=20&duration=2500&pause=600&color=22C55E&center=true&vCenter=true&width=760&lines=Detect.;Countdown.;Alert.;Locate.;Assist.;Record." alt="RoadSOS closing banner" />

</div>
