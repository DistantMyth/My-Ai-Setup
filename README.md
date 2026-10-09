# My AI Coding Setup

A version-controlled record of the AI coding tools installed on my PC: what exists, how it is configured, what has actually been verified, and what to improve next.

**Primary harness:** OMP (Oh My Pi). This repository documents the setup; it does not install tools or contain live credentials/configuration backups.

## Start here

- [Current setup and audit findings](docs/setup-inventory.md) — harnesses, plugins, skill sources, integrations, safety posture, recommendations, and refresh procedure.
- [Complete inventory snapshot](inventory/2026-10-09.json) — installed OMP package assets, shared skill directory names, lock-backed sources, overlap, and secondary-harness metadata.
- [Cool workflows playbook](docs/cool-workflows.md) — step-by-step creative engineering workflows and prompt templates.

The baseline audit is dated **2026-10-09**. Future changes should update the human-readable record and add a new dated snapshot rather than overwrite the baseline.

## Cool Workflows

Practical, repeatable AI workflows for engineering and creative web builds:

### 1. Scroll-Linked Canvas Animation (Apple-Style Product Scrub)

Interactive product scrubbers (spins, assembly, teardowns controlled by scroll) are built as sequential image stills rendered to an HTML5 `<canvas>` that scrubs based on container scroll progress.

- **Pipeline:**
  1. **Start & End Frames:** Generate matching front-facing and 180° rotated rear product frames in [Google Flow](https://labs.google/fx/tools/flow) with identical lighting, scale, and background.
  2. **Frames to Video:** Interpolate frames into a continuous rotation video with a locked camera, constant linear speed, and no easing.
  3. **Extract Stills:** Convert the MP4 into a 30fps image sequence ZIP using [Ezgif Video to JPG](https://ezgif.com/video-to-jpg) (~150 frames for a 5s clip).
  4. **Canvas Implementation:** Unzip into your project and prompt an AI coding tool (OMP, Claude, Cursor) to generate a vanilla JS canvas with progressive loading, DPR scaling, requestAnimationFrame loop, and reduced-motion support.
- **Detailed Playbook & Prompts:** See [docs/cool-workflows.md](docs/cool-workflows.md) for full, copyable prompts and technical caveats.

## Cool Websites

Curated tools, prompt libraries, and references for AI workflows:

- **[Pathwise Academy Prompts](https://pathwiseacademy.co/prompts)** — Curated high-leverage prompts, interactive build patterns, and frontend AI workflows.
- **[Google Flow](https://labs.google/fx/tools/flow)** — Generative media tool for consistent start/end frame creation and motion transitions ([docs](https://support.google.com/flow/answer/16352836)).
- **[Ezgif Video to JPG](https://ezgif.com/video-to-jpg)** — Fast browser-based video-to-frames extractor with batch ZIP export.
- **[Instagram Reference Demo](https://www.instagram.com/reel/DdyqkyqvuUM/)** — Visual example of scroll-linked product scrub animations.

## Evidence matters

- **Installed:** a package, file, or installation record exists.
- **Configured/enabled:** a configuration or manifest declares it; this does not prove successful execution.
- **Runtime-verified:** a command or this OMP session exercised the stated capability.
- **Unverified:** authentication, connectivity, loading, or execution was not exercised.

Skill counts are filesystem/package counts, not a count of simultaneously loaded or working capabilities. A skill is guidance; it does not install the libraries, services, MCP servers, or credentials it describes.

## Repository boundaries

Keep sanitized metadata, decisions, and verification results here. Never commit tokens, auth files, raw harness configuration, private project paths, chat/session history, memory contents, logs, or runtime databases. `.gitignore` guards common local artifacts; it is not a secret scanner.

No harness settings or installed packages were changed during the baseline audit.
