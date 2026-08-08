# ger _rev
curl -X PUT "https://couchdb.techaxon.de/_users/org.couchdb.user:a" \

# then add _rev
curl -X PUT "https://couchdb.techaxon.de/_users/org.couchdb.user:a" \
-u "admin:password" \
-H "Content-Type: application/json" \
-d '{
  "_rev": "1-376822bd9e566c234b4e00655285b5f5",
  "name": "a",
  "password": "pass",
  "roles": [],
  "type": "user"
}'

