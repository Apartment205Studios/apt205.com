# Apartment 205 Studios: apt205.com holding page

## Folder structure

```
apt205/
├── index.html
├── favicon.ico              # tab icon (16/32/48/256), served at /favicon.ico
└── assets/
    ├── logo.png             # full-color logo, "studios" recolored white for black bg
    ├── favicon.svg          # vector tab icon, crisp on any screen
    ├── favicon.png          # three brand dots, 512x512, transparent
    └── apple-touch-icon.png # same mark on black, 180x180 (iPhone home screen)
```

All assets are included and ready. The logo is your official full-color file with only the "studios" lettering switched from black to white, trimmed to its edges. Nothing else was changed.

For extra retina crispness later, export the logo from Illustrator at about 1600px wide (or as SVG) and replace `assets/logo.png`.

## Deploy to Vercel

**Option A: drag and drop (fastest)**
1. Go to vercel.com/new and choose to deploy without a Git repo (or use the Vercel dashboard upload).
2. Drag in the `apt205` folder.
3. Framework preset: **Other**. No build command, no output directory.
4. Deploy.

**Option B: CLI**
```
npm i -g vercel
cd apt205
vercel        # preview deploy
vercel --prod # production
```

**Option C: GitHub**
Push the folder to a repo, import it at vercel.com/new, preset **Other**, deploy. Every push redeploys.

**Connect apt205.com**
1. Project → Settings → Domains → add `apt205.com` and `www.apt205.com`.
2. At your registrar, set the records Vercel shows (typically an `A` record for the root to `76.76.21.21` and a `CNAME` for `www` to `cname.vercel-dns.com`).
3. Set `apt205.com` as primary so `www` redirects to it. SSL is automatic.

## Tuning the animation

Everything lives in the `CONFIG` object at the top of the `<script>` in `index.html`.

| Setting | Default | What it does |
|---|---|---|
| `gridSpacing` | 30 | Pixels between dots on desktop/tablet. Lower = denser sky. |
| `gridSpacingMobile` | 24 | Same, for screens under 640px wide. |
| `maxDots` | 5000 | Performance cap. Spacing widens automatically on huge screens. |
| `minRadius` | 0.9 | Resting dot radius (the small, understated state). |
| `maxRadius` | 2.9 | Peak radius of each dot's breathing cycle. |
| `radiusVariation` | 0.25 | Per-dot size randomness. 0 = all identical. |
| `dotOpacity` | 0.5 | Opacity of gray/off-white dots. |
| `colorOpacity` | 1.0 | Opacity of red/blue/yellow dots. |
| `coloredDotRatio` | 0.27 | Share of dots in brand colors. Keep 0.25 to 0.30. |
| `colorRadiusBoost` | 1.12 | Colored dots are drawn this much larger than neutral ones. |
| `pulseDurationMin` / `Max` | 5 / 9 | Seconds per breathing cycle; each dot picks a value in this range. |
| `pulsePhaseVariation` | 1 | 0 = every dot breathes in sync, 1 = fully staggered. |
| `driftDistanceMin` / `Max` | 4 / 8 | Pixels each dot wobbles around its grid point. |
| `driftSpeed` | 0.15 | Wobble cycles per second. Lower = calmer. |
| `driftSpeedVariation` | 0.55 | Per-dot speed randomness so the grid never sways together. |
| `mouseRadius` | 170 | Pixel reach of the cursor spotlight. |
| `mouseMaxRadius` | 4.2 | Dot radius right under the cursor. |
| `mouseStrength` | 1 | 0 turns the spotlight off, 1 is full. |
| `mouseSmoothing` | 6 | Response speed. Higher = snappier, lower = dreamier trail. |
| `logoOvalWidth` / `Height` | 1.18 / 1.5 | Size of the invisible oval around the logo, as multiples of the logo's width and height. |
| `logoOvalInner` | 0.55 | Inside this fraction of the oval, dots are fully gone. Raise it for a tighter clearing. |
| `logoOvalOuter` | 1.1 | At this fraction, dots are back to full size and opacity. Raise it for a softer, longer fade. |
| `socialClearance` | 14 | Empty sky (px) kept around the Instagram link. |
| `clearanceFeather` | 18 | Px over which dots fade back in around the Instagram link and tagline. |
| `taglineFadeMs` | 500 | Tagline fade in/out duration in ms. The dots part on the same timing. |
| `taglineClearance` | 16 | Sky (px) cleared around the tagline when it appears. |
| `taglinePushDistance` | 16 | How far (px) nearby dots slide away as the tagline appears. |
| `taglinePushReach` | 60 | How far beyond the tagline (px) dots still get nudged. |
| `cursorOffsetX` / `Y` | -3 / -19 | Nudges the 🔑 so its tip lands on the pointer. Emoji art differs by OS; adjust if the tip feels off. |

Quick recipes:
- **Calmer:** `driftSpeed: 0.09`, `driftDistanceMax: 6`, `pulseDurationMin: 7`, `pulseDurationMax: 11`
- **Tighter around the logo:** `logoOvalInner: 0.65`
- **Less color:** `coloredDotRatio: 0.2`
- **More visible stars:** `dotOpacity: 0.65`, `maxRadius: 2.8`
- **Bigger spotlight:** `mouseRadius: 240`, `mouseMaxRadius: 5`

## Behavior notes
- Desktop: hover the logo to reveal the tagline; the hover zone includes the tagline so it never flickers.
- Mobile: tap the logo to toggle, tap anywhere else to close. No custom cursor, no spotlight; the stars still breathe.
- Keyboard: Tab to the logo, Enter/Space toggles, Escape closes.
- Reduced motion: static grid, normal system cursor.
- Animation pauses when the tab is hidden.
