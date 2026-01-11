# Copier Variables

## All Template Variables

| Variable | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `package_name` | str | Yes | - | Dart package name (lowercase) |
| `description` | str | Yes | - | Package description |
| `native_library_name` | str | Yes | - | Native library name |
| `github_repo` | str | Yes | - | GitHub repository (user/repo) |
| `wrapper_type` | str | No | `c` | `c` or `rust` |
| `native_version` | str | Yes | - | Native library version |
| `ffi_prefix` | str | Yes | - | FFI function prefix |
| `header_entry_point` | str | Yes | - | Header file path |
| `lib_linux` | str | No | `lib{{ native_library_name }}.so` | Linux library filename |
| `lib_macos` | str | No | `lib{{ native_library_name }}.dylib` | macOS library filename |
| `lib_windows` | str | No | `{{ native_library_name }}.dll` | Windows library filename |
| `lib_android` | str | No | `lib{{ native_library_name }}.so` | Android library filename |
| `lib_ios` | str | No | `lib{{ native_library_name }}.dylib` | iOS library filename |
| `cmake_headers_path` | str | No | `include` | CMake headers path (C only) |
| `cmake_extra_args` | str | No | `""` | Extra CMake args (C only) |
| `cbindgen_config_path` | str | No | `cbindgen.toml` | cbindgen config (Rust only) |
| `cbindgen_crate` | str | No | `""` | FFI crate name (Rust only) |
| `flutter_version` | str | No | `3.38.4` | Flutter version for FVM |
| `dart_sdk_version` | str | No | `^3.10.0` | Dart SDK constraint |
| `flutter_sdk_version` | str | No | `>=3.38.0` | Flutter SDK constraint |
| `android_min_sdk` | str | No | `21` | Android min SDK |
| `android_compile_sdk` | str | No | `34` | Android compile SDK |
| `license` | str | No | `MIT` | Package license |
| `topics` | str | No | `ffi,native` | Pub.dev topics (comma-separated) |
| `strip_version_prefix` | bool | No | `false` | Strip 'v' from version |
| `current_year` | str | No | Auto | Current year (computed) |

## Validators

```yaml
package_name:
  validator: >-
    {% if not (package_name | regex_search('^[a-z][a-z0-9_]*$')) %}
    Must be lowercase, start with a letter, contain only letters, numbers, underscores.
    {% endif %}

github_repo:
  validator: >-
    {% if not (github_repo | regex_search('^[a-zA-Z0-9_-]+/[a-zA-Z0-9_.-]+$')) %}
    Must be in format "username/repo_name".
    {% endif %}
```

## Conditional Variables

Variables shown only for specific wrapper types:
```yaml
cmake_headers_path:
  when: "{{ wrapper_type == 'c' }}"

cbindgen_config_path:
  when: "{{ wrapper_type == 'rust' }}"
```
