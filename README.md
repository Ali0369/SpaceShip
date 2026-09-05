# NAUTILUS-X

**An interactive developer vessel — a hybrid 3D/2D portfolio experience.**

NAUTILUS-X is a single-file, dependency-light portfolio site built around one idea: the visitor isn't scrolling a webpage, they're sitting in the cockpit of a spacecraft. A first-person 3D cockpit provides the atmosphere and navigation; clean 2D interfaces — emerging directly from the physical consoles — carry the actual content (projects, skills, experience). A separate flyable Star Map lets visitors physically pilot a ship to each section instead of just clicking a nav bar.

No build step, no bundler, no node_modules. It's one `.html` file that runs anywhere.

---

## Contents

- [Quick start](#quick-start)
- [What's inside](#whats-inside)
- [Configuring your content](#configuring-your-content)
- [Adding project cover images](#adding-project-cover-images)
- [Setting up the contact form](#setting-up-the-contact-form)
- [The Star Map (flyable section)](#the-star-map-flyable-section)
- [Controls reference](#controls-reference)
- [Tech stack & external assets](#tech-stack--external-assets)
- [Browser support & performance](#browser-support--performance)
- [Known limitations](#known-limitations)
- [Customization tips](#customization-tips)
- [License & credits](#license--credits)

---

## Quick start

1. Download `nautilus-x.html`.
2. Open it directly in any modern desktop browser (Chrome, Edge, Firefox, Safari). Double-clicking the file works — no local server required.
3. To publish it, upload the single file to any static host (GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3, etc.) — there's nothing to build.

That's the whole deployment story. Everything — HTML, CSS, and JavaScript — lives in one file for portability.

---

## What's inside

| Layer | What it does |
|---|---|
| **3D cockpit** | First-person pilot's seat, a unified dashboard with three physical consoles (Identity, Mission Control, Neural Core) and an embedded command terminal, enclosure walls, an overhead canopy frame, and a rotating signature "core" object. |
| **2D interfaces** | Premium, readable panels for About, Projects (with full case studies), Skills, Education, Experience, Experiments, and Contact — all triggered by the consoles, never replacing the 3D world. |
| **Star Map** | A separate flyable third-person spaceship mode. Fly to one of six glowing objective markers and either press **E** or fly straight into it to open that section automatically. Includes asteroid obstacles with real collision "blasts," an engine trail, a minimap/radar, and live velocity/hull HUD gauges. |
| **Command terminal** | A real, working text terminal (`about`, `projects`, `skills`, `education`, `experiments`, `journey`, `mission`, `contact`, `resume`, `starmap`, `dock`, `clear`, `help`). |
| **Scroll-to-reveal intro** | Boots with a cinematic system-check sequence, then a full black screen with just your name/role. Scrolling down dollies the camera forward into the cockpit. |
| **Contact form** | A real form that emails you directly (see [setup](#setting-up-the-contact-form)) — no backend required. |
| **Mobile fallback** | 3D is disabled on small screens in favor of a clean, stacked card-based version of the same content — nothing is locked behind the 3D experience. |

---

## Configuring your content

Nearly everything you need to personalize lives in one place: the `CONFIG` object near the top of the `<script>` tag.

```js
const CONFIG = {
  name: "Muhammad Ali",
  resumeUrl: "",           // add a link to enable "OPEN MANIFEST" in the Identity panel
  contact: {
    email: "hello@example.com",
    github: "https://github.com/",
    linkedin: "https://linkedin.com/in/"
  },
  projects: [ /* … */ ],
  nodes: { /* … */ },       // Neural Core skill categories
  journey: [ /* … */ ],     // Flight Log / work experience
  education: [ /* … */ ],   // Academy panel
  focus: [ /* … */ ]        // Current Mission focus tags
};
```

**The `projects`, `journey`, and `education` arrays currently contain sample/dummy content**, clearly labeled as such (e.g. *"Sample: replace via CONFIG"*). They exist so the site demos fully populated out of the box. Replace them with your real work whenever you're ready — nothing else in the code needs to change; every panel, the Star Map objectives, and the Mission Control radar all read from this one object.

### Project entry shape

```js
{
  id: "01",
  name: "Project Name",
  category: "AI / ML",        // shown as a tag; also colors the case-study visuals
  year: "2025",
  role: "Your Role",
  tech: ["Python", "React"],
  description: "One or two sentence summary.",
  challenge: "What problem you were solving.",
  approach: "How you approached it.",
  build: "What you actually built.",
  result: "A real, verifiable outcome — never invent metrics here.",
  live: "",                   // optional live URL
  repo: "",                   // optional repository URL
  image: ""                   // cover image URL — see below
}
```

---

## Adding project cover images

Each project's case-study panel has a cover box sized to a 16∶7 ratio. To add an image, just fill in the `image` field on that project:

```js
image: "https://your-cdn.com/project-cover.jpg"
```

The `<img>` tag is already wired up with `object-fit: cover`, so **any image you drop in will fill the box completely and stay cropped-to-fit** — you don't need to pre-crop or resize it to an exact ratio. If a URL is missing or fails to load, it gracefully falls back to a plain placeholder mark instead of showing a broken-image icon.

---

## Setting up the contact form

The Contact panel includes a real, working form (Name / Email / Message) that emails you directly, powered by **[FormSubmit.co](https://formsubmit.co)** — chosen specifically because it requires **no signup, no API key, and no dashboard**, unlike Formspree.

**Setup:**
1. Put your real email address in `CONFIG.contact.email`.
2. That's it — the form already posts to `https://formsubmit.co/ajax/${CONFIG.contact.email}`.

**One-time activation:** the *first* message anyone submits triggers an automated confirmation email from FormSubmit to that address. Click **Confirm** in that email once, and every message after that is delivered silently and for free, forever. Until it's confirmed, submissions will appear to succeed on the front end but won't be delivered yet — so send yourself a test message first and confirm it before sharing the site.

*Prefer Formspree instead?* It offers a submission dashboard and spam filtering in exchange for requiring a free account. Swap the `fetch()` URL in the contact form handler to your Formspree endpoint (`https://formspree.io/f/{your_form_id}`) if you'd rather use that.

---

## The Star Map (flyable section)

Accessible via the **"ENTER STAR MAP"** button on the hero screen, the **STAR MAP** nav button, or the terminal commands `starmap` / `fly` / `launch`.

Six glowing markers orbit a sun, each mapped to a real section:

| Codename | Section |
|---|---|
| GENESIS | About |
| SYNTHEX | Skills |
| ACADEMY | Education |
| EXPEDITION | Experience |
| CODEX | Projects |
| NOVARA | Contact |

Fly close enough to a marker and it opens automatically; from further away, press **E** to scan it manually. An asteroid field provides obstacles — colliding with one triggers a real particle explosion, a hull-integrity hit, and a brief camera shake, then the rock respawns elsewhere after a few seconds. There's no fail state; hull regenerates slowly on its own. A minimap in the bottom-right shows your heading relative to the sun and every marker. Use **`dock`** in the terminal or the **"RETURN TO COCKPIT"** button to exit back to the console-based hub.

---

## Controls reference

| Input | Action |
|---|---|
| **Drag** (mouse/trackpad) | Rotate / look |
| **↑ / ↓** | Pitch up / down |
| **A/D** or **← / →** | Yaw left / right |
| **Scroll** | Zoom camera distance |
| **W / S** | Thrust forward / backward |
| **Shift** | Boost |
| **E** | Scan the nearest marker in range |
| **Tab** | Toggle chase-cam / nose-cam |
| **R** | Reset ship to spawn |

---

## Tech stack & external assets

- **Three.js r0.128** (via jsDelivr CDN) for all 3D rendering, plus the classic (non-module) `GLTFLoader` for real `.glb` model loading.
- **Vanilla JS, HTML, CSS** — no framework, no build tooling.
- **Google Fonts:** Space Grotesk (display), Inter (body), JetBrains Mono (technical/UI text).
- **Real GLB models used**, each with a procedural fallback in case a network blocks external assets:
  - **Player ship** — *"Spaceship"* by Liz Reddington, **CC BY 3.0**, via [poly.pizza](https://poly.pizza). Attribution is shown on-screen in the Star Map HUD — keep it if you redistribute this project, per the license.
  - **Drifting relic** — `DamagedHelmet.glb`, an official [Khronos Group glTF-Sample-Assets](https://github.com/KhronosGroup/glTF-Sample-Assets) public sample model.
- **[FormSubmit.co](https://formsubmit.co)** for the contact form backend.

---

## Browser support & performance

- Built and tested against evergreen desktop browsers (Chrome, Edge, Firefox, Safari).
- `prefers-reduced-motion` is respected — ambient camera drift, starfield rotation, and long transition animations are cut down automatically.
- On viewports under `720px`, the entire 3D engine is skipped in favor of the mobile card-based layout — there's no heavy WebGL cost on phones.
- Geometry counts, particle counts, and asteroid counts are deliberately modest so the scene stays smooth on mid-range laptops without a dedicated GPU.

---

## Known limitations

- The Star Map's flight controls (drag-to-look, WASD) are desktop-only; touch devices get the simplified mobile layout instead.
- The GLB assets are fetched from third-party CDNs (jsDelivr, poly.pizza) at runtime — if a visitor's network blocks those domains, the site automatically falls back to procedurally-built ship/relic geometry with no visible error, but the "real" models won't load until the network allows it.
- The resume button ("OPEN MANIFEST") does nothing until you set `CONFIG.resumeUrl` — this is intentional; no placeholder file is fabricated.

---

## Customization tips

- **Colors:** all of the site's palette lives in CSS custom properties at the top of the `<style>` block (`--void`, `--cyan`, `--green`, etc.) — change them once, and every glow sprite, console, and accent updates.
- **Consoles:** the cockpit's three console units are all built from one reusable `buildConsoleUnit(width, height, color, intensity)` function, so tweaking one screen's proportions is a one-line change.
- **Objectives / Star Map layout:** the `OBJECTIVES` array (codename, section, orbit angle, distance) fully drives both the 3D marker placement and the on-screen checklist — reorder or reposition sections by editing that array alone.

---

## License & credits

This project's own code has no license restrictions imposed by its structure — treat it as yours to use and modify freely for your personal portfolio.

The bundled third-party assets keep their original licenses:
- *"Spaceship"* by Liz Reddington — **CC BY 3.0** (attribution required; already included in the UI).
- `DamagedHelmet.glb` — Khronos Group glTF-Sample-Assets (public sample/demo asset).
- Google Fonts (Space Grotesk, Inter, JetBrains Mono) — **SIL Open Font License**.

---

*Built as NAUTILUS-X — the spaceship is the website.*
