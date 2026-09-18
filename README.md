# CC PKG

CC PKG is a lightweight package manager for CC:Tweaked.

## Goals

- Simple package installation from HTTP repositories
- Version-aware updates
- Safe package removal
- Dependency support
- Local package metadata
- Static hosting on GitHub Pages or any HTTP server

## Client

The initial client is `pkg`.

```text
pkg help
pkg repo list
pkg search <query>
pkg info <package>
pkg install <package>
pkg update
pkg upgrade
pkg remove <package>
pkg list
```

See `docs/package-format.md`, `docs/repository.md`, and `docs/architecture.md`.

## Status

This repository contains the initial CC PKG architecture. The format is intentionally small and HTTP-friendly so it can run on standard CC:Tweaked computers without external Lua libraries.
