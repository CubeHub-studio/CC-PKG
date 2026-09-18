# CC PKG

CC PKG is a lightweight package manager for CC:Tweaked. It installs packages over HTTP, selects versions based on the computer type, tracks installed packages, and supports dependencies.

## Free Installer

CC PKG is hosted for free with GitHub Pages.

Install CC PKG directly on a CC:Tweaked computer with HTTP enabled:

```lua
wget https://cubehub-studio.github.io/CC-PKG/ pkg
```

The installer endpoint is:

**https://cubehub-studio.github.io/CC-PKG/**

The downloaded client is installed as `/pkg`.

Then run:

```text
pkg help
```

## Update CC PKG itself

CC PKG 0.5.0 includes a self-updater. Run:

```text
pkg self-update
```

It downloads the current `pkg` client from the official repository and replaces `/pkg`.

If you are using an older CC PKG version that does not have `self-update`, reinstall it with:

```lua
delete pkg
wget https://cubehub-studio.github.io/CC-PKG/ pkg
```

## Package commands

```text
pkg help
pkg search <query>
pkg info <package>
pkg install <package>
pkg remove <package>
pkg list
pkg update
pkg upgrade
pkg self-update
pkg repo list
```

`pkg update` currently refreshes repository metadata on demand; package metadata is fetched whenever commands need it. `pkg self-update` is the command for updating the CC PKG client itself. `pkg upgrade` upgrades installed packages.

## Version targeting

Package versions are selected by the version name:

- A version containing `pocket` (case-insensitive) is for pocket computers.
- Every other version name is for regular computers.
- The highest compatible numeric version is selected.

For example:

- `v1.3 pocket` → pocket computer
- `v1.2 advance` → regular computer
- `v1.2 mini` → regular computer
- `v1.3` → regular computer

## Moon BIOS

The official repository currently provides Moon BIOS packages with separate compatible versions. The regular-computer `v1.3` package serves the fixed v1.3 source.

Install it with:

```text
pkg install moon-bios
```

## Local data and logs

CC PKG stores its state in `/.ccpkg/`:

- `config.json` — repository configuration
- `installed.json` — installed package records
- `pkg.log` — normal activity log
- `error.log` — errors

## Community repositories

We kindly ask for you to make your own PKG repositories. Thanks for helping PKG grow! Go to [Repository Documentation](docs/repository.md) for more info.

Users can add a community repository with:

```text
pkg repo add https://example.com/ccpkg/
```

Repository names are provided by each repository's `repometadata.json` file.
