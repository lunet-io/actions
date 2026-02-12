# `lunet-io/actions`

Reusable GitHub Actions for Lunet-based workflows.

## Available actions

- [`lunet-build`](./lunet-build/README.md): Checkout a repository, install Lunet, build a site, configure Pages, upload the Pages artifact, and optionally deploy to GitHub Pages.

## Quick usage

### Minimal: build another repository

```yaml
- name: Build Lunet site
  uses: lunet-io/actions/lunet-build@v1
  with:
    repository: XenoAtom/XenoAtom.Terminal.UI
    site_path: site
```

### Build current repository (defaults only)

```yaml
- name: Build Lunet site
  uses: lunet-io/actions/lunet-build@v1
```

## Versioning

Use stable major tags (for example `v1`) in consumer repositories instead of `@main`.
