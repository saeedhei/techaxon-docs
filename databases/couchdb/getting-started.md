# Quick Guide: Running CouchDB with Docker

This guide explains how to quickly run **Apache CouchDB** using Docker and access its web management interface.

---

> [!NOTE]
> The `docker run` and Docker Compose instructions below are two alternative methods. Choose one of them. Do not run both at the same time unless you change the container name and host port.

> [!WARNING]
> The username and password used in this guide are examples for local development only. Use secure credentials for a real environment.

---

## 1. Prerequisites

Make sure Docker and Docker Compose are installed:

```bash
docker --version
docker compose version
```

You can also check whether another CouchDB container or another service is already using port `5984`:

```bash
docker ps
```

## 2. Running CouchDB with Docker

Run CouchDB with the following command:

```bash
docker run -d --name couchdb -p 127.0.0.1:5984:5984 -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=mysecretpassword -v couchdb_data:/opt/couchdb/data couchdb:3
```

The command is written on one line so that it works in Windows Command Prompt, PowerShell, Linux and macOS terminals.

It creates:

- a container named `couchdb`
- an administrator named `admin`
- a named volume called `couchdb_data`
- a local connection on port `5984`

The named volume keeps the CouchDB data when the container is removed.

## 3. Running CouchDB with Docker Compose

Alternatively, create a file named `docker-compose.yml`:

```yaml
services:
  couchdb:
    image: couchdb:3
    container_name: couchdb
    ports:
      - "127.0.0.1:5984:5984"
    environment:
      COUCHDB_USER: admin
      COUCHDB_PASSWORD: mysecretpassword
    volumes:
      - couchdb_data:/opt/couchdb/data
    restart: unless-stopped

volumes:
  couchdb_data:
```

Validate the Compose file:

```bash
docker compose config
```

Start CouchDB in the background:

```bash
docker compose up -d
```

Check whether the container is running:

```bash
docker compose ps
```

## 4. Accessing the Web Dashboard (Fauxton)

The Web UI / Admin dashboard for CouchDB is called **Fauxton**.

### Access URL:
After starting the container, open your browser and navigate to:

```text
http://localhost:5984/_utils/
```

### Credentials:
* Username: `admin` (or the value set in `COUCHDB_USER`)
* Password: `mysecretpassword` (or the value set in `COUCHDB_PASSWORD`)

---

## 5. Completing the Single-Node Setup

Starting the container and creating an administrator does not automatically create all CouchDB system databases.

For a local single-node installation, open:

```text
http://localhost:5984/_utils/#setup
```

Choose the single-node setup and follow the instructions in Fauxton.

This creates the required system databases such as `_users`, `_replicator` and `_global_changes`.


## 6. Service Health Check

On Linux, macOS or PowerShell, run:

```bash
curl -u admin:mysecretpassword http://localhost:5984/
```

When using Windows Command Prompt, use:

```cmd
curl.exe -u admin:mysecretpassword http://localhost:5984/
```

A successful response contains values similar to:

```json
{
  "couchdb": "Welcome",
  "version": "<installed version>",
  "vendor": {
    "name": "The Apache Software Foundation"
  }
}
```

The exact version number and additional response fields depend on the installed CouchDB version.

## 7. Port or Container Name Conflicts

If another container already uses the name `couchdb`, either remove it or choose a different name.

If port `5984` is already in use, change only the host port. For example:

```bash
docker run -d --name couchdb-test -p 127.0.0.1:5985:5984 -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=mysecretpassword -v couchdb_test_data:/opt/couchdb/data couchdb:3
```

CouchDB will then be available at:

```text
http://localhost:5985/_utils/
```

## 8. Stopping and Removing CouchDB

### Docker container

Stop and remove the container:

```bash
docker stop couchdb
docker rm couchdb
```

The data remains stored in the named volume.

To permanently delete the stored data as well:

```bash
docker volume rm couchdb_data
```

### Docker Compose

Stop and remove the container and network:

```bash
docker compose down
```

The named volume and CouchDB data are preserved.

To remove the container, network and stored data:

```bash
docker compose down -v
```

> [!WARNING]
> Using `docker compose down -v` permanently deletes the CouchDB data stored in the Compose volume.