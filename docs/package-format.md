# CC PKG package format

A package can contain multiple versions. Each published version has its own manifest, and the repository index tells CC PKG which computer targets can use that version.

## Repository index

```json
{
  "packages": {
    "example": {
      "name": "example",
      "description": "Example package",
      "versions": {
        "1.0.0": {
          "manifest": "packages/example/1.0.0/package.json",
          "targets": {
            "computer": true
          }
        },
        "1.1.0": {
          "manifest": "packages/example/1.1.0/package.json",
          "targets": {
            "computer": true,
            "pocket": true
          }
        }
      }
    }
  }
}
```

### Target file

Each version directory also contains a `targets.json` file. This is the package-local compatibility declaration requested by the package author:

```json
{
  "version": "1.1.0",
  "targets": {
    "computer": true,
    "pocket": true
  }
}
```

CC PKG reads the repository metadata first, then validates the selected version against the package's `targets.json`. The repository and package-local declarations must agree.

Supported target names:

- `computer` — normal computer
- `pocket` — pocket computer
- `all` — both normal and pocket computers

## Version manifest

Each version has its own `package.json`:

```json
{
  "name": "example",
  "version": "1.1.0",
  "description": "Example package",
  "author": "CubeHub Studio",
  "dependencies": [],
  "files": [
    {
      "source": "example.lua",
      "path": "bin/example"
    }
  ]
}
```

The manifest describes the exact files installed by that version. Different versions can therefore contain completely different implementations.

## Selection

For `pkg install example`, CC PKG:

1. Detects the current computer type.
2. Reads all published versions.
3. Filters versions by repository `targets`.
4. Selects the highest compatible version.
5. Downloads that version's `package.json` and `targets.json`.
6. Verifies that the target is supported.
7. Installs the version's files.
8. Records the selected version and target locally.

