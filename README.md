::: {align="center"}
`<img src="app/src/main/res/drawable/lime_mascot.png" width="150" alt="lime pixel mascot">`{=html}

# `lime`

### **DISCOVER // SAVE // SHIP**

*A tiny pixel-control deck for archiving YouTube Shorts.*

`<br>`{=html}

![Android](https://img.shields.io/badge/Android-24%2B-11140D?style=flat-square&logo=android&logoColor=DFFF72&labelColor=DFFF72)
![Kotlin](https://img.shields.io/badge/Kotlin-Android-11140D?style=flat-square&logo=kotlin&logoColor=DFFF72&labelColor=DFFF72)
![Version](https://img.shields.io/badge/build-4.0-11140D?style=flat-square&labelColor=DFFF72&color=11140D)
![Status](https://img.shields.io/badge/status-beta-11140D?style=flat-square&labelColor=DFFF72&color=11140D)
:::

------------------------------------------------------------------------

## 🍋 What is LIME?

**LIME is a minimal Android client for a Shorts archiving pipeline.**

You connect your YouTube account, paste a public YouTube channel URL,
discover its Shorts, select the ones you want, and either:

-   **SAVE** them locally to the app's archive, or
-   **ADD TO UPLOAD QUEUE** so the backend can handle the upload
    workflow.

The idea is deliberately small:

``` text
DISCOVER
   ↓
SELECT
   ↓
SAVE  ───────────────→  local archive
   │
   └───────────────→  UPLOAD QUEUE
                           ↓
                      server handles it
```

LIME does **not** have a separate LIME account or user-login system.\
The only account connection is the YouTube OAuth connection needed for
YouTube operations.

------------------------------------------------------------------------

## ✦ The LIME philosophy

> **One job. Clean interface. No dashboard bloat.**

LIME is intentionally not trying to become a social network, analytics
suite, or complicated creator platform.

The interface is built around a small number of actions:

  -----------------------------------------------------------------------
  Area                                Purpose
  ----------------------------------- -----------------------------------
  `YOUTUBE`                           Connect and check the YouTube
                                      account

  `QUEUE`                             See waiting, processing, and daily
                                      upload status

  `DISCOVER`                          Scan a public channel for Shorts

  `SELECT`                            Pick individual Shorts or select
                                      everything

  `SAVE`                              Download selected Shorts to the
                                      local archive

  `UPLOAD QUEUE`                      Send selected Shorts to the
                                      server-side upload queue
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 🖥️ Interface

LIME uses a deliberately retro visual language:

``` text
┌─────────────────────────────────┐
│ !                         +     │
│                                 │
│             [ LIME ]            │
│                                 │
│  01 // YOUTUBE                  │
│  ┌───────────────────────────┐  │
│  │ ! YOUTUBE LINK     ACCOUNT│  │
│  │ ✓ YOUTUBE CONNECTED       │  │
│  │ [     LINK YOUTUBE →    ] │  │
│  └───────────────────────────┘  │
│                                 │
│  02 // QUEUE                    │
│  ┌───────────────────────────┐  │
│  │ DAILY LIMIT               │  │
│  │ 4 WAITING · 8 / 8 TODAY   │  │
│  │                           │  │
│  │  4        0        8 / 8  │  │
│  │ WAITING PROCESSING  TODAY │  │
│  └───────────────────────────┘  │
│                                 │
│  03 // DISCOVER                 │
│  [ SERVER // ONLINE ]           │
│  [ PUBLIC YOUTUBE CHANNEL URL ] │
│  [       SCAN CHANNEL →      ]  │
└─────────────────────────────────┘
```

### Design language

-   Lime-on-black contrast
-   Pixel/bitmap typography
-   Hard borders and compact cards
-   Minimal rounded controls
-   Pixel-art mascot
-   No unnecessary gradients or visual clutter
-   Information presented as a small **control deck**, not a traditional
    dashboard

The bundled typeface is `lime_pixel.ttf`, used throughout the UI for the
retro look.

------------------------------------------------------------------------

## 🔎 Discover

Paste a **full public YouTube channel URL** and press:

``` text
SCAN CHANNEL →
```

LIME requests Shorts from the backend in batches and builds a selectable
list.

Each discovered item shows:

-   Title
-   YouTube Short ID
-   Short URL
-   Selection state

The client also keeps a local archive list so previously downloaded
Shorts can be counted as archived.

------------------------------------------------------------------------

## 📦 Queue

The queue card is intentionally simple.

It surfaces the server's current:

``` text
WAITING
PROCESSING
UPLOADED TODAY / DAILY LIMIT
NEXT IN LINE
```

The current default daily limit reported by the backend is **8 uploads
per day**.

The limit is read from the queue response rather than being used only as
a visual counter in the app.

------------------------------------------------------------------------

## ⬇️ Save vs Upload Queue

These are two different workflows.

### `SAVE`

Downloads selected Shorts from:

``` text
POST /download
```

The resulting MP4 files are stored inside the app's external movie
storage under:

``` text
ShortsArchive/
```

Downloaded video IDs are recorded in:

``` text
ShortsArchive/download-archive.txt
```

### `+ ADD TO UPLOAD QUEUE`

This does **not** download the videos to the phone.

Instead, selected IDs are sent to:

``` text
POST /upload-queue/add
```

The server then owns the upload pipeline.

That distinction is intentional:

``` text
SAVE
→ phone gets the video

UPLOAD QUEUE
→ server gets the job
→ server downloads when ready
→ server uploads to YouTube
```

------------------------------------------------------------------------

## 🔐 YouTube connection

LIME does not create its own login.

For YouTube access it opens the backend OAuth flow:

``` text
GET /youtube/connect
```

Connection status is checked through:

``` text
GET /youtube/account
```

The app can also refresh the connection status and display whether
YouTube is connected.

------------------------------------------------------------------------

## 🌐 Backend API

The Android client currently talks to the configured backend using these
routes:

  Method   Endpoint              Used for
  -------- --------------------- ----------------------------
  `GET`    `/youtube/connect`    Start YouTube OAuth
  `GET`    `/youtube/account`    Check YouTube connection
  `POST`   `/discover`           Discover channel Shorts
  `GET`    `/upload-queue`       Read queue status
  `POST`   `/upload-queue/add`   Add Shorts to server queue
  `POST`   `/download`           Download a Short
  `POST`   `/youtube/upload`     Upload an archived video

The current fallback backend is:

``` text
https://shorts-archive-backend.onrender.com
```

The UI intentionally presents this as:

``` text
SERVER // ONLINE
```

rather than putting a long technical URL in front of the user.

------------------------------------------------------------------------

## 🧱 Project structure

``` text
lime/
├── app/
│   └── src/main/
│       ├── java/com/shortsarchiver/
│       │   ├── MainActivity.kt
│       │   └── SplashActivity.kt
│       │
│       └── res/
│           ├── drawable/
│           │   ├── lime_mascot.png
│           │   ├── button_dark.xml
│           │   ├── button_lime.xml
│           │   ├── card_lime.xml
│           │   └── ...
│           ├── font/
│           │   └── lime_pixel.ttf
│           ├── layout/
│           │   ├── activity_main.xml
│           │   └── activity_splash.xml
│           └── values/
│               ├── colors.xml
│               ├── strings.xml
│               └── themes.xml
│
├── build.gradle.kts
├── settings.gradle.kts
└── README.md
```

------------------------------------------------------------------------

## 🛠️ Build

Requirements:

-   Android Studio
-   JDK 17
-   Android SDK 36
-   Gradle/Android Gradle Plugin compatible with the project

Build a debug APK with:

``` bash
./gradlew assembleDebug
```

The generated APK is named:

``` text
lime-debug.apk
```

### Current Android configuration

``` text
applicationId: com.shortsarchiver
minSdk:        24
targetSdk:     36
compileSdk:    36
Kotlin/JVM:    17
View Binding:  enabled
```

------------------------------------------------------------------------

## 🎨 Visual tokens

  Token        Value
  ------------ -----------
  Background   `#DFFF72`
  Lime         `#DFFF72`
  Soft lime    `#C9E95E`
  Panel        `#E8FF93`
  Ink          `#11140D`
  Muted        `#46502C`

The mascot is a pixel-art lime and is used as the central visual
identity of the app.

------------------------------------------------------------------------

## 🚧 Current status

LIME is currently a **beta build** focused on the core archive workflow.

The priority is reliability, not adding more screens or features.

### Core flow

``` text
YouTube OAuth
     ↓
Discover channel
     ↓
Select Shorts
     ├──→ Save locally
     │
     └──→ Add to upload queue
                 ↓
             Server queue
                 ↓
             YouTube upload
```

------------------------------------------------------------------------

## 🍋 Why "lime"?

Because the app doesn't need a complicated name.

It's small.\
It's bright.\
It's a little weird.

**LIME.**

::: {align="center"}
### `DISCOVER // SAVE // SHIP`

**Built as a tiny tool, not a giant dashboard.**
:::
