# CC PKG repository format

A CC PKG repository is any HTTP-accessible directory containing `index.json` and `repometadata.json`.

Repositories can be hosted on GitHub Pages, a normal web server, or any static file host supporting HTTP(S). No server-side API is required.

## Repository metadata

Every repository must have a file named `repometadata.json` at its root.

Example:

```json
{
  "name": "My Repository"
}
```

The `name` field is the repository display name. CC PKG uses this name when listing and storing the repository.

## Repository index

Every repository must also have an `index.json` file.

Example:

```json
{
  "name": "My Repository",
  "version": 1,
  "packages": {
    "hello": {
      "name": "hello",
      "description": "A small example package",
      "manifest": "packages/hello/package.json"
    }
  }
}
```

The client downloads the manifest and then downloads each file listed by the manifest.

## Adding a repository

Users add a repository using only its URL:

```text
pkg repo add https://example.com/ccpkg/
```

CC PKG automatically downloads `repometadata.json` to obtain the repository name, then checks `index.json` before adding the repository.

Do not ask users to provide a separate repository name.

## Repository structure

```text
ccpkg/
├── repometadata.json
├── index.json
└── packages/
    └── hello/
        └── package.json
```

## Publishing checklist

Before publishing your repository, make sure:

1. `repometadata.json` exists at the repository root.
2. `repometadata.json` contains a non-empty `name`.
3. `index.json` is valid JSON.
4. Every listed package has a valid manifest.
5. Every manifest file source is reachable over HTTP(S).
6. Your repository is accessible from CC:Tweaked with HTTP enabled.

Community repositories are independently hosted and maintained. Users should only add repositories they trust, because package files can execute Lua code on their computer.

## Hosting

GitHub Pages is a convenient free option for static CC PKG repositories. A normal web server or another static HTTP(S) host also works.
