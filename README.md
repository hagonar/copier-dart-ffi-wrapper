# copier-dart-ffi-wrapper

[![GitHub release](https://img.shields.io/github/v/release/djx-y-z/copier-dart-ffi-wrapper)](https://github.com/djx-y-z/copier-dart-ffi-wrapper/releases)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A Copier template for generating Flutter/Dart FFI wrapper packages with full CI/CD support.

## Features

- Cross-platform native library support (Android, iOS, macOS, Linux, Windows)
- Automatic native library download via build hooks
- SHA256 checksum verification for supply chain security
- GitHub Actions workflows for CI/CD
- Support for both C and Rust native libraries

## Requirements

- Python 3.8+
- [Copier](https://copier.readthedocs.io/) 9.0+
- [jinja2-strcase](https://pypi.org/project/jinja2-strcase/) (for case conversion filters)

```bash
pip install copier jinja2-strcase
# or with brew
brew install copier && pip install jinja2-strcase
```

## Usage

### Create a new project

```bash
copier copy https://github.com/djx-y-z/copier-dart-ffi-wrapper my_package
```

### Update an existing project

```bash
cd my_package
copier update
```

## Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `package_name` | Dart package name | `my_ffi_lib` |
| `description` | Package description | `Dart bindings for MyLib` |
| `wrapper_type` | Native library type (`c` or `rust`) | `c` |
| `native_library_name` | Native library name | `mylib` |
| `github_repo` | GitHub repository of your wrapper package | `user/my_ffi_lib` |
| `native_repo` | GitHub repository of native library | `original/mylib` |
| `native_version` | Native library version | `0.15.0` |
| `ffi_prefix` | Function prefix for ffigen | `MY_` |
| `header_entry_point` | Header file path (for ffigen) | `headers/mylib.h` |
| `cmake_headers_path` | Path to headers after CMake build (C only) | `include` |
| `cmake_extra_args` | Extra CMake arguments (C only) | `-DBUILD_SHARED_LIBS=ON` |
| `cbindgen_config_path` | Path to cbindgen.toml (Rust only) | `cbindgen.toml` |
| `cbindgen_crate` | Crate name for cbindgen (Rust only) | `libsignal-ffi` |
| `needs_header_fixes` | Include `_fixHeaders()` function template | `true` |
| `flutter_version` | Flutter version for FVM | `3.38.4` |
| `license` | License type | `MIT` |
| `strip_version_prefix` | Strip 'v' from version tags | `true` |

## Post-Generation Setup

After generating the project, follow these steps:

### Initial Setup

1. **Navigate to your package directory:**
   ```bash
   cd <package_name>
   ```

2. **Add native library license:**
   Copy the license file from the native library to `LICENSE_NATIVE` or include it in the main `LICENSE` file

3. **Install dependencies:**
   ```bash
   make setup
   ```

4. **Generate FFI bindings (downloads headers automatically):**
   ```bash
   make regen
   ```

5. **Implement your Dart API:**
   Create your public API in `lib/src/`

6. **Update example apps:**
   Example apps are created automatically in `example/` and `example_cli/`
   Update them with your library's functionality

7. **Write tests:**
   A test template is provided at `test/<package_name>_test.dart`

8. **Run tests:**
   ```bash
   make test
   ```

### GitHub Actions Configuration

#### 1. GitHub App (for signed commits)

Create a GitHub App for automated signed commits in update PRs:

1. Go to **Settings > Developer settings > GitHub Apps > New GitHub App**
2. Set:
   - **Name**: `<your-package>-bot` (or any unique name)
   - **Homepage URL**: Your repository URL
   - **Permissions**:
     - Contents: Read & Write
     - Pull requests: Read & Write
     - Metadata: Read-only
   - **Webhook**: Uncheck "Active" (not needed)
3. After creation:
   - Note the **App ID** from the app's settings page
   - Generate a **private key** and download it
4. Install the app on your repository (Settings > Install App)
5. Add to your repository:
   - **Secret** `APP_PRIVATE_KEY`: The downloaded private key content
   - **Variable** `APP_ID`: The App ID from step 3

#### 2. Coverage Badge

To enable the coverage badge:

1. Create a **public GitHub Gist** (can be empty, will be updated automatically)
2. Generate a **Personal Access Token** with `gist` scope
3. Add to your repository:
   - **Secret** `GIST_TOKEN`: Your Personal Access Token
   - **Variable** `COVERAGE_GIST_ID`: The Gist ID (from the URL)

Then add the badge to your README:
```markdown
[![Coverage](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/<username>/<gist_id>/raw/coverage.json)](https://github.com/<your-repo>/actions)
```

#### 3. Automated Publishing (optional)

For automated pub.dev publishing on tag push:

1. Go to [pub.dev](https://pub.dev) > Your publisher > Admin
2. Enable **"Automated publishing"**
3. Add your repository to allowed repositories
4. Configure to allow publishing from tags (`v*`)

See: https://dart.dev/tools/pub/automated-publishing

## Generated Project Structure

```
<package_name>/
├── .github/
│   ├── actions/
│   │   ├── setup-fvm/          # FVM setup action
│   │   └── setup-rust/         # Rust setup action (rust only)
│   └── workflows/
│       ├── test.yml            # Test workflow
│       ├── test-reusable.yml   # Reusable test workflow
│       ├── build-*.yml         # Native library build
│       ├── check-*-updates.yml # Update checker
│       └── publish.yml         # pub.dev publishing
├── example/                    # Flutter example app
│   ├── lib/main.dart
│   └── pubspec.yaml
├── example_cli/                # Dart CLI example
│   ├── bin/<package_name>_cli.dart
│   └── pubspec.yaml
├── lib/
│   └── src/
│       └── bindings/           # FFI bindings
├── scripts/
│   ├── build.dart              # Build script
│   ├── check_updates.dart      # Update checker
│   ├── regenerate_bindings.dart # Headers download + ffigen
│   ├── setup_build.dart        # Build deps setup (rust only)
│   └── src/                    # Script utilities
├── hook/
│   └── build.dart              # Build hook for auto-download
├── Makefile                    # Development commands
└── pubspec.yaml
```

## Development Commands

After generation, use `make help` to see all available commands:

```bash
make setup      # Setup development environment
make build      # Build native libraries
make test       # Run tests
make regen      # Regenerate FFI bindings
make check      # Check for native library updates
```

## Key Differences by Wrapper Type

### C (`wrapper_type: c`)
- Uses CMake + Ninja/Make for building
- No Rust toolchain required
- Excludes:
  - `.github/actions/setup-rust/`
  - `scripts/setup_build.dart`

### Rust (`wrapper_type: rust`)
- Uses Cargo for building
- Requires Rust toolchain + protoc
- Has `setup-build` Makefile target
- Windows CI: Git link.exe workaround for MSVC

## License

MIT
