# Querio Sync

## Description

This GitHub [action](https://github.com/Querio-App/sync/blob/main/action.yml) syncs committed context files from your repository to a Querio workspace.

## Usage

Generate an access token in Querio and save it as a GitHub Actions secret named `QUERIO_ACCESS_TOKEN` in your repository. Replace `<workspace-id>` below with your destination workspace ID.

Add the following workflow to `.github/workflows/querio-sync.yml` to sync your context whenever you push to `main`:

```yaml
name: Sync Querio context

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: querio-context-sync
  cancel-in-progress: false

jobs:
  sync:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4

      - name: Sync committed context to Querio
        uses: Querio-App/sync@main
        with:
          token: ${{ secrets.QUERIO_ACCESS_TOKEN }}
          workspace-id: <workspace-id>
```
