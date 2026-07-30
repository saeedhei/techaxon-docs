# Quick Guide: Running CouchDB with Docker

This guide explains how to quickly run **Apache CouchDB** using Docker and access its web management interface.

---

## 1. Running CouchDB with Docker

To run a CouchDB container with a specified username and password, execute the following command in your terminal:

```
  docker run -d \
    --name couchdb \
    -p 5984:5984 \
    -e COUCHDB_USER=admin \
    -e COUCHDB_PASSWORD=mysecretpassword \
    couchdb:latest
```

### Running CouchDB with Docker Compose

If you prefer using a `docker-compose.yml` file:


```
  services:
    couchdb:
      image: couchdb:latest
      container_name: couchdb
      ports:
        - "5984:5984"
      environment:
        - COUCHDB_USER=admin
        - COUCHDB_PASSWORD=mysecretpassword
      volumes:
        - couchdb_data:/opt/couchdb/data
      restart: always

  volumes:
    couchdb_data:
```

Command to run:
  docker compose up -d

---

## 2. Accessing the Web Dashboard (Fauxton)

The Web UI / Admin dashboard for CouchDB is called **Fauxton**.

### Access URL:
After starting the container, open your browser and navigate to:

  http://localhost:5984/_utils/

### Credentials:
* Username: `admin` (or the value set in `COUCHDB_USER`)
* Password: `mysecretpassword` (or the value set in `COUCHDB_PASSWORD`)

---

## 3. Service Health Check

To verify that CouchDB is running properly, execute:

  curl http://admin:mysecretpassword@localhost:5984/

Sample response:
```
  {
    "couchdb": "Welcome",
    "version": "3.3.3",
    "vendor": {
      "name": "The Apache Software Foundation"
    }
  }
```