# Continuous integration contract

Websonette keeps shared CI orchestration in the organization `.github` repository. Each package owns its commands, tool configuration, and tests.

## PHP packages

A PHP package calls the shared workflow from `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  ci:
    uses: Websonette/.github/.github/workflows/php-package.yml@main
```

The package must expose a `composer test` script that runs all checks required for merging. This normally includes automated tests, static analysis, and coding-standard validation. The exact tools and their configuration remain inside the package repository.

Supported PHP versions can be overridden by the caller:

```yaml
jobs:
  ci:
    uses: Websonette/.github/.github/workflows/php-package.yml@main
    with:
      php-versions: '["8.3", "8.4"]'
```

## Node packages

A Node or Vite package calls the shared Node workflow:

```yaml
jobs:
  ci:
    uses: Websonette/.github/.github/workflows/node-package.yml@main
```

The shared workflow always runs `npm ci`. It then runs the `test`, `typecheck`, and `build` scripts when they are present.

## Versioning the workflow

Callers use `@main` while the CI contract is being established. Once it is proven on the first real packages, the workflow will be tagged as `v1` and package repositories should use that stable major tag.

Reusable workflows receive read-only repository permissions by default. A publishing workflow must be separate and request only the additional permissions it needs.

