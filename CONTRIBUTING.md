# Contributing to copier-dart-ffi-wrapper

Thank you for your interest in contributing to this Copier template!

## Getting Started

### Prerequisites

- Python 3.8+
- [Copier](https://copier.readthedocs.io/) 9.0+
- [jinja2-strcase](https://pypi.org/project/jinja2-strcase/)

```bash
pip install copier jinja2-strcase
# or
brew install copier && pip install jinja2-strcase
```

### Setup

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/copier-dart-ffi-wrapper.git
   cd copier-dart-ffi-wrapper
   ```

## Development Workflow

### Template Structure

```
copier-dart-ffi-wrapper/
├── copier.yml                    # Template configuration and variables
├── template/
│   └── {{ package_name }}/       # Template files (Jinja2 syntax)
│       ├── lib/                  # Dart library code
│       ├── scripts/              # Build and utility scripts
│       ├── .github/              # GitHub Actions workflows
│       └── ...
├── README.md                     # Template documentation
├── CHANGELOG.md                  # Version history
└── LICENSE                       # MIT license
```

### Making Changes

1. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature
   ```

2. Edit template files in `template/{{ package_name }}/`
   - Use `.jinja` suffix for files with Jinja2 content
   - Use `{{ variable_name }}` for variable substitution
   - Use `{% if condition %}...{% endif %}` for conditionals

3. If adding new variables, update `copier.yml`:
   ```yaml
   my_variable:
     type: str
     help: "Description of the variable"
     default: "default_value"
   ```

### Testing Changes

Always test both C and Rust variants after making changes:

```bash
# Test C wrapper
rm -rf /tmp/test_c && copier copy . /tmp/test_c \
  --trust --vcs-ref HEAD \
  --data package_name=test_c \
  --data description="Test C wrapper" \
  --data native_library_name=testlib \
  --data github_repo=user/test_c \
  --data native_repo=original/testlib \
  --data wrapper_type=c \
  --data native_version=1.0.0 \
  --data ffi_prefix=TEST_ \
  --data header_entry_point=headers/test.h

# Check generated files
ls -la /tmp/test_c/test_c/

# Test Rust wrapper
rm -rf /tmp/test_rust && copier copy . /tmp/test_rust \
  --trust --vcs-ref HEAD \
  --data package_name=test_rust \
  --data description="Test Rust wrapper" \
  --data native_library_name=testlib \
  --data github_repo=user/test_rust \
  --data native_repo=original/testlib \
  --data wrapper_type=rust \
  --data native_version=v1.0.0 \
  --data ffi_prefix=test_ \
  --data header_entry_point=headers/test.h \
  --data cbindgen_crate=testlib_ffi \
  --data strip_version_prefix=true

# Check generated files
ls -la /tmp/test_rust/test_rust/
```

### Jinja2 Syntax Reference

This template uses Jinja2 with [jinja2-strcase](https://pypi.org/project/jinja2-strcase/) extension:

```jinja2
{{ package_name }}                      # Variable substitution
{{ package_name | to_camel }}           # PascalCase: MyPackage
{{ package_name | to_lower_camel }}     # camelCase: myPackage
{{ package_name | to_snake }}           # snake_case: my_package
{{ package_name | to_screaming_snake }} # CONSTANT_CASE: MY_PACKAGE

{% if wrapper_type == 'rust' %}...{% endif %}  # Conditional for Rust
{% if wrapper_type == 'c' %}...{% endif %}     # Conditional for C
```

## Pull Request Process

1. Ensure your changes work for both C and Rust wrapper types
2. Update documentation if needed:
   - `README.md` - if adding new variables or features
   - `CLAUDE.md` - if adding new variables or changing template structure
   - `CHANGELOG.md` - add your changes under `[Unreleased]`
3. Follow the commit message format (see below)
4. Submit a pull request

## Commit Messages

Use clear, descriptive commit messages following [Conventional Commits](https://www.conventionalcommits.org/):

- `feat: Add new feature` - new functionality
- `fix: Fix bug in XYZ` - bug fixes
- `docs: Update README` - documentation only
- `refactor: Improve code structure` - code changes that neither fix bugs nor add features
- `chore: Update dependencies` - maintenance tasks

Examples:
```
feat: add support for custom CMake arguments
fix: correct header path in Rust wrapper
docs: add examples for all variables
```

## Code Style

### Template Files

- Use 2 spaces for indentation in YAML, Dart, and shell scripts
- Use meaningful variable names
- Add comments for complex Jinja2 logic
- Keep conditionals simple and readable

### copier.yml

- Group related variables with comments
- Provide helpful descriptions in `help` field
- Add validators for required fields
- Use sensible defaults where possible

## Reporting Issues

When reporting issues, please include:

1. Copier version (`copier --version`)
2. Python version (`python --version`)
3. Operating system
4. Steps to reproduce
5. Expected vs actual behavior
6. Relevant error messages

## Questions?

Feel free to open an issue for questions or discussions.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
