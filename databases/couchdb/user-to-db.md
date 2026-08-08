curl -X PUT "https://couchdb.techaxon.de/kanban_test/_security" \
  -u "admin:ADMIN_PASSWORD" \
  -H "Content-Type: application/json" \
  -d '{
    "admins": {
      "names": [],
      "roles": []
    },
    "members": {
      "names": ["a"],
      "roles": []
    }
  }'
