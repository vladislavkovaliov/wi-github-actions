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
- `get-node-version-action` - Path to get-node-version action
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
- `npm test` - placeholder (exits with error)

## Notes
- Each action needs `action.yml` with `name`, `description`, `inputs`, `outputs`, `runs`
- Test actions locally with `act` before publishing