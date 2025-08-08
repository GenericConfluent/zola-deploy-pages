# zola-install-action
If you want privacy you're best choice is probably
[shalzz/zola-deploy-action](https://github.com/shalzz/zola-deploy-action/tree/master)

Use to Deploy to Pages Github action to installl zola with the builtin math
branch of cestef. I had some articles that needed this functionality and I don't
want to wait anymore. The zola build is from
[cestef/zola](https://github.com/cestef/zola/tree/feature/math-rendering) at
commit `8f335f1`.

## Usage

This approach also doesn't require creating an extra branch for the deployment
so make sure to use set Pages to `Source: Github Actions`. The downside is that
it doesn't allow for a private site source since the site repo needs to be
public. `actions/upload-pages-artifact@v3` and `actions/deploy-pages@v4` both
operate in the scope of the current repository, and the former in particular
works on some echo magic to work.

```yaml
name: Build and Deploy Site

on:
  push:
    branches:
      - main

  # Allow manual running
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - uses: GenericConfluent/zola-install-action@main

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build with Zola
        id: zola-build
        shell: bash
        run: |
          /usr/local/bin/zola build --base-url "${{ steps.pages.outputs.base_url }}" --minify

      - name: Upload Site Artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## TODO
- [ ] Enable SVG minification
- [ ] Store SVG cache between site builds
