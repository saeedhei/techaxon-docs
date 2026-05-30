docker stop couchdb

docker run --rm \
  -v couchdb_data:/data \
  -v $(pwd):/backup \
  alpine \
  sh -c "rm -rf /data/* && tar xzf /backup/couchdb_backup.tar.gz -C /data"

docker start couchdb
