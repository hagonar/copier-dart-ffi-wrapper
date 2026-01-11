# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly:

1. **Do NOT** open a public GitHub issue
2. Email the maintainers directly or use GitHub's private vulnerability reporting
3. Include as much detail as possible:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

## Security Measures

This package implements several security measures:

### Supply Chain Security

- **SHA256 Checksum Verification**: All downloaded native libraries are verified against checksums from GitHub Releases
- **HTTPS Only**: All downloads use HTTPS
- **Pinned Versions**: Native library versions are pinned in `pubspec.yaml`

### Memory Safety

- **Secure Memory Handling**: Sensitive data is zeroed before deallocation
- **Finalizers**: Automatic cleanup of native resources
- **Validation**: Input validation before FFI calls

### Build Security

- **Reproducible Builds**: Native libraries are built in CI with pinned dependencies
- **No Code Execution**: Build hooks only download and verify pre-built binaries

## Security Updates

Security updates will be released as patch versions. Subscribe to releases to be notified.
