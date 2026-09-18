# CC PKG repository format

A repository is any HTTP-accessible directory containing `index.json`.

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

Repositories can be hosted on GitHub Pages, a normal web server, or any static file host supporting HTTP(S). No server-side API is required.
