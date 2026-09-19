# Creating a CC PKG Repository

This guide defines the CC-PKG repository format, including multiple packages, version directories, and device-specific versions.

## Default preset repository

CC-PKG includes the CubeHub Studio CC-PKG Preset Repository automatically. It is intended for curated starter packages and useful presets.

Repository:
~~~text
https://raw.githubusercontent.com/CubeHub-studio/CC-PKG-Preset-Repository/main/
~~~

It is configured as the built-in `presets` repository and does not need to be added manually.

## Repository layout

A repository can contain any number of packages:

~~~text
my-pkg-repo/
├── repometadata.json
├── index.json
└── packages/
    ├── moon-bios/
    │   ├── package.json
    │   └── versions/
    │       ├── 1.0.0/
    │       │   ├── devicedata
    │       │   ├── package.json
    │       │   └── startup
    │       └── 1.1.0/
    │           ├── devicedata
    │           ├── package.json
    │           └── startup
    └── hello/
        ├── package.json
        └── versions/
            └── 1.0.0/
                ├── devicedata
                ├── package.json
                └── hello
~~~

The packages directory is the package collection. Each package has its own directory, and each package can contain any number of versions.

## index.json

Example:

~~~json
{
  "name": "My Packages",
  "version": 1,
  "packages": {
    "hello": {
      "name": "hello",
      "description": "A small example package",
      "versions": {
        "1.0.0": {
          "path": "packages/hello/versions/1.0.0/"
        },
        "1.1.0": {
          "path": "packages/hello/versions/1.1.0/"
        }
      }
    }
  }
}
~~~

For a version directory, path points to the directory containing that version.

CC-PKG automatically looks for the version's package.json and devicedata. A manifest or devicedata path may also be supplied explicitly.

## Package metadata

Create packages/hello/package.json:

~~~json
{
  "name": "hello",
  "description": "A small example package",
  "author": "Your Name",
  "license": "MIT"
}
~~~

This is package-level metadata. Version-specific installation data belongs inside the version directory.

## Version manifest

Create packages/hello/versions/1.0.0/package.json:

~~~json
{
  "name": "hello",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "A small example package",
  "dependencies": {},
  "files": [
    {
      "path": "hello",
      "source": "packages/hello/versions/1.0.0/hello",
      "sha256": "PUT_THE_FILE_SHA256_HERE"
    }
  ]
}
~~~

The version manifest tells CC-PKG which files to install.

## devicedata

Every new-style version should contain a file named devicedata.

Recommended JSON:

~~~json
{
  "devices": [
    "advanced_pocket_computer",
    "noisy_pocket_computer"
  ]
}
~~~

Supported device identifiers are:

~~~text
computer
advanced_computer
pocket_computer
advanced_pocket_computer
noisy_pocket_computer
~~~

Device matching is explicit. If a version supports several device types, list all of them.

A version supporting every pocket device can use:

~~~json
{
  "devices": [
    "pocket_computer",
    "advanced_pocket_computer",
    "noisy_pocket_computer"
  ]
}
~~~

CC-PKG detects pocket computers through the CC:Tweaked pocket API and distinguishes advanced terminals using terminal colour support. The pocket API is only available on pocket computers, and advanced computers support colour output. citeturn0search3turn7search0

devicedata may also be a simple newline-separated list:

~~~text
computer
advanced_computer
~~~

Blank lines and lines beginning with # are ignored.

### Noisy pocket computers

noisy_pocket_computer is an optional CC-PKG device identifier for environments which expose a speaker peripheral to the pocket computer. CC:Tweaked exposes speakers through the peripheral system. citeturn2search3

Because modded devices can expose different APIs, repository maintainers should list every device target they have actually tested.

## Version selection

When a user runs:

~~~text
pkg install hello
~~~

CC-PKG:

1. Finds the package.
2. Examines its versions.
3. Reads devicedata.
4. Detects the current device.
5. Discards incompatible versions.
6. Applies dependency constraints.
7. Selects the highest compatible numeric version.
8. Downloads the selected manifest and files.
9. Performs validation and antivirus scanning.
10. Installs the package transactionally.

The user does not need to manually choose the hardware version.

## Multiple device-specific versions

A package can have completely different implementations:

~~~text
packages/
└── moon-bios/
    ├── package.json
    └── versions/
        ├── 1.2.0/
        │   ├── devicedata
        │   ├── package.json
        │   └── startup
        ├── 1.2.0-advance/
        │   ├── devicedata
        │   ├── package.json
        │   └── startup
        ├── 1.3.0/
        │   ├── devicedata
        │   ├── package.json
        │   └── startup
        └── 1.3.0-pocket/
            ├── devicedata
            ├── package.json
            └── startup
~~~

The directory name is a repository identifier. The version field in package.json is the version used for dependency comparison.

## Dependencies and integrity

Version manifests support dependency constraints and optional SHA-256 hashes:

~~~json
{
  "dependencies": {
    "library": ">=1.0.0 <2.0.0"
  },
  "files": [
    {
      "path": "hello",
      "source": "packages/hello/versions/1.0.0/hello",
      "sha256": "..."
    }
  ]
}
~~~

CC-PKG resolves the dependency tree, validates paths, downloads the tree, scans it, verifies supplied hashes, and commits the installation transaction.

## Hosting

The repository must be reachable over HTTP or HTTPS. GitHub Pages works well because it serves static files using the repository's directory structure. citeturn6search0turn6search1

Users add a repository with:

~~~text
pkg repo add https://yourname.github.io/my-pkg-repo/
~~~

## Updating

Add a new version directory and add it to index.json. Users can then run:

~~~text
pkg upgrade
~~~

CC-PKG selects the newest compatible version.

## Legacy compatibility

CC-PKG 0.9.1 still supports older repositories using direct manifest entries and version names such as v1.3 pocket. New repositories should use version directories and devicedata.

## Security

CC-PKG performs path validation, dependency validation, optional SHA-256 verification, antivirus scanning, and transactional installation with rollback backups.

Only install repositories and packages you trust.
