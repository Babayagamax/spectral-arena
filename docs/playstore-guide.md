# Spectral Arena — Play Store Build Kit

This package contains everything needed to turn the browser game into an Android
app you can publish on Google Play. The game itself is a single file
(`www/index.html`) — all the other files are the "wrapper" that makes Android
and app stores happy.

**What you need before starting:**
- A Windows / Mac / Linux computer
- About 1 hour of setup time (mostly downloads)
- A one-time **$25 Google Play developer fee** (paid when you create the account)

---

## What's in this folder

```
spectral-arena/
├── www/                    ← the game (works in any browser as-is)
│   ├── index.html          ← the complete game, one file
│   ├── manifest.webmanifest
│   ├── sw.js               ← offline caching (PWA support)
│   └── icons/              ← app icons (192, 512, 1024 px)
├── assets/                 ← icon sources for @capacitor/assets
├── capacitor.config.json   ← Android app settings (name, id, colors)
└── package.json            ← project dependencies
```

The game app id is `com.voidbound.spectralarena` and the app name is
"Spectral Arena". You can change both — see the FAQ at the bottom.

---

## Step 1 — Install the tools (one time)

1. Install **Node.js** (LTS version) from https://nodejs.org
2. Install **Android Studio** from https://developer.android.com/studio
   - During first launch it will download the Android SDK automatically.
   - In Android Studio → Settings → SDK Manager, make sure an SDK Platform
     (e.g. Android 14 / API 34) and "Android SDK Build-Tools" are installed.
3. Install **Java JDK 17** if Android Studio doesn't bundle it for you
   (recent versions do). `java -version` should print 17.x.

## Step 2 — Set up this project

Open a terminal in this folder, then:

```bash
npm install
npx cap add android
```

`npx cap add android` generates the `android/` folder — a full Android Studio
project pre-configured to run the game in a native app shell.

## Step 3 — Test it on your phone

- **Easiest:** connect your Android phone with USB (enable Developer Options →
  USB debugging), then run the app from Android Studio (green ▶ button).
- Or use Android Studio's built-in emulator.

Check that the joystick, dash button, and fullscreen feel right. The game
already has touch controls built in.

## Step 4 — Generate the final icons

```bash
npm install -D @capacitor/assets
npx capacitor-assets generate --android
```

This builds all the mipmap/splash icons from `assets/icon.png` into the
`android/` project.

## Step 5 — Create a signing key (REQUIRED for publishing)

Every Play Store app must be signed. Create one keystore and keep it safe —
if you lose it, you can never update your app:

```bash
keytool -genkey -v -keystore spectral-arena.keystore -alias spectralarena \
  -keyalg RSA -keysize 2048 -validity 10000
```

(keytool comes with the JDK. Save the .keystore file and password somewhere
permanent like a cloud backup.)

## Step 6 — Build the release app (AAB)

Google Play requires the .aab format:

```bash
cd android
./gradlew bundleRelease
```

The file appears at `android/app/build/outputs/bundle/release/app-release.aab`.

> If the build asks for signing details, open
> `android/app/build.gradle` and add:
>
> ```gradle
> android {
>   signingConfigs {
>     release {
>       storeFile file("../../spectral-arena.keystore")
>       storePassword "YOUR_PASSWORD"
>       keyAlias "spectralarena"
>       keyPassword "YOUR_PASSWORD"
>     }
>   }
>   buildTypes { release { signingConfig signingConfigs.release } }
> }
> ```

## Step 7 — Publish to Google Play

1. Go to https://play.google.com/console and create a developer account ($25 one-time).
2. Click **Create app** → fill in name ("Spectral Arena"), language, and
   "Game" / "Free" (or paid).
3. In **Release → Production**, upload your `app-release.aab`.
4. Complete the required items in the left sidebar:
   - App content questionnaire (ads: none, etc.)
   - **Privacy policy URL** — you need a simple web page saying the game
     collects no data. Host one free on GitHub Pages / Netlify / Google Sites.
   - Store listing: description, screenshots (phone screenshots of the game —
     you can take these from your phone while testing), icon, feature graphic.
5. Submit for review. First review typically takes a few days to a week.

That's it — once approved, your game is live on the Play Store, worldwide.

---

## FAQ

**Change the app name / id?**
Edit `capacitor.config.json` (`appName`, `appId`) BEFORE running
`npx cap add android`. The appId can never be changed after publishing, so
choose something unique, e.g. `com.yourname.spectralarena`.

**Changed the game (www/index.html)?**
Run `npx cap sync android` and rebuild.

**No Android Studio / don't want to pay $25 yet?**
The `www/` folder is a complete **PWA**. Upload it free to Netlify, GitHub
Pages, or itch.io and players can install it from the browser ("Add to Home
screen") with zero fees and zero review.

**Sound issues on Android?**
Sound starts only after the first screen tap (browser policy). The game
already starts audio on first interaction, so players just tap ENTER THE ARENA.

**Want ads / analytics later?**
Add Google AdMob via the Capacitor community plugin — but then your privacy
policy and store questionnaire must reflect it.

---

*Game: Spectral Arena — Echoes of the Voidbound Warden · v1.0*
