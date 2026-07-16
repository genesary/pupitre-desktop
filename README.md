# Pupitre Desktop

Desktop companion app for [Pupitre](https://github.com/genesary/pupitre), built with **Tauri v2**.

Some labs require verifying work done locally on the student's machine (e.g. `podman pull`, `podman run`). For these, the platform runs as a desktop app that wraps the Pupitre web frontend and exposes local check commands to it.

```
.
├── src/              # Minimal HTML shell (loads localhost:3000)
└── src-tauri/         # Rust backend
    └── src/lib.rs     # Local check commands (podman images, podman events)
```

When running inside Pupitre, the "Vérifier mon travail" button calls a local Rust command instead of the remote checker-service:

```
Student clicks "Vérifier" (inside Pupitre)
  → frontend detects window.__TAURI_INTERNALS__
  → invoke("local_check", { checkType, params })
  → Rust runs podman commands locally
  → {allow, violations} returned to frontend
```

Lab modules with `checkProvider: local` in the Pupitre CRD use this flow. Labs with `checkProvider: gitlab` (or no provider) use the remote checker-service instead — see the main [pupitre](https://github.com/genesary/pupitre) repository.

## Build

```bash
# macOS (ARM64)
cd src-tauri && cargo build --release

# Linux x86_64 (via Silverblue VM or toolbox)
toolbox run bash -c 'source ~/.cargo/env && cd ~/pupitre-desktop/src-tauri && cargo build --release'
```

## Dev

```bash
npm install
npm run dev
```

Requires the Pupitre frontend running on `http://localhost:3000` (see `src-tauri/tauri.conf.json`).
