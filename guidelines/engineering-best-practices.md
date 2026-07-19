## Database Conventions

### CouchDB Document ID Format

All documents stored in CouchDB must adhere to a strict ID and typing structure to optimize indexing and querying performance.

- **Format:** `<type>:<UUIDv7>`
- **Rule:** All document IDs must use a lowercase entity type prefix, followed by a colon, and end with a time-sorted UUIDv7.
- **Requirement:** The document body **must** also include a matching `type` field.

#### Example Structures:

```json
// User Document
{
  "_id": "user:01980a1d-5d9d-7c0f-a4a5-0e4c53d2c9c1",
  "type": "user"
}

// Order Document
{
  "_id": "order:01980a1d-5d9d-7b2a-b3c4-1f2e3d4c5b6a",
  "type": "order"
}

// Session Document
{
  "_id": "session:01980a1d-5d9d-7e3f-92a1-8b7c6d5e4f3a",
  "type": "session"
}
```
