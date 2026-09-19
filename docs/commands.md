# CC-PKG Commands

Complete command reference for CC-PKG 0.9.1.

## Package commands

| Command | Description |
|---|---|
| pkg search <query> | Search available packages. |
| pkg info <package> | Show package metadata and the selected device target. |
| pkg inspect <package> | Inspect files, capabilities, metadata, device targeting, and antivirus status. |
| pkg test <package> | Validate a package and its dependency tree without installing it. |
| pkg install <package> | Install the newest version compatible with the current device and its dependencies. |
| pkg remove <package> [--force] | Remove a package. |
| pkg list | List installed packages. |
| pkg verify <package> | Verify installed files against recorded SHA-256 hashes. |
| pkg audit | Verify every installed package. |
| pkg upgrade | Upgrade installed packages to the newest compatible versions. |
| pkg autoremove | Remove non-explicit packages that are no longer required. |
| pkg restore <package> | Restore a package from its backup. |
| pkg clean | Remove CC-PKG backups and temporary data. |

## Information and maintenance tools

| Command | Description |
|---|---|
| `pkg version` | Show the CC-PKG client version. |
| `pkg device` | Show detected CC:Tweaked device types. |
| `pkg sources [package]` | Show configured repositories or the source of an installed package. |
| `pkg files <package>` | List files owned by an installed package. |
| `pkg depends <package>` | Show an installed package's dependencies. |
| `pkg why <package>` | Show installed packages that depend on a package. |
| `pkg capabilities <package>` | Show recorded package capabilities. |
| `pkg lock` | Show the installed package lock state. |
| `pkg outdated` | Find installed packages with newer compatible versions. |
| `pkg check [package]` | Verify one package or audit all installed packages. |

## Repository commands

| Command | Description |
|---|---|
| pkg update | Refresh/check configured repository metadata and health. |
| pkg repo list | List configured repositories. |
| pkg repo add <url> | Add a community package repository. |
| pkg repo remove <name> | Remove a community package repository. |
| pkg repo update | Check configured repository health. |

## Maintenance and diagnostics

| Command | Description |
|---|---|
| pkg doctor | Diagnose CC-PKG configuration, repositories, antivirus signatures, and installed packages. |
| pkg recovery | Show CC-PKG recovery status and recovery options. |
| pkg self-update | Update the CC-PKG client itself. |
| pkg help | Show a link to this command reference. |

## Built-in repositories

CC-PKG automatically configures the official repository and the CC-PKG Preset Repository. Built-in repositories cannot be removed.

## Version and device selection

New-style repositories use:

~~~text
packages/<package>/versions/<version>/
├── devicedata
├── package.json
└── package files...
~~~

devicedata declares the supported device identifiers. CC-PKG detects the current device and selects the highest compatible version.

Example:

~~~json
{
  "devices": [
    "computer",
    "advanced_computer"
  ]
}
~~~

Legacy repositories using version-name targeting remain supported.

## Installation

Install CC-PKG with:

~~~lua
wget https://cubehub-studio.github.io/CC-PKG/ pkg
~~~
