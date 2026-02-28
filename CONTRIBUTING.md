# Contributing to Red Hat OpenShift Dev Spaces Images

Thank you for your interest in contributing to Red Hat OpenShift Dev Spaces!

## Overview

This repository contains the midstream code for Red Hat OpenShift Dev Spaces (formerly CodeReady Workspaces) images. The code here is used to build images in Brew/OSBS.

## Getting Started

### Prerequisites

- Git
- Docker or Podman
- Basic understanding of containers and Kubernetes

### Repository Structure

This repository contains multiple image directories:

- `devspaces-code/` - VS Code based IDE
- `devspaces-dashboard/` - Dashboard component
- `devspaces-operator/` - Kubernetes operator
- `devspaces-pluginregistry/` - Plugin registry
- `devspaces-server/` - Server component
- And many more...

Each directory typically contains:
- `Dockerfile` - Container build instructions
- `build/` - Build scripts and configurations
- `container.yaml` - Container metadata
- `README.md` or documentation

## CI/CD Workflows

This repository uses GitHub Actions for continuous integration. The following workflows are configured:

### CI Workflow (`.github/workflows/ci.yml`)

Runs on all pull requests and pushes to main branches. Validates:
- Repository structure
- VERSION.json format
- Presence of key files

### Docker Build Check (`.github/workflows/docker-build.yml`)

Runs when Dockerfiles are modified. Performs:
- Dockerfile syntax validation using hadolint
- Container configuration checks

### Code Quality (`.github/workflows/code-quality.yml`)

Validates code quality for:
- Shell scripts (using ShellCheck)
- YAML files (using yamllint)
- JSON files (using Python json.tool)

### Welcome Workflow (`.github/workflows/welcome.yml`)

Automatically welcomes new contributors when they open issues or pull requests.

## Making Contributions

### Pull Request Process

1. **Fork and Clone**: Fork this repository and clone it locally
2. **Create a Branch**: Create a feature branch for your changes
3. **Make Changes**: Make your changes, keeping them focused and minimal
4. **Test Locally**: Test your changes locally before submitting
5. **Run Linters**: Ensure code quality checks pass:
   ```bash
   # Check shell scripts
   shellcheck script.sh

   # Check YAML files
   yamllint -d relaxed file.yaml

   # Validate JSON
   python3 -m json.tool file.json
   ```
6. **Submit PR**: Create a pull request with a clear description
7. **CI Checks**: Ensure all CI checks pass
8. **Code Review**: Address any feedback from reviewers

### Commit Messages

- Use clear, descriptive commit messages
- Reference issue numbers when applicable
- Keep commits focused on a single change

### Code Style

- Follow existing code patterns in each directory
- Keep changes minimal and focused
- Document complex changes with comments when necessary

## Development Workflow

### Building Images

Each image directory may have its own build process. Typically:

```bash
cd devspaces-<component>
# Check for build.sh or Makefile
./build.sh
# or
make build
```

### Testing Changes

Before submitting a PR:

1. Build the affected images locally
2. Test the functionality
3. Run available linters and validators
4. Check that CI workflows would pass

## Reporting Issues

For issues, please see [.github/ISSUE_TEMPLATE/where-to-report-issues.md](.github/ISSUE_TEMPLATE/where-to-report-issues.md).

## Code Owners

See [.github/CODEOWNERS](.github/CODEOWNERS) for the list of code owners and reviewers.

## Additional Resources

- [README.md](README.md) - Repository overview
- [Red Hat OpenShift Dev Spaces Documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_dev_spaces/)
- Upstream Eclipse Che: https://github.com/eclipse/che

## License

See [LICENSE](LICENSE) for license information.

---

Thank you for contributing to Red Hat OpenShift Dev Spaces! 🎉
