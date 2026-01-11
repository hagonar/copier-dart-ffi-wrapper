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
- Variable validators with regex for `package_name`, `github_repo`, and `native_repo`
- Variables `native_repo` for native library repository and `needs_header_fixes` for header fix function
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
- Badges in generated README.md (pub.dev, CI, License, Dart, Flutter, native library version)
- Badges in template README.md (GitHub release, License)
- Created `lib/src/` directory with `.gitkeep` in generated projects
- CONTRIBUTING.md with development guidelines and testing instructions
- Example commands with all parameters in README.md
- Full absolute path in post-generation message
- Information about updating topics and SECURITY.md in post-generation message
- `_fixHeaders()` function available for both C and Rust wrappers (controlled by `needs_header_fixes`)

[Unreleased]: https://github.com/djx-y-z/copier-dart-ffi-wrapper/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/djx-y-z/copier-dart-ffi-wrapper/releases/tag/v1.0.0
