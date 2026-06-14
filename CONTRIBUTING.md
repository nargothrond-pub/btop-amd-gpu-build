# Contributing to btop AMD GPU Build

First off — thank you for taking the time to contribute! 🎉

This project's primary asset is the **GitHub Actions workflow** that builds btop with AMD GPU support. Contributions that improve the build, expand platform support, fix CI issues, or improve documentation are all very welcome.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How can I contribute?](#how-can-i-contribute)
  - [Reporting bugs](#reporting-bugs)
  - [Suggesting enhancements](#suggesting-enhancements)
  - [Submitting a pull request](#submitting-a-pull-request)
- [Development setup](#development-setup)
- [Style guide](#style-guide)
- [Commit messages](#commit-messages)

---

## Code of Conduct

This project adheres to our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold it.

---

## How can I contribute?

### Reporting bugs

Use the **[Bug Report](.github/ISSUE_TEMPLATE/bug_report.md)** issue template. Please include:

- The OS and ROCm version on your target machine
- The exact `btop` version the artifact was built from
- Steps to reproduce the problem
- What you expected vs. what actually happened
- Relevant logs or error messages

### Suggesting enhancements

Use the **[Feature Request](.github/ISSUE_TEMPLATE/feature_request.md)** template. Describe the use case and the problem it solves. We especially welcome:

- Support for additional CPU architectures (`aarch64`, `riscv64`)
- Support for packaging formats (`.deb`, `.rpm`, Flatpak, AppImage)
- Improvements to the ROCm version matrix
- Better artifact retention / release automation

### Submitting a pull request

1. **Fork** the repo and create a branch from `main`:
   ```bash
   git checkout -b fix/my-improvement
   ```
2. **Make your changes** — keep commits focused and atomic.
3. **Test locally** if possible (see [Development setup](#development-setup)).
4. **Ensure** the workflow YAML is valid (`actionlint` is a great tool).
5. **Push** and open a PR against `main` using the pull request template.

---

## Development setup

The only real "code" here is the GitHub Actions workflow. To validate it locally:

```bash
# Install actionlint (GitHub Actions linter)
# https://github.com/rhysd/actionlint
brew install actionlint          # macOS
# or
go install github.com/rhysd/actionlint/cmd/actionlint@latest

# Lint the workflow
actionlint .github/workflows/build-amd-gpu.yaml
```

To test a build end-to-end, trigger a [manual workflow run](../../actions/workflows/build-amd-gpu.yaml) against a fork of this repo with your branch.

---

## Style guide

- Workflow steps should follow the existing pattern: **numbered sections with clear comment headers**.
- Prefer `apt-get` over `apt` in CI scripts (more stable for scripting).
- All new shell snippets should be POSIX-compatible where possible.
- Document the *why* in comments, not just the *what*.

---

## Commit messages

Follow the **[Conventional Commits](https://www.conventionalcommits.org/)** spec:

```
<type>(<scope>): <short description>

[optional body]

[optional footer(s)]
```

Types: `feat`, `fix`, `ci`, `docs`, `refactor`, `chore`, `test`

Examples:
```
ci(build): add aarch64 matrix leg
fix(rocm): pin ROCm apt key to latest stable
docs(readme): add GPU panel screenshot
```
