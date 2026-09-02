<div align="center">

<img src="app/src/main/res/drawable/lime_mascot.png" width="120" alt="lime mascot"/>

# 🟢 lime

### _a pixel-punk control deck for archiving YouTube Shorts_

![platform](https://img.shields.io/badge/platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![version](https://img.shields.io/badge/version-4.0-DFFF72?style=for-the-badge&labelColor=11140D)
![minsdk](https://img.shields.io/badge/minSdk-24-11140D?style=for-the-badge&color=11140D)
![status](https://img.shields.io/badge/status-in%20dev-C9E95E?style=for-the-badge&labelColor=11140D)

```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░  DISCOVER → QUEUE → ARCHIVE  ░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```

</div>

---

## 🍋‍🟩 what is this

**lime** connects to a YouTube account, scans a public channel for Shorts,
lets you pick which ones matter, and queues them for download + re-upload
through your own backend. One tap to discover, one tap to save.

It's built as a dark lime-on-black "control deck" — bordered cards, chunky
pixel type, no-frills feedback. Function first, styled like a Game Boy menu.

---

## ✦ features

| | |
|---|---|
| 🔗 | Connect a YouTube account, view + refresh connection status |
| 🔎 | Scan any public channel URL for Shorts |
| ☑️ | Select individual Shorts or select all at once |
| ⬇️ | Download selected Shorts to the backend |
| ⬆️ | Queue + upload archived Shorts back to YouTube |
| 📊 | Live queue metrics — waiting / processing / uploaded today |
| 🟩 | Retro pixel UI, custom bitmap font, lime-on-ink palette |

---

## 🖥️ screens

```
┌──────────────────────────────┐
│   !          🍈          +   │
│              lime             │
│                               │
│  01 // YOUTUBE                │
│  ┌───────────────────────┐   │
│  │ ! YOUTUBE LINK  [ACC] │   │
│  │ Checking connection...│   │
│  │ [   LINK YOUTUBE  →  ]│   │
│  └───────────────────────┘   │
│                               │
│  02 // QUEUE                  │
│  03 // DISCOVER                │
└──────────────────────────────┘
```

---

## ⚙️ how it talks to your backend

lime is a **frontend only** — it drives an existing backend over HTTP.

**OAuth**
- App opens → `GET /youtube/connect`
- Backend must keep its callback at exactly `/oauth/callback`
  (not `/oauth2callback` — don't rename it)

**Endpoints used**

| Method | Route | Purpose |
|---|---|---|
| `GET` | `/youtube/connect` | start OAuth |
| `GET` | `/youtube/account` | connection status |
| `POST` | `/discover` | scan a channel for Shorts |
| `POST` | `/download` | download selected Shorts |
| `POST` | `/youtube/upload` | upload an archived Short *(new)* |

`/youtube/upload` expects multipart field `video`, plus optional
`title`, `description`, `privacy_status`.

> If `/youtube/upload` isn't live on your backend yet, everything else
> still works — upload alone will error until that route exists.

---

## 🛠️ build it

```bash
# open in Android Studio, then:
./gradlew assembleDebug
```

- `applicationId` — `com.shortsarchiver`
- `minSdk 24` · `targetSdk 36` · Kotlin + view binding
- No prebuilt Gradle wrapper is bundled — this is source-ready, not APK-ready
- CI: see `.github/workflows/build-apk.yml` for a GitHub Actions build

---

## 🎨 design system

| token | value |
|---|---|
| `lime` | `#DFFF72` |
| `lime_panel` | `#E8FF93` |
| `lime_soft` | `#C9E95E` |
| `ink` (text/borders) | `#11140D` |
| `muted` (secondary text) | `#46502C` |

Typography runs on a single bitmap pixel font (`lime_pixel.ttf`) across
every label, number, and button for a consistent 8-bit feel.

---

<div align="center">

**v4.0** · built for feeding a personal Shorts archive pipeline

</div>
