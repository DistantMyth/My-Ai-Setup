# My AI Coding Setup

A version-controlled record of the AI coding tools installed on my PC: what exists, how it is configured, what has actually been verified, and what to improve next.

**Primary harness:** OMP (Oh My Pi). This repository documents the setup; it does not install tools or contain live credentials/configuration backups.

## Start here

- [Current setup and audit findings](docs/setup-inventory.md) — harnesses, plugins, skill sources, integrations, safety posture, recommendations, and refresh procedure.
- [Complete inventory snapshot](inventory/2026-10-09.json) — installed OMP package assets, shared skill directory names, lock-backed sources, overlap, and secondary-harness metadata.

The baseline audit is dated **2026-10-09**. Future changes should update the human-readable record and add a new dated snapshot rather than overwrite the baseline.

## Evidence matters

- **Installed:** a package, file, or installation record exists.
- **Configured/enabled:** a configuration or manifest declares it; this does not prove successful execution.
- **Runtime-verified:** a command or this OMP session exercised the stated capability.
- **Unverified:** authentication, connectivity, loading, or execution was not exercised.

Skill counts are filesystem/package counts, not a count of simultaneously loaded or working capabilities. A skill is guidance; it does not install the libraries, services, MCP servers, or credentials it describes.

## Repository boundaries

Keep sanitized metadata, decisions, and verification results here. Never commit tokens, auth files, raw harness configuration, private project paths, chat/session history, memory contents, logs, or runtime databases. `.gitignore` guards common local artifacts; it is not a secret scanner.

No harness settings or installed packages were changed during the baseline audit.
