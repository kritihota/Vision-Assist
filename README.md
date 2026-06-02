# Vision Assist

A mobile accessibility app that uses your phone's camera to describe your surroundings and read the description aloud — designed for visually impaired users.

Point the camera, tap **Scan Surroundings**, and the app speaks a clear, concise description of the scene in front of you.

---

## Features

- **AI scene description** — sends a camera snapshot to Google Gemini and receives a 4–6 sentence description covering subjects, background, lighting, and any visible text
- **Text-to-speech** — reads the description aloud using the native iOS voice, one phrase at a time in order
- **Accessible UI** — large tap targets, `accessibilityRole` and `accessibilityLabel` on every interactive element, screen-reader friendly layout
- **Graceful fallbacks** — if Gemini is unavailable the app surfaces a clear error message rather than silently failing

---

## Stack

| Layer | Technology |
|---|---|
| Framework | [Expo](https://expo.dev) (SDK 54) / React Native |
| Camera | `expo-camera` |
| Vision AI | Google Gemini (`gemini-2.5-flash-lite`) |
| Text-to-speech | `expo-speech` (native iOS voice) |
| Language | TypeScript |

---

## Getting started

### Prerequisites

- Node 18+
- [Expo Go](https://expo.dev/go) installed on your iPhone
- A [Gemini API key](https://aistudio.google.com/app/apikey) (free tier works)

### 1. Clone and install

```bash
git clone https://github.com/your-username/vision-assist-mobile.git
cd vision-assist-mobile
npm install
```

### 2. Add your API key

Create `visionapikey.env` in the project root:

```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.5-flash-lite
```

### 3. Start the dev server

```bash
npm run dev
```

### 4. Open on your phone

Scan the QR code that appears in the terminal with the **Expo Go** app on your iPhone. Your phone and Mac must be on the same Wi-Fi network.

---

## Project structure

```
src/
  components/
    AccessibleConfirmButton.tsx   # double-tap confirm button
    LoadingOverlay.tsx            # scanning indicator
  config/
    env.ts                        # reads API keys from environment
  hooks/
    useDoublePressConfirm.ts      # double-press interaction logic
  services/
    mockSceneDescription.ts       # Gemini API integration
  tools/
    textToSpeech.ts               # serial TTS queue using expo-speech
scripts/
  sync-vision-env.mjs             # syncs visionapikey.env → .env.local
App.tsx                           # root component
visionapikey.env                  # your keys (git-ignored)
```

---

## How it works

1. The user taps **Scan Surroundings**
2. `expo-camera` captures a JPEG frame as base64
3. The base64 image is sent to the Gemini API with a prompt requesting a 4–6 sentence scene description
4. The response is displayed on screen and spoken aloud via `expo-speech`
5. Phrases are queued and spoken one at a time — a new scan cancels any in-progress speech

---

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start Metro bundler (use with Expo Go) |
| `npm run ios` | Build and run on iOS simulator |
| `npm run android` | Build and run on Android emulator |
| `npm run typecheck` | TypeScript type check |

---

## Privacy

- Camera frames are sent to the Google Gemini API over HTTPS and are not stored by this app
- No analytics or tracking
- `visionapikey.env` is git-ignored and never committed

---

## License

MIT
