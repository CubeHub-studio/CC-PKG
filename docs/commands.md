# CC-PKG Commands

Complete command reference for CC-PKG 0.8.0.

## Package commands

| Command | Description |
|---|---|
| `pkg search <query>` | Search available packages. |
| `pkg info <package>` | Show package metadata. |
| `pkg inspect <package>` | Inspect files, capabilities, metadata, and antivirus status. |
| `pkg test <package>` | Validate a package and its dependency tree without installing it. |
| `pkg install <package>` | Install a package and its dependencies transactionally. |
| `pkg remove <package> [--force]` | Remove a package. |
| `pkg list` | List installed packages. |
| `pkg verify <package>` | Verify installed files against recorded SHA-256 hashes. |
| `pkg audit` | Verify every installed package. |
| `pkg upgrade` | Upgrade installed packages. |
| `pkg autoremove` | Remove non-explicit packages that are no longer required. |
| `pkg restore <package>` | Restore a package from its backup. |
| `pkg clean` | Remove CC-PKG backups and temporary data. |

## Repository commands

| Command | Description |
|---|---|
| `pkg update` | Refresh/check configured repository metadata and health. |
| `pkg repo list` | List configured repositories. |
| `pkg repo add <url>` | Add a community package repository. |
| `pkg repo remove <name>` | Remove a community package repository. |
| `pkg repo update` | Check configured repository health. |

## Maintenance and diagnostics

| Command | Description |
|---|---|
| `pkg doctor` | Diagnose CC-PKG configuration, repositories, antivirus signatures, and installed packages. |
| `pkg recovery` | Show CC-PKG recovery status and recovery options. |
| `pkg self-update` | Update the CC-PKG client itself. |
| `pkg help` | Show a link to this command reference. |

## Installation

Install CC-PKG with:

```lua
wget https://cubehub-studio.github.io/CC-PKG/ pkg
```
