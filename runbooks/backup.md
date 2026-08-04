docker run --rm \
  -v couchdb_data:/data \
  -v $(pwd):/backup \
  alpine \
  tar czf /backup/couchdb_backup.tar.gz -C /data .
