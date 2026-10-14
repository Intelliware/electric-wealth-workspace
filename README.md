# Electric Wealth v2

Clone all four repos into the same parent folder (any name you like):

```
<parent folder>/
  electric-wealth-workspace/   ← this repo
  electric-wealth-db/
  electric-wealth-api/
  electric-wealth-web/
  electric-wealth-api-spring/  ← optional: the same API in Java / Spring Boot
```

Open `electric-wealth-workspace/electric-wealth.code-workspace` in VS Code to see all of them together.

Prerequisite: **Node.js 24** (workshop standard: 24.19.0). No Docker, database server or admin rights needed.

## Run it

Open two terminals, both in the parent folder.

**Terminal 1**: build the database, then start the API:

```bash
cd electric-wealth-db && npm run db:reset
cd ../electric-wealth-api && npm install && npm run dev   # API on :4100
```

**Terminal 2**: start the web app:

```bash
cd electric-wealth-web && npm install && npm run dev      # web on :5173
```

Sign in: `sarah.chen@example.com` / `Demo123!` / MFA `123456`.

Stop the API before running `db:reset` again.

## Optional: the Spring Boot API

`electric-wealth-api-spring` is the same API (same endpoints, same database) written in Java with Spring Boot, for
learning and comparison. It needs **Java 21**, and runs on port 4200 so it can run alongside the Node API:

```bash
cd electric-wealth-api-spring && ./mvnw spring-boot:run   # API on :4200
```

To point the web app at it, set `API_URL=http://localhost:4200` in `electric-wealth-web/.env` and restart the web app.

See each repo's README for its scripts.
