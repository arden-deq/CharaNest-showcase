# CharaNest

> An offline-first desktop AI companion that lives—and remembers—on your PC

[한국어](README.md) · [GitHub Profile](https://github.com/jungducknam)

![Status](https://img.shields.io/badge/status-private_development-6f5bd3)
![Platform](https://img.shields.io/badge/platform-desktop-20242c)
![Approach](https://img.shields.io/badge/AI-local_first-38b2ac)
![Source](https://img.shields.io/badge/source-private-59636e)

![CharaNest main overlay](docs/screenshots/main-overlay.jpg)

CharaNest is a desktop AI companion prototype built around a pixel character, local inference, and user-controlled memory. It combines a transparent Electron overlay with a local LLM runtime to create a personal companion experience without permanent dependence on a hosted AI API.

This public repository is a portfolio and technical showcase. Product source code, model files, training data, and release assets are maintained in private repositories.

## At a glance

| Item | Details |
|---|---|
| Product | Transparent-overlay desktop AI companion |
| Core value | Local inference, user-controlled memory, character-first interaction |
| Scope | Product planning, system architecture, Electron client, local LLM integration, evaluation and release automation |
| Platforms | Windows and macOS development/validation |
| Status | Private development showcase — no public download |

## The problem

Cloud AI assistants are convenient, but personal conversations may leave the device while availability and cost depend on a network service. CharaNest started with three questions:

- Can personal conversations and memories remain on the user's own PC whenever possible?
- Can an assistant feel present on the desktop instead of living behind another chat tab?
- Can a desktop AI system with many failure points—windows, storage, model runtime, and packaging—behave predictably?

## Highlights

- **Character-first overlay** — transparent, frameless, always-on-top window; avatar drag and zoom; speech bubbles; radial menu
- **Local AI runtime** — manages a Gemma-family local model through a `llama.cpp` `llama-server` sidecar
- **User-controlled memory** — local SQLite storage with view, per-item deletion, full deletion, and export controls
- **Context-aware conversation** — recent turns, active topics, session summaries, and local date/time grounding
- **Explicit training-data flow** — only user-approved answer corrections can be exported for SFT/DPO experiments
- **Companion loop** — tasks, focus timer, and accumulated activity tied to the character experience
- **Localized UI** — Korean, English, and Japanese interface strings

## Architecture

![CharaNest system architecture](docs/architecture.svg)

The renderer runs without Node.js privileges. A sandboxed preload bridge exposes a narrow, validated IPC surface to the Electron main process. Main owns local storage, window behavior, conversation orchestration, and the LLM sidecar lifecycle.

### Key design decisions

1. **Offline-first product path**
   Normal product execution does not start an external search backend. Fresh-information experiments remain isolated from the default product path.

2. **App-owned LLM sidecar**
   The runtime uses a random high loopback port and a per-process API key. Startup failures remain visible instead of silently switching models, and child processes are cleaned up when the app exits.

3. **Explicit Electron trust boundary**
   `contextIsolation`, renderer sandboxing, a limited IPC surface, and payload validation separate UI code from OS-level capabilities.

4. **User-owned memory**
   Memory lives in a local database and remains visible, deletable, and exportable from the UI. Consent and observability are treated as ongoing release gates.

## Engineering case studies

### Operating a local model, not merely calling one

A desktop app must handle model files, native modules, CPU/GPU differences, sidecar ports, and abnormal exits. Runtime profiles, readiness probes, process ownership, force-stop paths, and diagnostic snapshots make failures traceable.

### Isolating cross-OS response differences

A repeatable parity workflow locks code revision, model hash, runtime build, sampler settings, user data, and feature flags across Windows and macOS. This separates model variation from product policy, streaming behavior, and evaluation-fixture issues.

### Separating feature tests from release readiness

One Windows baseline passed 332 automated tests and 28 Electron E2E scenarios, yet the release stayed blocked because separate product-quality gates did not pass. Safety, final-UI convergence, and transparent failure behavior take precedence over the raw test count.

### Improving conversations without losing data control

Only answers that a user explicitly edits and approves enter the export path. Real-time product data and offline training/evaluation workflows remain separate by design.

## Technology

| Area | Technology |
|---|---|
| Desktop | Electron, electron-vite, electron-builder |
| Frontend | React, TypeScript, Vite |
| Local storage | SQLite, better-sqlite3 |
| Local AI | Gemma-family GGUF, llama.cpp / llama-server |
| 2D / 3D | Sprite atlas, experimental Three.js path |
| Training & evaluation | Python, PyTorch/Transformers, MLX, LoRA experiments, SFT/DPO export |
| Quality | Vitest, Playwright, real Electron bridge validation, failure injection |

## Screens

### Desktop overlay

![Main overlay](docs/screenshots/main-overlay.jpg)

### Character radial menu

![Radial menu](docs/screenshots/radial-menu.jpg)

### Memory controls

![Memory controls](docs/screenshots/memory-control.jpg)

### Short onboarding

![Onboarding](docs/screenshots/onboarding.jpg)

> Screens were captured from a development build. Some internal naming and UI details may change before final branding.

## Public scope

This repository contains only public-safe project documentation, an architecture diagram, and development screenshots. It does not contain application source code, commit history, model weights, adapters, datasets, credentials, infrastructure details, internal QA reports, or release assets.

The application cannot be built or run from this showcase repository.

## Project

- Created and published by **Dev Dot.**
- GitHub: [@jungducknam](https://github.com/jungducknam)
- Documentation baseline: August 2026

## Copyright

No reuse rights are granted for the documents, diagrams, or images in this repository unless separately permitted. See [LICENSE](LICENSE).
