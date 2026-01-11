# copier-dart-ffi-wrapper - Claude Code Configuration

## Overview

This is a Copier template for generating Dart/Flutter FFI wrapper packages with full CI/CD support for C and Rust native libraries.

## Project Structure

```
copier-dart-ffi-wrapper/
├── copier.yml              # Template configuration and variables
├── README.md               # Template documentation
├── CHANGELOG.md            # Version history
├── LICENSE                 # MIT license
└── template/
    └── {{ package_name }}/ # Template directory (Jinja2 syntax)
        ├── lib/            # Dart library code
        ├── scripts/        # Build and utility scripts
        ├── .github/        # GitHub Actions workflows
        ├── .claude/        # Claude Code skills for generated project
        └── ...
```

## Template Variables

| Variable | Type | Description |
|----------|------|-------------|
| `package_name` | string | Dart package name (lowercase) |
| `description` | string | Package description |
| `wrapper_type` | enum (c/rust) | Native library type |
| `native_library_name` | string | Name of native library |
| `github_repo` | string | GitHub repository path |
| `native_version` | string | Native library version |
| `ffi_prefix` | string | Function prefix for ffigen |
| `header_entry_point` | string | Header file path |
| `license` | enum | Package license |
| `strip_version_prefix` | boolean | Strip 'v' from version tags |

## Template Syntax

This template uses Jinja2 with [jinja2-strcase](https://pypi.org/project/jinja2-strcase/) extension:

```jinja2
{{ package_name }}                    # Variable substitution
{{ package_name | to_camel }}         # PascalCase: MyPackage
{{ package_name | to_lower_camel }}   # camelCase: myPackage
{{ package_name | to_snake }}         # snake_case: my_package
{{ package_name | to_screaming_snake }} # CONSTANT_CASE: MY_PACKAGE
{% if wrapper_type == 'rust' %}...{% endif %}  # Conditional for Rust
{% if wrapper_type == 'c' %}...{% endif %}     # Conditional for C
{% if not strip_version_prefix %}...{% endif %} # Inverted conditional
```

## Testing the Template

### Generate C project
```bash
copier copy . /tmp/test_c \
  --trust \
  --defaults \
  --data package_name=test_c \
  --data description="Test C wrapper" \
  --data native_library_name=testlib \
  --data github_repo=user/test_c \
  --data wrapper_type=c \
  --data native_version=1.0.0 \
  --data ffi_prefix=TEST_ \
  --data header_entry_point=headers/test.h
```

### Generate Rust project
```bash
copier copy . /tmp/test_rust \
  --trust \
  --defaults \
  --data package_name=test_rust \
  --data description="Test Rust wrapper" \
  --data native_library_name=testlib \
  --data github_repo=user/test_rust \
  --data wrapper_type=rust \
  --data native_version=v1.0.0 \
  --data ffi_prefix=test_ \
  --data header_entry_point=headers/test.h \
  --data cbindgen_config_path=cbindgen.toml \
  --data cbindgen_crate=testlib_ffi \
  --data strip_version_prefix=true
```

## Key Differences by Wrapper Type

### C (`wrapper_type: c`)
- Uses CMake + Ninja/Make for building
- No Rust toolchain required
- Files excluded via `_exclude` in copier.yml:
  - `.github/actions/setup-rust/`
  - `scripts/setup_build.dart`

### Rust (`wrapper_type: rust`)
- Uses Cargo for building
- Requires Rust toolchain + protoc
- Has `setup-build` Makefile target
- Windows CI: Git link.exe workaround for MSVC

## Files with Conditional Content

These files have `{% if wrapper_type == 'X' %}...{% endif %}` blocks:

- `Makefile.jinja` - setup targets differ
- `.github/workflows/build-{{ package_name }}.yml.jinja` - build commands
- `scripts/src/common.dart.jinja` - build utilities
- `LICENSE.jinja` - license selection

## Copier Configuration

Key settings in `copier.yml`:

- `_min_copier_version: "9.0.0"` - Minimum Copier version
- `_subdirectory: template` - Template files location
- `_templates_suffix: .jinja` - Only process `.jinja` files
- `_jinja_extensions` - Case conversion filters
- `_exclude` - Conditional file exclusion
- `_tasks` - Post-generation commands (create example apps)

## Development

### Requirements

```bash
pip install copier jinja2-strcase
# or
brew install copier && pip install jinja2-strcase
```

### Testing Changes

```bash
# Quick test
copier copy . /tmp/test --trust --defaults --data package_name=test --data description=Test --data native_library_name=lib --data github_repo=u/r --data wrapper_type=c --data native_version=1.0 --data ffi_prefix=X_ --data header_entry_point=h/x.h

# Check generated files
ls -la /tmp/test/test/
```

### Updating the Template

1. Edit files in `template/{{ package_name }}/`
2. Use `.jinja` suffix for files with Jinja2 content
3. Test both C and Rust variants
4. Update `copier.yml` if adding new variables
