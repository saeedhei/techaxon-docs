GitLab Self-Hosted to GitHub Repository Mirroring Setup
Prerequisites

Before starting, make sure:

You have Maintainer/Owner permission on the GitLab project.
You have access to the GitHub repository.
Your GitLab server can reach GitHub over HTTPS.
1. Create a Repository on GitHub

Create an empty repository on GitHub.

Example:

https://github.com/saeedhei/techaxon

Do not initialize it with:

README
.gitignore
License

The repository should be empty.

2. Create a GitHub Personal Access Token

Go to:

GitHub → Settings → Developer settings → Personal access tokens

Create a Fine-grained Personal Access Token.

Configure it as follows:

Repository access

Choose:

Only select repositories

Select your target repository:

techaxon
Repository permissions

Enable only:

Contents → Read and write

All other permissions can remain:

No access

Generate the token and copy it.

3. Configure Repository Mirroring in GitLab

Open your GitLab project:

Project
  → Settings
    → Repository
      → Mirroring repositories

Click:

Add new mirror
4. Configure the Mirror URL

For the Git repository URL, use the HTTPS Git URL:

Correct:

https://github.com/saeedhei/techaxon.git

Not:

https://github.com/saeedhei/techaxon
5. Configure Authentication

Authentication method:

Password

Enter:

Username:

saeedhei

Password:

YOUR_GITHUB_PERSONAL_ACCESS_TOKEN
6. Mirror Direction

Select:

Push

This means:

GitLab  ─────────►  GitHub

Every push to GitLab will be mirrored to GitHub.

7. Mirror Options
Keep divergent refs

Recommended:

☑ Enable

Purpose:

Prevents GitLab from force pushing over branches/tags that have diverged on GitHub.
Protects changes that exist only on GitHub.
Mirror only protected branches

Recommended:

☐ Disable

Purpose:

If enabled, only protected branches are mirrored.
Feature branches and non-protected branches will not be synced.
