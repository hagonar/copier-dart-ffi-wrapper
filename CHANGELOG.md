# Changelog

All notable changes to this template will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2025-01-11

### Added

- Initial release as Copier template
- Support for C and Rust native library wrappers
- Jinja2 templating with jinja2-strcase for case conversions
- Variable validators with regex for `package_name` and `github_repo`
- Conditional file exclusion via `_exclude` in copier.yml
- `copier update` support for updating existing projects
- Cross-platform build scripts (macOS, iOS, Android, Linux, Windows)
- GitHub Actions workflows:
  - Multi-platform testing (Linux x64/ARM64, macOS ARM64, Windows x64)
  - Native library building with GitHub App signed commits
  - Automated update checking
  - pub.dev publishing with OIDC
- Build hooks for automatic native library download
- SHA256 checksum verification for supply chain security
- FVM (Flutter Version Manager) integration with Windows Git Bash support
- Claude Code skills for development assistance
- Comprehensive documentation templates (README, CONTRIBUTING, SECURITY, CHANGELOG)
- Automatic header downloading and generation:
  - C libraries: CMake build to generate headers
  - Rust libraries: cbindgen to generate C headers
- Automatic example app creation during generation

[Unreleased]: https://github.com/djx-y-z/copier-dart-ffi-wrapper/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/djx-y-z/copier-dart-ffi-wrapper/releases/tag/v1.0.0
