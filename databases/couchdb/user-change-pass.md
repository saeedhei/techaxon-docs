curl -X PUT "https://couchdb.techaxon.de/_users/org.couchdb.user:a" \
  -u "admin:ADMIN_PASSWORD" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "a",
    "password": "NEW_PASSWORD",
    "roles": [],
    "type": "user"
  }'
