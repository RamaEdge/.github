# Shared GitHub Actions Documentation

This document describes the reusable GitHub Actions and workflows available in the ramaedge organization.

## Overview

This repository contains:
- **Composite Actions** - Reusable action steps for common tasks
- **Reusable Workflows** - Complete workflow definitions that can be called from other repos

## Composite Actions

### `actions/trivy-scan`

Installs Trivy and scans container images for vulnerabilities, uploading results to GitHub Advanced Security.

**Usage:**
```yaml
steps:
  - uses: ramaedge/.github/actions/trivy-scan@main
    with:
      images: "myregistry.com/app:v1.0.0 myregistry.com/api:v1.0.0"
      severity: "CRITICAL,HIGH"
```

**Inputs:**
| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `images` | Space-separated list of images to scan | Yes | - |
| `severity` | Severity levels to report | No | `CRITICAL,HIGH` |
| `output-dir` | Directory to store SARIF results | No | `trivy-results` |
| `upload-to-ghas` | Upload results to GitHub Security | No | `true` |

### `actions/setup-python-env`

Sets up Python environment with pip, build tools, and dependencies.

**Usage:**
```yaml
steps:
  - uses: ramaedge/.github/actions/setup-python-env@main
    with:
      python-version: '3.12'
      editable-paths: 'libs/common libs/utils'
      install-dev: 'true'
```

**Inputs:**
| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `python-version` | Python version to use | No | `3.12` |
| `install-dev` | Install dev dependencies | No | `true` |
| `extra-packages` | Additional packages to install | No | - |
| `editable-paths` | Paths to install in editable mode | No | - |

## Reusable Workflows

### `reusable-container-build.yml`

Builds, scans, and pushes container images using Podman.

**Usage:**
```yaml
jobs:
  build:
    uses: ramaedge/.github/.github/workflows/reusable-container-build.yml@main
    with:
      registry: harbor.example.com/project
      registry-host: harbor.example.com
      services: "service1 service2 service3"
    secrets: inherit
```

**Inputs:**
| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Container registry path | Yes | - |
| `registry-host` | Container registry host for login | Yes | - |
| `services` | Space-separated list of services | Yes | - |
| `containerfile-path` | Path pattern to Containerfile | No | `services/{service}/Containerfile` |
| `build-context` | Build context directory | No | `.` |
| `trivy-severity` | Trivy severity levels | No | `CRITICAL,HIGH` |
| `push-on-tag` | Push images when triggered by tag | No | `true` |

**Required Secrets:**
- `REGISTRY_USERNAME` - Container registry username
- `REGISTRY_PASSWORD` - Container registry password

### `reusable-python-ci.yml`

Runs Python linting and tests with coverage.

**Usage:**
```yaml
jobs:
  test:
    uses: ramaedge/.github/.github/workflows/reusable-python-ci.yml@main
    with:
      test-paths: 'src/'
      coverage-threshold: 80
      editable-paths: 'libs/common'
```

**Inputs:**
| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `python-version` | Python version to use | No | `3.12` |
| `test-path` | Path to test directory (deprecated) | No | - |
| `test-paths` | Space-separated paths to test | No | - |
| `lint-path` | Path to lint (deprecated) | No | - |
| `lint-paths` | Space-separated paths to lint (defaults to tests) | No | - |
| `cov-paths` | Space-separated paths for coverage (defaults to tests) | No | - |
| `coverage-threshold` | Minimum coverage percentage | No | `80` |
| `editable-paths` | Paths to install in editable mode | No | - |
| `pythonpath` | Additional PYTHONPATH to set | No | - |
| `run-lint` | Whether to run linting | No | `true` |
| `run-tests` | Whether to run tests | No | `true` |

## Runner

All workflows use `runs-on: arc-runner-set` by default.

## Versioning

- Use `@main` for development and testing
- Use tags like `@v1` or `@v1.2.0` for production stability

## Contributing

1. Create a branch for your changes
2. Test the actions/workflows in a test repository
3. Create a pull request
4. After merge, create a tag if making a stable release
