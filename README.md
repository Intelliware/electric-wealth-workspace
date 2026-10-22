# electric-wealth-workspace

Setup instructions for Electric Wealth, for people and AI agents.

Prerequisites: **Node.js 24** (workshop standard: 24.19.0) and **Java 21**. The database is SQLite, built into Node, so there's nothing else to install.

[`GUIDE.md`](./GUIDE.md) explains every step below in much more detail — why it is there and what to do when it goes wrong. Refer to it when you need to, but follow these simple instructions first.

## 1. Get the repos

In a parent folder (any name), clone all six repos side by side. Skip any you already have.

```bash
git clone https://github.com/Intelliware/electric-wealth-workspace.git
git clone https://github.com/Intelliware/electric-wealth-db.git
git clone https://github.com/Intelliware/electric-wealth-web.git
git clone https://github.com/Intelliware/electric-wealth-api.git
git clone https://github.com/Intelliware/electric-wealth-knowledge-base.git # how the app works
git clone https://github.com/Intelliware/electric-wealth-specifications.git # specifications
```

The app runs from `db`, `api` and `web`. `knowledge-base` and `specifications` are documentation.

Open `electric-wealth-workspace/electric-wealth.code-workspace` in VS Code to see all of them together.

### Branches

- `main` is frozen on all six repos. Do not push to it or raise a PR against it.
- Work on **your own branch**, with the same name in all six repos — e.g. `jdoe`.
- For each increment, branch off your course branch, e.g. `jdoe/analysis`, and raise a PR back into it. Review and complete it yourself.

## 2. Run it

Open two terminals, both in the parent folder.

**Terminal 1**: build the database, then start the Spring API:

```bash
cd electric-wealth-db && npm run db:reset
cd ../electric-wealth-api && ./mvnw spring-boot:run # API on :4200
```

**Terminal 2**: start the web app:

```bash
cd electric-wealth-web && npm install && npm run dev # web on :5173
```

Sign in: `sarah.chen@example.com` / `Demo123!` / MFA `123456`.

The web app is configured to use the Spring API on port 4200 by default. Stop the API before running `db:reset` again.

> **AI agents:** if Node 24 or Java 21 is missing, stop and tell the user instead of installing them. Run both servers in the background and confirm they respond on ports 4200 and 5173.

## 3. Install A.I.D.E.

A.I.D.E. is installed by your AI agent. Use the prompt below to install A.I.D.E.

**Prompt:**

```text
Install AIDE from https://github.com/Intelliware/aide.git using AIDE_REF=R2.8.0 per its INSTRUCTIONS.md.
```

**Check:**

Type `/` in agent chat — **52 skills** appear. Then **commit the install** to your branch.


## 4. Exercises

When you have your project set up and A.I.D.E. installed, you can move on to the [exercises](./exercises/01.md).