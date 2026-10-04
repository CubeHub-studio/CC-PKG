# CC PKG package format

CC PKG supports multiple versions per package. Hardware compatibility is determined **only by the version name**.

## Version-name targeting

The rule is simple:

- A version name containing **"pocket"** (case-insensitive) is for a pocket computer.
- Every other version name is for a regular computer.

Examples:

- `v1.3 pocket` → pocket computer
- `v1.2 advance` → regular computer
- `v1.2 mini` → regular computer
- `v2.0 POCKET` → pocket computer

There is no separate `computer: true` or `pocket: true` flag used for selection.

## Repository index

Each package has a `versions` object. The version key is the published version name.

```json
{
  "packages": {
    "example": {
      "name": "example",
      "description": "Example package",
      "versions": {
        "v1.2 advance": {
          "manifest": "packages/example/1.2.0/package.json"
        },
        "v1.2 mini": {
          "manifest": "packages/example/1.2.0/package.json"
        },
        "v1.3 pocket": {
          "manifest": "packages/example/1.3.0/package.json"
        }
      }
    }
  }
}
```

The manifest's `version` should match the version key.

## Package manifest

Each published version has its own `package.json`:

```json
{
  "name": "example",
  "version": "v1.3 pocket",
  "description": "Example package",
  "author": "CubeHub Studio",
  "dependencies": [],
  "files": [
    {
      "source": "packages/example/1.3.0/example.lua",
      "path": "bin/example"
    }
  ]
}
```

## Selection

When `pkg install example` runs:

1. CC PKG detects whether it is running on a pocket or regular computer.
2. It examines every published version name.
3. Pocket computers accept only versions whose names contain `pocket`.
4. Regular computers reject versions whose names contain `pocket`.
5. The highest compatible numeric version is selected.
6. The selected manifest is downloaded and installed.

This means package authors can publish names such as `v1.2 mini`, `v1.2 advance`, and `v1.3 pocket` without maintaining a separate hardware-target field.


## `.pkg` package instructions

Every package directory may contain a `.pkg` file. CC-PKG treats this as **declarative instructions**, not executable Lua. It is read before installation and can control package metadata behavior.

Example:

```json
{
  "format": 1,
  "type": "cc-pkg-instructions",
  "package": "example",
  "version": "1.2.0",
  "files": "manifest",
  "ignoreFile": ".pkgignore"
}
```

Supported instruction fields:

- `format` — instruction format version.
- `type` — must identify the file as CC-PKG instructions.
- `package` — package name.
- `version` — package/version identifier.
- `files` — currently `"manifest"`, meaning the package manifest supplies the install file list.
- `manifest` — optional alternate manifest path.
- `ignoreFile` — optional name of the ignore file; defaults to `.pkgignore`.

CC-PKG does **not** execute commands from `.pkg`. This keeps package instructions declarative and safer.

## `.pkgignore`

Every package can also contain a `.pkgignore` file. It tells CC-PKG which files from the package payload should be ignored.

Rules are line-based:

- Blank lines are ignored.
- Lines beginning with `#` are comments.
- A pattern can match a complete path or filename.
- `*` matches any sequence of characters.
- A directory pattern also ignores files below that directory.

Example:

```text
# Package metadata is not an installation payload
.pkg
.pkgignore
package.json
devicedata
*.bak
docs/
```

Ignored files are removed from the installation plan before download and installation. Package authors should therefore only ignore files that are not required at runtime.

For versioned packages, the `.pkg` and `.pkgignore` files can be placed in each version directory so each published version can have its own instructions.
