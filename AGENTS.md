# AGENTS.md

## Project Type
Shared GitHub Actions for reuse across repositories.

## Actions

### get-node-version
Reads Node.js version from `package.json.engines.node`.

**Inputs:**
- `path` - Path to package.json (default: `./package.json`)

**Outputs:**
- `version` - Node.js version string

**Usage from external repo:**
```yaml
- uses: vladislavkovaliov/wi-github-actions/.github/actions/get-node-version@main
  with:
    path: ./package.json
```

### env-setup
Sets up Node.js, installs deps, outputs package version.

**Inputs:**
- `package-json-path` - Path to package.json
- `package-manager` - npm, pnpm, or yarn

**Outputs:**
- `package-version` - Version from package.json

**Usage from external repo:**
```yaml
- uses: vladislavkovaliov/wi-github-actions/.github/actions/env-setup@main
  with:
    package-json-path: ./package.json
    package-manager: pnpm
```

## Commands
- `npm run prettier` - Check YAML formatting
- `npm run prettier:fix` - Fix YAML formatting
- `npm run lint` - Run biome check
- `npm run lint:fix` - Fix biome issues
- `npm test` - placeholder (exits with error)

## Commit Workflow
Uses commitizen with cz-adapter for conventional commits.

**Making commits:**
```bash
git add .
npx cz
# Select type (feat, fix, chore, docs, style, refactor, perf, test)
# Enter description
```

**Pre-commit hooks:**
- `.husky/pre-commit` - Runs prettier + biome check

## Development Setup

1. Install deps: `npm install`
2. Initialize husky: `npx husky install` (or `npm run prepare`)
3. Make commits: `npx cz` (instead of `git commit`)

## Release Workflow

Uses semantic-release with conventionalcommits.

**Workflows:**
- `.github/workflows/ci.yml` - Runs on push to main
- `.github/workflows/release-draft.yml` - Manual trigger (workflow_dispatch)
- `.github/workflows/semantic-release/action.yml` - Reusable action

**Flow:**
1. Developer pushes commits to `main`
2. Manually trigger `release-draft.yml` on GitHub
3. Semantic-release analyzes commits
4. Version bump (major/minor/patch)
5. Updates `package.json` version
6. Creates draft release on GitHub

**Branches:** main, dev, rc/*

## Notes
- Each action needs `action.yml` with `name`, `description`, `inputs`, `outputs`, `runs`
- Test actions locally with `act` before publishing