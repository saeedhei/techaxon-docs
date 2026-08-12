# Production Environment Rules

To keep production environments secure, stable, and predictable:

- **No package installation on production servers**
  - Dependencies should be installed during the build process, not directly on production.

- **No application builds on production servers**
  - Production servers should only run the already-built application artifacts.

- **No running applications as root**
  - Services should use dedicated users with only the required permissions.

- **No secrets in the repository**
  - API keys, passwords, tokens, and environment-specific secrets must be stored securely outside the codebase.

These rules help keep deployments safer, easier to reproduce, and easier to maintain across all Techaxon projects.
