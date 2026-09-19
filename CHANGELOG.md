# CC PKG Changelog

## 0.8.0

- Added transactional dependency-tree installation.
- Added dependency version constraints.
- Added circular dependency and dependency conflict detection.
- Added SHA-256 hashing and optional manifest file hash verification.
- Added package lock data at `/.ccpkg/pkg-lock.json`.
- Added installed-file integrity verification with `pkg verify`.
- Added `pkg audit` for all installed packages.
- Added `pkg test` for pre-install validation without writing package files.
- Added `pkg inspect` for package metadata, capabilities, and antivirus status.
- Added `pkg doctor` for local and repository health checks.
- Added `pkg recovery` for recovery status.
- Added `pkg clean` for backup and temporary-file cleanup.
- Added `pkg autoremove` for unused dependency cleanup.
- Added package capability reporting.
- Added package license, homepage, README, and changelog metadata support.
- Added repository health reporting.
- Added package size limits before installation.
- Improved path protection and transactional rollback.
- Preserved backups for package removal and recovery.
- Synchronized the GitHub Pages installer with the new client.
- Kept antivirus scanning before any package files are committed.

## Security note

CC:T supports filesystem, network, command, turtle, redstone, and peripheral APIs, including custom peripherals. Static scanning identifies capabilities and suspicious patterns but cannot mathematically prove arbitrary Lua code is safe.
