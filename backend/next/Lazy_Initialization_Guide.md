# Developer Guide: Preventing Build-Time Crashes with Lazy Initialization in Next.js

## 1. Context & Problem Statement
In Next.js, static routes and API handlers are evaluated during the build phase (`next build`). If external service clients (e.g., CouchDB, Redis, Database ORMs) execute connection logic at the top level of a module, Next.js attempts to establish those connections or validate environment variables **during the build process**.

When runtime secrets (such as `COUCH_PASS` or `DATABASE_URL`) are stored exclusively on the production server (VPS) for security reasons, the build environment lacks these variables. This leads to build failures such as:

Error: Configuration is missing
    at module evaluation (lib/db.ts:10:9)
    at ...
Error: Failed to collect page data for /api/route

---

## 2. The Solution: Lazy Initialization
To prevent top-level execution during `next build`, we defer the client initialization until the function is explicitly invoked at runtime. 

By wrapping the instantiation inside a function (a singleton or getter), the connection logic executes **only when an actual HTTP request hits the API route on the server**.

---

## 3. Implementation Patterns

### Anti-Pattern: Top-Level Execution (Avoid)
The following code runs as soon as the file is imported during the build step, causing builds to fail when environment variables are missing.

// lib/couchdb.ts
import nano from 'nano';

const dbUser = process.env.COUCH_USER;
const dbPass = process.env.COUCH_PASS;
const dbHost = process.env.COUCH_HOST;
const dbName = process.env.COUCH_DB;

// Throws an error during `next build` because env vars are missing in CI/CD!
if (!dbUser || !dbPass || !dbHost || !dbName) {
  throw new Error('CouchDB configuration is missing');
}

const couchUrl = `https://${encodeURIComponent(dbUser)}:${encodeURIComponent(dbPass)}@${dbHost}`;
export const couch = nano(couchUrl);
export const kanbansDB = couch.db.use(dbName);

---

### Recommended Pattern: Lazy Singleton Initialization
Wrap client instantiation inside a function. This guarantees that environment variables are evaluated strictly at Runtime on the server.

// lib/couchdb.ts
import nano from 'nano';

let dbInstance: ReturnType<typeof nano.db.use> | null = null;

export function getKanbanDb() {
  // Reuse existing instance if already initialized in memory
  if (dbInstance) {
    return dbInstance;
  }

  const dbUser = process.env.COUCH_USER;
  const dbPass = process.env.COUCH_PASS;
  const dbHost = process.env.COUCH_HOST;
  const dbPort = process.env.COUCH_PORT || '5984';
  const dbName = process.env.COUCH_DB;
  const dbProtocol = process.env.COUCH_PROTOCOL || 'https';

  if (!dbUser || !dbPass || !dbHost || !dbName) {
    throw new Error('[Runtime Error] CouchDB configuration is missing on server environment.');
  }

  const couchUrl = `${dbProtocol}://${encodeURIComponent(dbUser)}:${encodeURIComponent(dbPass)}@${dbHost}:${dbPort}`;
  const couch = nano(couchUrl);
  
  dbInstance = couch.db.use(dbName);
  return dbInstance;
}

---

## 4. Usage in Route Handlers

Instead of importing a global database variable, invoke `getKanbanDb()` inside the request handler:

// app/api/boards/route.ts
import { NextResponse } from 'next/server';
import { getKanbanDb } from '@/lib/couchdb';

export async function GET() {
  try {
    // Client is initialized strictly when the endpoint is requested
    const db = getKanbanDb();
    const result = await db.list();

    return NextResponse.json({ data: result });
  } catch (error) {
    console.error('API Error:', error);
    return NextResponse.json({ error: 'Internal Server Error' }, { status: 500 });
  }
}

---

## Summary Checklist
1. Never instantiate SDKs, DB drivers, or third-party connections at the top-level of files.
2. Use getter functions (`getDbClient()`) for database connections.
3. CI/CD pipeline requires only `NEXT_PUBLIC_*` build arguments.
4. All private credentials (`COUCH_PASS`, `DATABASE_URL`, etc.) remain safely isolated on the production server (VPS).
