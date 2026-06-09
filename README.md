# wi-github-actions

Shared GitHub Actions for reuse across repositories.

## Usage

Reference any action using the format:

```yaml
uses: vladislavkovaliov/wi-github-actions/.github/actions/<action-name>@main
```

## Actions

### `get-node-version`

Reads Node.js version from `package.json` → `engines.node`.

**Inputs:**

| Name | Required | Default | Description |
|---|---|---|---|
| `path` | No | `./package.json` | Path to package.json |

**Outputs:**

| Name | Description |
|---|---|
| `version` | Node.js version string |

**Example:**

```yaml
- uses: vladislavkovaliov/wi-github-actions/.github/actions/get-node-version@main
  id: node-version
  with:
    path: ./package.json

- name: Use Node.js
  run: echo "Node ${{ steps.node-version.outputs.version }}"
```

### `env-setup`

Sets up Node.js (with caching), installs dependencies, and outputs the package version.

**Inputs:**

| Name | Required | Default | Description |
|---|---|---|---|
| `package-json-path` | No | `./package.json` | Path to package.json |
| `package-manager` | No | `npm` | Package manager: `npm`, `pnpm`, or `yarn` |

**Outputs:**

| Name | Description |
|---|---|
| `package-version` | Version from package.json |

**Example:**

```yaml
- uses: vladislavkovaliov/wi-github-actions/.github/actions/env-setup@main
  with:
    package-json-path: ./package.json
    package-manager: pnpm
```

### `run-tests`

Runs unit tests with Jest or Vitest.

**Inputs:**

| Name | Required | Default | Description |
|---|---|---|---|
| `runner` | No | `jest` | Test runner: `jest` or `vitest` |
| `coverage` | No | `false` | Enable coverage (`true`/`false`) |
| `args` | No | `""` | Additional CLI flags |
| `workdir` | No | `.` | Working directory |

**Examples:**

```yaml
# Default (Jest)
- uses: vladislavkovaliov/wi-github-actions/.github/actions/run-tests@main

# Vitest with coverage
- uses: vladislavkovaliov/wi-github-actions/.github/actions/run-tests@main
  with:
    runner: vitest
    coverage: true

# Vitest in a monorepo sub-package
- uses: vladislavkovaliov/wi-github-actions/.github/actions/run-tests@main
  with:
    runner: vitest
    coverage: true
    workdir: ./packages/frontend
    args: --reporter=verbose
```

## Full Workflow Example

```yaml
name: CI

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: vladislavkovaliov/wi-github-actions/.github/actions/env-setup@main
        with:
          package-manager: pnpm
      - uses: vladislavkovaliov/wi-github-actions/.github/actions/run-tests@main
        with:
          runner: vitest
          coverage: true
```
