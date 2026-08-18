```
curl -X PUT "https://couchdb.techaxon.de/_users/org.couchdb.user:a" \
  -u "admin:ADMIN_PASSWORD" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "a",
    "password": "YOUR_PASSWORD",
    "roles": [],
    "type": "user"
  }'
```

# with Fauxton
Log in:
https://db.techaxon.de/_utils/
Open Databases on the left.
Select _users.
Click New Doc.
Create a document with a similar structure to the following:
```
{
  "_id": "org.couchdb.user:saeed",
  "name": "saeed",
  "password": "YourStrongPassword",
  "roles": [],
  "type": "user"
}
```
