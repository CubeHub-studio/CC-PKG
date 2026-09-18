# CC PKG package format

Each package has a `package.json` manifest.

Example:

```json
{
  "name": "hello",
  "version": "1.0.0",
  "description": "Example CC:Tweaked package",
  "author": "CubeHub Studio",
  "dependencies": [],
  "files": [
    {
      "source": "hello.lua",
      "path": "bin/hello"
    }
  ]
}
```

Fields:

- `name`: unique package identifier.
- `version`: package version.
- `description`: human-readable description.
- `author`: package author.
- `dependencies`: package names that must be installed first.
- `files`: files copied to the CC computer. `source` is the repository path and `path` is the destination.

The initial implementation uses exact dependency names. Version ranges and optional dependencies can be added later.
