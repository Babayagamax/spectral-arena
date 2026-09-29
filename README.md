# ⚔️ Spectral Arena — Echoes of the Voidbound Warden

A dark-fantasy **wave-survival arena game** in a single HTML file. You are a
silver-and-gold knight; the Voidbound Warden's echoes descend every 5th wave.
No engine, no dependencies — pure HTML, CSS and JavaScript.

![arena](assets/icon.png)

## Play

- **In the browser (local):** open `www/index.html` — that's the whole game.
- **Online (GitHub Pages):** after enabling Pages (below), play at
  `https://babayagamax.github.io/spectral-arena/`

| Action | Desktop | Phone |
|---|---|---|
| Move | WASD / Arrows | left-side joystick |
| Aim & fire | Mouse + hold click | auto-aim |
| Dash | Space | DASH button |
| Pause / mute | P / M | — |

After every wave, pick 1 of 3 random **boons** (multishot, crits, piercing
bolts, lifesteal…) — every run plays differently. High score is saved
locally. Survive the boss that arrives every 5th wave.

## Repository structure

```
spectral-arena/
├── www/                      ← the game (self-contained PWA)
│   ├── index.html            ← entire game, one file
│   ├── manifest.webmanifest  ← installable-app metadata
│   ├── sw.js                 ← offline caching service worker
│   └── icons/                ← app icons
├── character-sheet/
│   └── voidbound-warden.html ← the boss's visual design & lore sheet
├── assets/                   ← icon sources for @capacitor/assets
├── capacitor.config.json     ← Android app shell configuration
├── package.json
├── docs/
│   └── playstore-guide.md    ← step-by-step Google Play publishing guide
└── .github/workflows/
    ├── pages.yml             ← auto-deploys the game to GitHub Pages
    └── build-android.yml     ← builds the Play Store app in the cloud
```

## Publish on Google Play

The game ships as a ready-to-wrap **Capacitor** project. Full walkthrough in
[`docs/playstore-guide.md`](docs/playstore-guide.md) — the short version:

```bash
npm install
npx cap add android
npx capacitor-assets generate --android
cd android && ./gradlew bundleRelease   # → app-release.aab for the Play Store
```

### 📱 No computer? Build in the cloud (works entirely from a tablet)

GitHub can build the Play Store app for you — **no Android Studio needed**:

1. **Add the signing secrets** (one time). Repo → *Settings* → *Secrets and
   variables* → *Actions* → *New repository secret*. Create these four, pasting
   the values from the `signing-details.txt` / `keystore-base64.txt` files you
   were given:
   | Secret name | Value |
   |---|---|
   | `KEYSTORE_BASE64` | entire contents of `keystore-base64.txt` |
   | `KSTORE_PWD` | the password in `signing-details.txt` |
   | `KEY_ALIAS` | `spectralarena` |
   | `KEY_PWD` | the password in `signing-details.txt` (same as `KSTORE_PWD`) |

2. **Run the build.** Repo → *Actions* tab → **Build Android app (Play Store)** →
   *Run workflow*. ~10 minutes later the finished `app-release.aab` appears
   under **Releases** — download it straight from your tablet.

3. **Upload to Google.** [play.google.com/console](https://play.google.com/console)
   in your tablet browser → create app → *Release → Production* → upload the
   `.aab`. The privacy policy URL is:
   `https://babayagamax.github.io/spectral-arena/privacy.html`
   (live once GitHub Pages is enabled — see above).

## Design docs

The boss is designed first, the arena built around it:
see [`character-sheet/voidbound-warden.html`](character-sheet/voidbound-warden.html)
— palette, silhouette, attack lore and the original art prompt.

---

*Made with vanilla JS · no frameworks were harmed.*
