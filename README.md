# Electric Wealth v2

Four repos, cloned side by side into one folder:

```
some-folder/
  electric-wealth-workspace/   ← this repo
  electric-wealth-db/
  electric-wealth-api/
  electric-wealth-web/
```

Open `electric-wealth-workspace/electric-wealth.code-workspace` in VS Code to see all of them together.

Prerequisite: **Node.js 24** (workshop standard: 24.19.0). No Docker, database server or admin rights needed.

## Run it

From `some-folder`:

```bash
cd electric-wealth-db  && npm run db:reset                    # build the database
cd ../electric-wealth-api && npm install && npm run dev        # API on :4100
cd ../electric-wealth-web && npm install && npm run dev        # web on :5173 (second terminal)
```

Sign in: `sarah.chen@example.com` / `Demo123!` / MFA `123456`.

Stop the API before running `db:reset` again.

See each repo's README for its scripts.
