# JAX Portfolio v3 — Cinematic 3D Design

**Date:** 2026-08-10
**Goal:** Personal brand / creative showcase. Replace the current `index.html` (dark Awwwards-style single page) with a full-screen, chapter-based cinematic 3D site. Same repo (`jax-portfolio`), same URL.

## Concept

One fixed WebGL canvas renders a continuous dark 3D space. Scrolling flies the camera through 7 full-viewport chapters. DOM overlays carry all text content. Nav dots (right edge) jump between chapters.

## Chapters

| # | Chapter | Content | Scene motif | Palette |
|---|---------|---------|-------------|---------|
| 1 | Intro | Loader → giant JAX wordmark, tagline "Creative Developer", scroll hint | Wordmark floating in particle space | Deep navy/black |
| 2 | Kalos | Real project — AI face analysis, live 3D scan (MediaPipe + Gemini) | Particle face/point-mesh motif | Deep teal |
| 3 | Video Studio | Real project — local prompt-to-video (LTX-2B + ComfyUI) | Floating film-frame planes | Warm ember |
| 4 | PromptDeck | Real project — private prompt-library site (Astro) | Card grid drifting in space | Violet |
| 5 | Nebula (concept) | Concept piece — generative identity system | Morphing blob / nebula | Electric blue |
| 6 | About | Short bio + skills list | Starfield pullback | Neutral dark |
| 7 | Contact | "LET'S TALK" → bcool5869@gmail.com, GitHub bcool5869-coder | Camera settles, calm scene | Accent glow |

Project chapters show: index number, title, one-line description, tag list. Real projects may link out (GitHub/live) where available; concept piece has no link.

## Architecture

- **Single file:** `index.html` at repo root. No build step.
- **Libraries (CDN, deferred):** Three.js (r160+) and GSAP 3 + ScrollTrigger.
- **Scroll model:** body height = 7 × 100vh sections. ScrollTrigger maps overall progress → camera path (position/rotation lerp between per-chapter waypoints) and per-chapter DOM overlay timelines (fade/slide).
- **Scene:** one Three.js scene containing all chapter set-pieces placed along the camera path; fog + per-chapter color grading (background/fog/light colors lerp with progress).
- **Overlays:** absolutely-positioned DOM per chapter; GSAP toggles visibility/transform by scroll progress.
- **Nav:** 7 dots, click = smooth scroll to chapter; active state tracks progress.

## Fallback (static mode)

Trigger if any of: WebGL context creation fails, Three.js/GSAP fail to load (CDN blocked), `prefers-reduced-motion: reduce`, or small-screen + low `hardwareConcurrency` heuristic.

Static mode: canvas hidden; each chapter becomes a normal-scroll 100vh section with a per-chapter CSS gradient background. All text content identical. No scroll hijacking anywhere (native scroll in both modes).

## Performance

- `renderer.setPixelRatio(min(devicePixelRatio, 1.5))`
- Low-poly geometry; particle counts reduced ~50% on mobile
- Single rAF loop; paused via `visibilitychange` when tab hidden
- No post-processing passes

## Verification

1. Serve locally, open in browser pane.
2. Console: zero errors in WebGL mode and in forced static mode.
3. Scroll through all 7 chapters at desktop (1280×800) and mobile (375×812); text readable, transitions smooth.
4. Screenshot each chapter as proof.

## Deploy

Replace `index.html`, commit to `main`, push (publishes to same URL). Push only after user confirmation.
