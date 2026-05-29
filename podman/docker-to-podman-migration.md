# Podman Troubleshooting on Windows (After Docker)

You have successfully removed the old Podman machine. Follow these steps if `podman machine init` fails.

## A. Full Cleanup of Remaining Files

```bash
# Force remove existing machine
podman machine rm -f podman-machine-default

# Remove Podman configuration folders
rm -rf ~/.config/containers
rm -rf ~/.local/share/containers

# Also run these commands in PowerShell:
Remove-Item -Recurse -Force "$env:USERPROFILE\.config\containers" -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force "$env:USERPROFILE\.local\share\containers" -ErrorAction SilentlyContinue
B. Restart WSL
Bashwsl --shutdown
C. Update WSL
Bashwsl --update
wsl --install --no-distribution

After completing steps A, B, and C, run these commands:
Bashpodman machine init
podman machine start
podman info
