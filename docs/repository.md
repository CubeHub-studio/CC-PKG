# Creating a CC PKG Repository

This guide explains how to make your own package repository for CC PKG.

You do **not** need to run a server. A normal static website host is enough.

## 1. Make a repository

Create a folder for your package repository.

A simple repository looks like this:

~~~text
my-pkg-repo/
├── repometadata.json
├── index.json
└── packages/
    └── hello/
        └── package.json
~~~

You can use GitHub Pages to host this for free.

## 2. Create `repometadata.json`

Create a file named exactly:

~~~text
repometadata.json
~~~

Put it in the root of your repository.

Example:

~~~json
{
  "name": "My Packages"
}
~~~

The `name` field is the name CC PKG will show for your repository.

You do **not** give the repository name to the user. CC PKG gets it automatically from `repometadata.json`.

## 3. Create `index.json`

Create a file named exactly:

~~~text
index.json
~~~

This file tells CC PKG which packages are available.

Example:

~~~json
{
  "name": "My Packages",
  "version": 1,
  "packages": {
    "hello": {
      "name": "hello",
      "description": "A small example package",
      "manifest": "packages/hello/package.json"
    }
  }
}
~~~

The package name is `hello`. The manifest path is `packages/hello/package.json`. Paths are relative to your repository URL.

## 4. Create the package manifest

Create:

~~~text
packages/hello/package.json
~~~

Example:

~~~json
{
  "name": "hello",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "A small example package",
  "license": "MIT",
  "homepage": "https://example.com/",
  "readme": "README.md",
  "changelog": "CHANGELOG.md",
  "dependencies": {
    "library": ">=1.0.0 <2.0.0"
  },
  "files": [
    {
      "path": "hello",
      "source": "packages/hello/hello",
      "sha256": "PUT_THE_FILE_SHA256_HERE"
    }
  ]
}
~~~

The manifest tells CC PKG which files to download.

## 5. Add the package files

For the example above, create:

~~~text
packages/hello/hello
~~~

Put your Lua program in that file:

~~~lua
print("Hello from my package!")
~~~

When installed, this file is downloaded to `/hello` because the manifest uses `"path": "hello"`.

## 6. Finished repository

Your repository should now look like:

~~~text
my-pkg-repo/
├── repometadata.json
├── index.json
└── packages/
    └── hello/
        ├── package.json
        └── hello
~~~

## 7. Host the repository

Your repository must be available over HTTP or HTTPS.

GitHub Pages is a free option.

For example, if your GitHub Pages address is:

~~~text
https://yourname.github.io/my-pkg-repo/
~~~

these files must be reachable:

~~~text
https://yourname.github.io/my-pkg-repo/repometadata.json
https://yourname.github.io/my-pkg-repo/index.json
https://yourname.github.io/my-pkg-repo/packages/hello/package.json
https://yourname.github.io/my-pkg-repo/packages/hello/hello
~~~

## 8. Add your repository to CC PKG

Users only need your repository URL:

~~~text
pkg repo add https://yourname.github.io/my-pkg-repo/
~~~

CC PKG automatically:

1. Downloads `repometadata.json`.
2. Reads the repository name.
3. Downloads `index.json`.
4. Checks that the repository works.
5. Adds the repository.

Users can see their repositories with:

~~~text
pkg repo list
~~~

Do not ask users to provide a separate repository name.

## 9. Install your package

After adding your repository:

~~~text
pkg install hello
~~~

CC PKG finds `hello` in `index.json`, downloads its manifest, and downloads the files listed in that manifest.

## Adding more packages

You can put many packages in one repository.

Example:

~~~text
my-pkg-repo/
├── repometadata.json
├── index.json
└── packages/
    ├── hello/
    │   ├── package.json
    │   └── hello
    ├── calculator/
    │   ├── package.json
    │   └── calculator
    └── my-os/
        ├── package.json
        └── startup
~~~

Add each package to `index.json`.

## Multiple versions

You can provide different versions of a package.

A version name containing `pocket` is treated as a pocket-computer version. Other version names are treated as regular-computer versions.

Example:

~~~json
{
  "name": "my-os",
  "description": "My operating system",
  "versions": {
    "v1.0": {
      "manifest": "packages/my-os/1.0/package.json"
    },
    "v1.1": {
      "manifest": "packages/my-os/1.1/package.json"
    },
    "v1.1 pocket": {
      "manifest": "packages/my-os/1.1-pocket/package.json"
    }
  }
}
~~~

A regular computer can select `v1.1`. A pocket computer can select `v1.1 pocket`.

## Dependencies

A package can require other packages.

Example:

~~~json
{
  "name": "my-app",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "An application that needs another package",
  "dependencies": {
    "library": ">=1.0.0 <2.0.0"
  },
  "files": [
    {
      "path": "my-app",
      "source": "packages/my-app/my-app"
    }
  ]
}
~~~

When the user runs `pkg install my-app`, CC PKG resolves the dependency tree, checks version constraints, downloads the complete tree, scans it before writing anything, verifies any supplied SHA-256 hashes, then commits the installation transaction.

## Package integrity and metadata

Each file may optionally include a SHA-256 hash:

~~~json
{
  "path": "hello",
  "source": "packages/hello/hello",
  "sha256": "..."
}
~~~

CC PKG verifies supplied hashes before installation and records downloaded hashes in `/.ccpkg/pkg-lock.json`. The lock file records exact installed versions, repositories, targets and file hashes.

Optional manifest metadata includes:

~~~json
{
  "license": "MIT",
  "homepage": "https://example.com/",
  "readme": "README.md",
  "changelog": "CHANGELOG.md"
}
~~~

## Client diagnostics

Useful commands include:

~~~text
pkg inspect hello
pkg test hello
pkg verify hello
pkg audit
pkg doctor
pkg recovery
pkg clean
pkg autoremove
~~~

`pkg test` validates a package and its dependency tree without installing it. `pkg inspect` shows metadata, detected capabilities and antivirus status. `pkg verify` checks installed files against recorded hashes. `pkg audit` checks every installed package. `pkg doctor` checks PKG state and repository health.

## Updating packages

When you publish a newer version, update `index.json` and add the new manifest and files.

Users can then run:

~~~text
pkg upgrade
~~~

## Testing

Before sharing your repository, check that `repometadata.json` and `index.json` work in a web browser.

Then test in CC:Tweaked:

~~~text
pkg repo add https://yourname.github.io/my-pkg-repo/
pkg repo list
pkg search hello
pkg install hello
~~~

If `pkg repo add` fails, check that:

- `repometadata.json` exists at the repository root.
- `repometadata.json` is valid JSON.
- `repometadata.json` contains a non-empty `name`.
- `index.json` exists and is valid JSON.
- Every package manifest exists.
- Every package file is reachable.
- CC:Tweaked HTTP access is enabled.

## Security

A package can contain Lua code that runs on a user's CC:Tweaked computer. CC PKG now performs pre-install antivirus scanning, path validation, dependency-tree validation, optional SHA-256 verification, and transactional installation with rollback backups.

The scanner reports suspicious capabilities such as filesystem access, network access, commands, redstone, turtles, peripherals and program execution. These are capability indicators, not proof that a package is malicious. CC:Tweaked also supports custom peripherals, so static scanning cannot prove arbitrary Lua is safe. See the official CC:Tweaked documentation for the API and peripheral model.

Only install repositories and packages you trust.

Community repositories are independently maintained. Creating a repository does not make it an official CC PKG repository.

Have fun making packages, and thanks for helping the CC PKG community grow! 🚀