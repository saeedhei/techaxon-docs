docker run -d \
  --name couchdb \
  -p 5984:5984 \
  -e COUCHDB_USER=admin \
  -e COUCHDB_PASSWORD=admin123 \
  -v couchdb_data:/opt/couchdb/data \
  apache/couchdb:3.5.2
