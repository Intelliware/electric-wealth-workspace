# Electric Wealth v2

Clone these four repos into the same parent folder (any name you like):

```
<parent folder>/
  electric-wealth-workspace/   ← this repo
  electric-wealth-db/
  electric-wealth-web/
  electric-wealth-api-spring/
```

Open `electric-wealth-workspace/electric-wealth.code-workspace` in VS Code to see all of them together.

Prerequisites: **Node.js 24** (workshop standard: 24.19.0) and **Java 21**. No Docker, database server or admin rights needed.

## Run it

Open two terminals, both in the parent folder.

**Terminal 1**: build the database, then start the Spring API:

```bash
cd electric-wealth-db && npm run db:reset
cd ../electric-wealth-api-spring && ./mvnw spring-boot:run   # API on :4200
```

**Terminal 2**: start the web app:

```bash
cd electric-wealth-web && npm install && npm run dev      # web on :5173
```

Sign in: `sarah.chen@example.com` / `Demo123!` / MFA `123456`.

The web app is configured to use the Spring API on port 4200 by default. Stop the API before running `db:reset` again.

See each repo's README for its scripts.
