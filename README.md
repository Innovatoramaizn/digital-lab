# Digital Lab (working title)

A browser-based 3D virtual laboratory for Nigerian students, from secondary school through university. It runs offline on low-end Android phones and aligns experiments to Nigerian curricula.

## Why

Many Nigerian schools and universities lack reagents, equipment, power, or lab space for practical work, yet practicals are examined (WAEC, NECO, and university lab courses). Digital Lab lets students rehearse and complete practicals on a phone or a shared computer, with no consumables and no risk.

## Who it serves

| Level | Users | Alignment | Fidelity |
|---|---|---|---|
| Secondary | SS1-SS3 students, science teachers | WAEC / NECO practical syllabi | Guided, step-by-step, forgiving |
| Tertiary | 100-300 level science and engineering students, lecturers, lab technicians | University course outlines (e.g. CHM/PHY/BIO lab courses) | Open-ended, quantitative, with error analysis and lab-report export |

The same simulation engine serves both levels. An experiment declares which levels it supports, and the level changes guidance, tolerances, and assessment, not the underlying physics or chemistry.

## MVP (two-hour build)

One experiment: **acid-base titration** (secondary level).

- Low-poly 3D bench, burette, and conical flask built from Three.js primitives
- Drip control, live burette reading, indicator color change at the endpoint
- Guided steps, a readings table, and a short quiz with a score
- Installable PWA that works offline

Out of scope for the MVP: accounts, other experiments, teacher dashboard, payments.

## Roadmap

1. **MVP:** titration, secondary level, offline PWA
2. **Experiment pack 1:** Ohm's law, simple pendulum, density, flame tests, osmosis
3. **Tertiary mode:** open-ended parameters, error analysis, CSV export, lab-report PDF
4. **Teacher and lecturer dashboard:** classes, assignments, scores
5. **Sync and accounts:** cloud progress, school licenses, payments
6. **Experiment pack 2 (tertiary):** analytical chemistry, RC circuits, microscopy, fluid mechanics
7. **LMS integration:** Moodle (LTI) for universities

## Tech stack

- **Client:** Vite, vanilla JavaScript (TypeScript later), Three.js
- **Storage:** IndexedDB (offline progress), service worker (asset caching)
- **Hosting:** Netlify, Vercel, or GitHub Pages for the static client
- **Backend (phase 5):** Postgres-backed API with auth, sync, and licensing
- **Payments (phase 5):** Paystack or Flutterwave, with mobile money / USSD where possible

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the full technical plan and [DESIGN.md](DESIGN.md) for the design system and screens.

## Project structure

```
digital-lab/
├── README.md
├── docs/
│   └── ARCHITECTURE.md
├── public/
│   ├── manifest.webmanifest
│   └── icons/
├── src/
│   ├── main.js
│   ├── engine/            # simulation core (no rendering code)
│   ├── render/            # Three.js scene, materials, camera, input
│   ├── experiments/
│   │   └── titration/
│   │       ├── manifest.json
│   │       ├── simulation.js
│   │       ├── scene.js
│   │       └── assessment.js
│   ├── ui/                # steps, readings table, quiz, level switcher
│   ├── storage/           # IndexedDB wrapper, progress store
│   └── sw.js              # service worker
├── package.json
└── vite.config.js
```

## Getting started

```bash
git clone <repo-url>
cd digital-lab
npm install
npm run dev        # local dev server
npm run build      # production build in /dist
npm run preview    # serve the production build locally
```

## Adding an experiment

1. Create `src/experiments/<name>/` with `manifest.json`, `simulation.js`, `scene.js`, and `assessment.js`.
2. In the manifest, declare title, subject, supported levels, curriculum tags, steps, and asset budget.
3. Keep simulation logic free of rendering code so it can be unit-tested and reused across levels.
4. Register the experiment in the experiment registry; the app picks it up automatically.

## Design constraints

- **Offline first:** every experiment must run with no network after the first load.
- **Light:** the first load stays under about 1 MB, and each experiment under about 500 KB.
- **Low-end devices:** target 30 fps on a 2 GB RAM Android phone; no heavy post-processing.
- **Accessible:** touch-first controls, readable text sizes, and a 2D fallback if WebGL fails.

## Business model (summary)

School and university licenses (main revenue), student exam-prep subscriptions paid by parents, sponsorship and grants for public institutions, and a freemium hook of one free experiment. Pricing is to be validated in pilots.

## Contributing

Open an issue describing the experiment or fix, branch from `main`, and submit a pull request. New experiments need a manifest, a unit-tested simulation, and a curriculum mapping.

## License

To be decided.
