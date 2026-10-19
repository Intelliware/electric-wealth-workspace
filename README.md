# electric-wealth-workspace

Setup instructions for Electric Wealth, for people and AI agents.

Prerequisites: **Node.js 24** (workshop standard: 24.19.0) and **Java 21**. The database is SQLite, built into Node, so there's nothing else to install.

## Get the repos

In a parent folder (any name), clone all six repos side by side. Skip any you already have.

```bash
git clone https://github.com/adam-mccormick-em/electric-wealth-workspace.git
git clone https://github.com/adam-mccormick-em/electric-wealth-db.git
git clone https://github.com/adam-mccormick-em/electric-wealth-web.git
git clone https://github.com/adam-mccormick-em/electric-wealth-api.git
git clone https://github.com/adam-mccormick-em/electric-wealth-knowledge-base.git   # how the app works
git clone https://github.com/adam-mccormick-em/electric-wealth-specifications.git   # specifications
```

The app runs from `db`, `api` and `web`. `knowledge-base` and `specifications` are documentation.

Open `electric-wealth-workspace/electric-wealth.code-workspace` in VS Code to see all of them together.

### Branches

`main` is the clean starting point in every repo. Work happens on a branch with the same name in every repo. For example, the internal docs and the knowledge base are on `kb1`. To switch, run `git checkout <branch>` in each repo that has that branch.

## Run it

Open two terminals, both in the parent folder.

**Terminal 1**: build the database, then start the Spring API:

```bash
cd electric-wealth-db && npm run db:reset
cd ../electric-wealth-api && ./mvnw spring-boot:run   # API on :4200
```

**Terminal 2**: start the web app:

```bash
cd electric-wealth-web && npm install && npm run dev      # web on :5173
```

Sign in: `sarah.chen@example.com` / `Demo123!` / MFA `123456`.

The web app is configured to use the Spring API on port 4200 by default. Stop the API before running `db:reset` again.

> **AI agents:** if Node 24 or Java 21 is missing, stop and tell the user instead of installing them. Run both servers in the background and confirm they respond on ports 4200 and 5173.

See each repo's README for its scripts.
