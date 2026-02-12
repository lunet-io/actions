# `lunet-build` action

Builds a Lunet website from any repository with GitHub Pages-ready defaults.

By default, this action:
- checks out the source repository,
- installs .NET and Lunet,
- runs `lunet build` in `site/`,
- runs `actions/configure-pages`,
- uploads `.lunet/build/www` as Pages artifact.

By default, this action also runs `actions/deploy-pages` after uploading the artifact.
Set `deploy_pages: false` to skip deployment and only upload the artifact.

## Minimal usage

### Build another repository

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

### Build and deploy in one job

```yaml
jobs:
  site:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.lunet.outputs.page_url }}
    steps:
      - id: lunet
        name: Build and deploy Lunet site
        uses: lunet-io/actions/lunet-build@v1
        with:
          repository: XenoAtom/XenoAtom.CommandLine
          site_path: site
          deploy_pages: true
```

## Inputs

- `repository` (optional): Repository in `owner/name` format. Default: `${{ github.repository }}`.
- `ref` (optional): Branch, tag, or SHA to checkout.
- `token` (optional): Token for checkout. Default: `${{ github.token }}`.
- `checkout_path` (optional): Folder where the repository is checked out. Default: `source`.
- `submodules` (optional): Checkout submodules mode. Default: `recursive`.
- `fetch_depth` (optional): Checkout depth. Default: `0`.
- `dotnet_version` (optional): .NET SDK version. Default: `9.0.x`.
- `lunet_version` (optional): Lunet tool version range. Default: `0.9.*`.
- `site_path` (optional): Site folder path in the checked out repo. Default: `site`.
- `build_command` (optional): Build command run in `site_path`. Default: `lunet build`.
- `output_path` (optional): Output folder path relative to `site_path`. Default: `.lunet/build/www`.
- `setup_pages` (optional): Run `actions/configure-pages`. Default: `true`.
- `upload_pages_artifact` (optional): Run `actions/upload-pages-artifact` on `output_path`. Default: `true`.
- `deploy_pages` (optional): Run `actions/deploy-pages` after upload. Requires `pages: write` and `id-token: write`. Default: `true`.

## Outputs

- `repository`: Repository actually built.
- `checkout_directory`: Absolute checkout path.
- `site_directory`: Absolute site path.
- `output_directory`: Absolute Lunet output path.
- `page_url`: GitHub Pages URL from deployment when `deploy_pages` is enabled.
