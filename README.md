# Common Configuration

A collection of reusable configuration files for consistent development workflows across projects.

## Overview

This repository contains common configuration files that can be shared across multiple projects using Git submodules. Instead of duplicating configuration in every project, you can include this as a submodule and override only what's necessary for your specific project.

## Contents

- **`justfile-go`** - Just command runner configuration for Go projects with common build, test, lint, and format commands
- **`justfile-nix`** - Just command runner configuration for Nix flake projects with formatting, linting, and deployment commands

## Usage

### Adding as a Git Submodule

1. Add this repository as a submodule in your project:
```bash
git submodule add https://github.com/fred-drake/common-configuration.git common-config
```

2. Initialize and update the submodule:
```bash
git submodule update --init --recursive
```

### Go Projects with Justfile

For Go projects, you can use the `justfile-go` configuration by setting required variables and importing it.

#### Example Setup

Create a `justfile` in your project root:

```justfile
# Set project-specific variables
APP_NAME := "my-awesome-app"
APP_MAIN_GO_FILE := "cmd/server/main.go"
APP_BUILD_PATH := "./cmd/server"

# Import common Go commands
import "common-config/justfile-go"

# Add project-specific commands here
deploy:
    @echo "Deploying {{APP_NAME}}..."
    # Your deployment commands

setup:
    @echo "Setting up development environment..."
    go mod download
    # Any other setup commands
```

#### Required Variables

The `justfile-go` configuration expects these variables to be defined:

- `APP_NAME` - The name of your application binary
- `APP_MAIN_GO_FILE` - Path to your main Go file for the `run` command (e.g., `main.go`, `./cmd/server/main.go`)
- `APP_BUILD_PATH` - Path to the directory containing your main Go package for building (e.g., `.` for root, `./cmd/server` for subdirectories)

**Common Project Structures:**

```bash
# Simple project with main.go in root
APP_MAIN_GO_FILE := "main.go"
APP_BUILD_PATH := "."

# Project with main.go in cmd/app directory
APP_MAIN_GO_FILE := "./cmd/app/main.go"
APP_BUILD_PATH := "./cmd/app"

# Multiple binaries - define for each
APP_MAIN_GO_FILE := "./cmd/server/main.go"
APP_BUILD_PATH := "./cmd/server"
```

#### Available Commands

Once imported, you'll have access to these commands:

- `just check-tools` - Verify required development tools are installed
- `just run` - Build and run the application
- `just build` - Build the application binary
- `just build-prod` - Build production binary with optimizations
- `just test` - Run tests with coverage
- `just test-coverage` - Generate HTML coverage report
- `just format` - Format Go code using goimports-reviser
- `just format-check` - Check if code formatting is correct
- `just lint` - Run golangci-lint checks
- `just lint-fix` - Fix common linting issues and format code
- `just vulncheck` - Check for security vulnerabilities
- `just check` - Run all checks (format-check, test, lint, vulncheck)
- `just clean` - Clean build artifacts and cache

### Nix Projects with Justfile

For Nix flake projects, you can use the `justfile-nix` configuration by importing it.

#### Example Setup

Create a `justfile` in your project root:

```justfile
# Import common Nix commands
import "common-config/justfile-nix"

# Add project-specific commands here
build:
    nix build .#

dev:
    nix develop
```

#### Available Commands

Once imported, you'll have access to these commands:

- `just format` - Format all .nix files using alejandra
- `just lint` - Run statix linting checks
- `just update` - Update input definitions from remote resources
- `just update-secrets` - Update the secrets flake
- `just colmena HOST` - Run colmena remote switch on given host (for NixOS deployment)

## Updating the Submodule

To pull in updates from this repository:

```bash
git submodule update --remote common-config
git add common-config
git commit -m "Update common configuration"
```

