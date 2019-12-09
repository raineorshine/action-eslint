# Lint changed Pull Request files with ESLint from GitHub Action

Using this GitHub Action, scan files changed in current Pull Request with inline code annotations:

<img src="./images/annotations.png">

## Usage

The workflow, usually declared in `.github/workflows/lint.yml`, looks like:

```yml
name: Lint

on: pull_request

jobs:
  eslint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
        with:
          fetch-depth: 1
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: rm -f .yarnclean
      - run: yarn --frozen-lockfile --ignore-engines --ignore-optional --no-bin-links --non-interactive --silent --ignore-scripts --production=false
        env:
          PUPPETEER_SKIP_CHROMIUM_DOWNLOAD: true
          HUSKY_SKIP_INSTALL: true
      # Alternative: if you use npm instead of yarn
      # - run: npm ci --no-audit --prefer-offline
      - uses: tinovyatkin/action-eslint@v1
        env:
          GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
        with:
          repo-token: ${{secrets.GITHUB_TOKEN}}
          check-name: eslint # this is the check name from above 👆 where to post annotations
```

<img src="./images/check.png">

## License

All scripts and documentation in this project are released under the MIT License.
