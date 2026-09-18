# CC PKG architecture

CC PKG has three main components:

1. **Client** - the `pkg` Lua program running on CC:Tweaked.
2. **Repository** - an HTTP directory containing `index.json`.
3. **Package** - a manifest plus the files it installs.

## Local state

- `/.ccpkg/config.json` stores repository configuration.
- `/.ccpkg/installed.json` stores installed package metadata.

## Installation flow

1. Load configured repositories.
2. Download a repository index.
3. Find the requested package.
4. Download its manifest.
5. Install dependencies recursively.
6. Download package files.
7. Record installed files and version.

## Design constraints

- No external Lua libraries.
- Standard CC:Tweaked APIs.
- HTTP-based and suitable for static hosting.
- Removal uses the file list recorded at installation time.
