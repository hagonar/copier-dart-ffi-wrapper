# Jinja2 Syntax for Copier Templates

## Basic Syntax

```jinja2
{{ variable }}                  # Variable output
{% if condition %}...{% endif %} # Conditional
{% for item in list %}...{% endfor %} # Loop
```

## Case Conversion (jinja2-strcase)

```jinja2
{{ "my_package" | to_camel }}           # MyPackage (PascalCase)
{{ "my_package" | to_lower_camel }}     # myPackage (camelCase)
{{ "my_package" | to_snake }}           # my_package (snake_case)
{{ "my_package" | to_screaming_snake }} # MY_PACKAGE (CONSTANT_CASE)
{{ "my_package" | to_kebab }}           # my-package (kebab-case)
```

## Conditionals

```jinja2
{% if wrapper_type == 'rust' %}
Rust-specific content
{% endif %}

{% if wrapper_type == 'c' %}
C-specific content
{% endif %}

{% if not strip_version_prefix %}
Include 'v' prefix
{% endif %}
```

## Loops

```jinja2
{% for topic in topics.split(',') | map('trim') %}
  - {{ topic }}
{% endfor %}
```

## Built-in Filters

```jinja2
{{ variable | upper }}         # UPPERCASE
{{ variable | lower }}         # lowercase
{{ variable | title }}         # Title Case
{{ variable | trim }}          # Remove whitespace
{{ variable | default('x') }}  # Default value
```

## Whitespace Control

```jinja2
{%- if condition -%}   # Remove surrounding whitespace
{{ variable -}}        # Remove trailing whitespace
{%- variable }}        # Remove leading whitespace
```

## File Naming

- `{{ package_name }}` in directory names - Dynamic directory names
- `filename.jinja` - Processed and .jinja suffix removed
- `filename` without .jinja - Copied as-is

## Examples from Template

**Makefile.jinja:**
```jinja2
{% if wrapper_type == 'rust' %}
.PHONY: setup setup-fvm setup-build
{% endif %}
{% if wrapper_type == 'c' %}
.PHONY: setup build
{% endif %}
```

**pubspec.yaml.jinja:**
```jinja2
ffigen:
  name: {{ package_name | to_camel }}Bindings
```

**LICENSE.jinja:**
```jinja2
{% if license == 'MIT' %}
MIT License
Copyright (c) {{ current_year }} {{ package_name }} authors
{% endif %}
```
