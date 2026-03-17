# Assistive Ball Shortcut — OPPO Reno4 / ColorOS 12
## ⚡ Get the APK via GitHub Actions (no PC setup needed)

### Step 1 — Create a GitHub account
Go to https://github.com and sign up (free).

### Step 2 — Create a new repository
1. Click the **+** icon → **New repository**
2. Name it `assistive-ball-shortcut`, set it to **Public**, click **Create repository**

### Step 3 — Upload all project files (keep folder structure as-is)

### Step 4 — Go to the Actions tab → watch the build run (~3 min)

### Step 5 — Download the APK
Scroll to **Artifacts** at the bottom of the completed run → click **AssistiveBall-debug** → unzip → get `app-debug.apk`

### Step 6 — Install on Reno4
Settings → Additional Settings → enable **Install from Unknown Sources** → open the APK

---


A lightweight Android app that lets you quickly access or toggle the **Assistive Ball**
(floating virtual button) on OPPO Reno4 running Android 12.1 / ColorOS 12.

---

## Two Modes

### Mode 1 — Settings Shortcut (no setup needed)
Tap the app icon → jumps straight to the Assistive Ball settings page in ColorOS.
Toggle it on/off from there in one tap. Works out of the box, no root required.

### Mode 2 — True Toggle (requires one-time ADB command)
Silently flips Assistive Ball ON ↔ OFF with a single tap — no settings screen appears.

#### Setup (one time only):
1. Enable **Developer Options** on your phone:
   - Settings → About Phone → tap "Build Number" 7 times
2. Enable **USB Debugging** in Developer Options
3. Connect phone to PC via USB and run:
   ```
   adb shell pm grant com.example.assistiveball android.permission.WRITE_SECURE_SETTINGS
   ```
4. Done — the app now silently toggles the ball.

To use ToggleActivity as your default launcher shortcut, change `MainActivity` to `ToggleActivity`
in `AndroidManifest.xml` for the `android.intent.action.MAIN` intent filter.

---

## Project Structure

```
AssistiveBallShortcut/
├── app/
│   ├── build.gradle
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/example/assistiveball/
│       │   ├── MainActivity.kt       ← Settings deep-link mode
│       │   └── ToggleActivity.kt     ← Direct toggle mode (needs ADB grant)
│       └── res/values/
│           ├── strings.xml
│           └── themes.xml
└── build.gradle
```

---

## How It Works

### Settings Deep-Link
The app tries a sequence of known ColorOS component names for the Assistive Ball
settings screen:
- `com.oppo.assistantscreen/.settings.AssistantScreenSettingsActivity`
- `com.coloros.assistantscreen/.settings.AssistantScreenSettingsActivity`
- Falls back to general Accessibility Settings if none match

### Direct Toggle
Reads and writes `Settings.System` key `oppo_assistant_screen_enable` (or variants).
Requires `WRITE_SECURE_SETTINGS` permission, which only ADB (or root) can grant.

---

## Building

Open in Android Studio (Flamingo or newer), sync Gradle, and run on your device.
Minimum SDK: 31 (Android 12).

---

## Notes

- No root required for Mode 1
- Mode 2 needs a one-time ADB command but does NOT require permanent root
- The WRITE_SECURE_SETTINGS grant survives reboots but may need re-granting after
  factory reset or major OS updates
- If ColorOS updates change the setting key name, edit `SETTING_KEYS` in ToggleActivity.kt
