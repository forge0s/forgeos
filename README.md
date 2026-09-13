# forgeos

The main website for ForgeOS, an x86_64 operating system built by hand. Served via GitHub Pages.

Live at: https://forge0s.github.io/forgeos/

## Contents

```
index.html       homepage
packages.html    package listing
docs.html        documentation
packages/        individual package pages
assets/          shared stylesheet
```

## Related repos

- [forgeos-src](https://github.com/forge0s/forgeos-src) — kernel, bootloader, and build scripts
- [forge-packages](https://github.com/forge0s/forge-packages) — package index and `.fpk` files
- Compiled ISO releases are attached to [Releases](https://github.com/forge0s/forgeos/releases) on this repo

## Local preview

These are plain static files — open `index.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```
