# workflows

Shared GitHub Actions workflows for marvinvr's apps. Public so that public and private repos can call them. No secrets in here.

## build-image

Builds the repo's Dockerfile and pushes `ghcr.io/marvinvr/<app>:sha-<commit>` and `:latest`. Optionally triggers the deploy in `marvinvr/prod`.

`.github/workflows/image.yml` in an app repo:

```yaml
name: image
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  packages: write
jobs:
  image:
    uses: marvinvr/workflows/.github/workflows/build-image.yml@main
    with:
      app: portfolio          # name in marvinvr/prod
      context: .
      dockerfile: Dockerfile
      deploy: ${{ vars.PROD_AUTO_DEPLOY == 'true' }}
    secrets:
      SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
      PROD_DEPLOY_TOKEN: ${{ secrets.PROD_DEPLOY_TOKEN }}
```

App repo settings: secret `PROD_DEPLOY_TOKEN` (fine-grained, only marvinvr/prod, Actions: read/write), variable `PROD_AUTO_DEPLOY=true` to deploy after each build, optional secret `SENTRY_AUTH_TOKEN` (BuildKit secret `id=SENTRY_AUTH_TOKEN`).
