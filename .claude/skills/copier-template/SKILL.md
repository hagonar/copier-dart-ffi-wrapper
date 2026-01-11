# Copier Template Development

## Testing the Template

### Generate C project
```bash
copier copy . /tmp/test_c --trust --defaults \
  --data package_name=test_c \
  --data description="Test" \
  --data native_library_name=lib \
  --data github_repo=u/r \
  --data wrapper_type=c \
  --data native_version=1.0 \
  --data ffi_prefix=X_ \
  --data header_entry_point=h/x.h
```

### Generate Rust project
```bash
copier copy . /tmp/test_rust --trust --defaults \
  --data package_name=test_rust \
  --data description="Test" \
  --data native_library_name=lib \
  --data github_repo=u/r \
  --data wrapper_type=rust \
  --data native_version=1.0 \
  --data ffi_prefix=x_ \
  --data header_entry_point=h/x.h \
  --data cbindgen_config_path=cbindgen.toml \
  --data cbindgen_crate=lib_ffi \
  --data strip_version_prefix=true
```

## Key Files

- `copier.yml` - Template configuration
- `template/{{ package_name }}/` - Template files
- `.jinja` suffix - Files with Jinja2 content

## Configuration

```yaml
# copier.yml structure
_min_copier_version: "9.0.0"
_subdirectory: template
_templates_suffix: .jinja
_jinja_extensions:
  - jinja2_strcase.StrcaseExtension
_exclude:
  - "{% if condition %}path{% endif %}"
_tasks:
  - command: "shell command"
    working_directory: "{{ _copier_conf.dst_path }}"
```

## Conditional File Exclusion

Use `_exclude` with Jinja2 conditions:
```yaml
_exclude:
  - "{% if wrapper_type == 'c' %}{{ package_name }}/.github/actions/setup-rust{% endif %}"
```
