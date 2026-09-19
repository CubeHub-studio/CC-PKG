# CC PKG

CC PKG is a lightweight package manager for CC:Tweaked. It installs packages over HTTP, selects device-specific versions, resolves dependencies, verifies package integrity, scans packages before installation, tracks installed packages, and supports transactional rollback.

## Free Installer

Install CC-PKG directly on a CC:Tweaked computer with HTTP enabled:

~~~lua
wget https://cubehub-studio.github.io/CC-PKG/ pkg
~~~

The downloaded client is installed as /pkg.

## Package repository structure

A repository can contain many packages:

~~~text
repository/
├── repometadata.json
├── index.json
└── packages/
    ├── package-a/
    │   ├── package.json
    │   └── versions/
    │       ├── 1.0.0/
    │       │   ├── devicedata
    │       │   ├── package.json
    │       │   └── ...
    │       └── 1.1.0/
    │           ├── devicedata
    │           ├── package.json
    │           └── ...
    └── package-b/
        ├── package.json
        └── versions/
            └── 1.0.0/
                ├── devicedata
                ├── package.json
                └── ...
~~~

A repository is a package collection. A package can have many versions, and each version can target different CC:Tweaked hardware.

## Device-specific versions

Each new-style version has a devicedata file.

Example:

~~~json
{
  "devices": [
    "advanced_pocket_computer",
    "noisy_pocket_computer"
  ]
}
~~~

Supported identifiers:

- computer
- advanced_computer
- pocket_computer
- advanced_pocket_computer
- noisy_pocket_computer

CC-PKG detects the current device, reads devicedata, filters incompatible versions, and selects the newest compatible version. CC:Tweaked documents the pocket API as pocket-only and documents colour support on advanced computers. citeturn0search3turn7search0

## Moon BIOS

The official repository now demonstrates the new format with Moon BIOS:

~~~text
packages/moon-bios/
├── package.json
└── versions/
    ├── 1.2.0/
    │   ├── devicedata
    │   └── package.json
    ├── 1.2-advance/
    │   ├── devicedata
    │   └── package.json
    ├── 1.3.0/
    │   ├── devicedata
    │   └── package.json
    └── 1.3-pocket/
        ├── devicedata
        └── package.json
~~~

Install normally:

~~~text
pkg install moon-bios
~~~

CC-PKG chooses the compatible Moon BIOS version automatically.

## Community repositories

Create your own repository and publish it over HTTP or HTTPS. GitHub Pages works well because it serves static files using the repository's directory structure. citeturn6search0turn6search1

Users add repositories with:

~~~text
pkg repo add https://example.com/ccpkg/
~~~

See docs/repository.md for the complete repository specification.

## Security and integrity

CC-PKG performs pre-install scanning across the dependency tree before package files are written. It validates package paths, checks supplied SHA-256 hashes, records installed hashes in /.ccpkg/pkg-lock.json, detects suspicious capabilities, and uses backups for transactional rollback.

Useful commands include:

~~~text
pkg inspect <package>
pkg test <package>
pkg verify <package>
pkg audit
pkg doctor
~~~

Only install packages and repositories you trust.
