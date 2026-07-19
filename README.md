# Techaxon Documentation Hub

Welcome to the centralized documentation repository for the Techaxon engineering team. This repository serves as our single source of truth for technical standards, workflows, infrastructure setups, and cultural guidelines.

---

## 🚀 Team Guidelines & Core Rules

Before committing code or starting architectural tasks, please review our core operational frameworks:

- **[Code of Conduct](/guidelines/code-of-conduct.md)**  
  _Our cultural foundations. Covers professional expectations, mutual respect, and engineering team values._
- **[Git & Workflow Rules](/guidelines/git-workflow.md)**  
  _Our technical boundaries. Covers branching strategies, Merge Request (MR) procedures, and commit naming conventions._
- **[Engineering Best Practices](/guidelines/engineering-best-practices.md)**  
  _Our technical standards. Covers architectural design patterns, database constraints (including CouchDB document ID requirements), and code quality guidelines._

---

## 📁 Repository Directory Map

For infrastructure setups, deployment scripts, and framework guidelines, navigate through the specific domains below:

### 🔹 [CouchDB](/couchdb/)

- Database operations including **[CLI execution](/couchdb/run-with-cli.md)**, **[Backups](/couchdb/backup.md)**, and **[Restoration routines](/couchdb/restore)**.

### 🔹 [Next (NestJS Backend)](/next/)

- Backend development guides covering **[NestJS Business Logic Best Practices](/next/NestJS_Business_Logic_Best_Practice.md)** and framework **[Installation](/next/install.md)**.

### 🔹 [DevOps & Infrastructure Frameworks]

- **[AWS](/aws/)**: Cloud architectures and the **[Amazon Cognito Workshop](/aws/Amazon-Cognito-Workshop.md)**.
- **[GitLab](/gitlab/)**: Self-hosted setups, runner configurations, pipelines, and **[Repository Mirroring](/gitlab/GitLab%20Self-Hosted%20to%20GitHub%20Repository%20Mirroring%20Setup.md)**.
- **[Traefik](/traefik/)**: Reverse proxy profiles across production-local and development **docker-compose** configurations.

### 🔹 [Containers](/podman/)

- Container runtime environments and **[Docker to Podman Migration steps](/podman/docker-to-podman-migration.md)**.

---

## ✍️ Contributing to Docs

Documentation is a living entity. If you update a workflow, configure a new pipeline, or notice outdated steps:

1. Create a new branch off `main`.
2. Apply changes following standard markdown readability rules.
3. Open a Merge Request for review.
