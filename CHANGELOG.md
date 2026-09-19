# CC PKG Changelog

## 0.9.1

- Added the CC-PKG Preset Repository as an automatic built-in repository.
- Added `pkg version`, `device`, `sources`, `files`, `depends`, `why`, `capabilities`, `lock`, `outdated`, and `check` tools.
- Added `pkg repo info`.
- Protected built-in repositories from removal.
- Fixed the existing autoremove variable bug.


- Added multi-package repository support with version directories.
- Added packages/package/versions/version structure support.
- Added automatic package.json discovery inside version directories.
- Added devicedata support for device-specific versions.
- Added automatic device detection.
- Added explicit device identifiers for computers, advanced computers, pocket computers, advanced pocket computers, and noisy pocket computers.
- Added newline-separated devicedata support.
- Preserved compatibility with legacy version-name targeting and direct manifests.
- Migrated the official Moon BIOS catalog to the new structure.
- Updated repository, command, and README documentation.

## 0.8.0

- Added transactional dependency-tree installation.
- Added dependency version constraints.
- Added circular dependency and dependency conflict detection.
- Added SHA-256 hashing and optional manifest file hash verification.
- Added package lock data at /.ccpkg/pkg-lock.json.
- Added installed-file integrity verification with pkg verify.
- Added pkg audit for all installed packages.
- Added pkg test for pre-install validation.
- Added pkg inspect for package metadata, capabilities, and antivirus status.
- Added pkg doctor for local and repository health checks.
- Added pkg recovery, pkg clean, and pkg autoremove.
- Added package capability reporting and metadata support.
- Added repository health reporting.
- Added package size limits and improved path protection.
- Improved transactional rollback and package removal backups.
