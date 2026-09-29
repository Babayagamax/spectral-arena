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
└── .github/workflows/pages.yml ← auto-deploys the game to GitHub Pages
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

## Design docs

The boss is designed first, the arena built around it:
see [`character-sheet/voidbound-warden.html`](character-sheet/voidbound-warden.html)
— palette, silhouette, attack lore and the original art prompt.

---

*Made with vanilla JS · no frameworks were harmed.*
