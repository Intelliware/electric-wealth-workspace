# Crash Course Exercise — Companion Guide

*Written against A.I.D.E. R2.6.0. The steps themselves are in [`exercise-instructions.md`](exercise-instructions.md); this guide explains them, step by step, and covers what to do when one goes wrong. Companion to [`crash-course-deck-outline.md`](crash-course-deck-outline.md) — slides 47–50 brief the exercise.*

**What you are building:** your own multi-root workspace with A.I.D.E. installed, the three Electric Wealth application repositories beside it, a knowledge base built from those repositories, and the start of a new feature through the brownfield flow.

**Six repositories, cloned as siblings.** Three are pre-created for the course — the project workspace, the knowledge base, and the specifications repo — and three are the application: `electric-wealth-web`, `electric-wealth-api-spring`, and `electric-wealth-db`. This is the same separation a real project uses, which is why you are setting it up rather than reading about it.

**Adapted from the full Day 0 runbook**, cut down to three source repositories and one person.

**Why hand the mechanical steps to your agent.** The commands are written out so you know what *should* happen and can tell when it has not — but cloning the repositories, writing a `.code-workspace` file, or fixing a path in `project.toml` is work you can describe in a sentence and let the agent do.

> **A fuller version exists.** There is an HTML setup guide written for clients standing up A.I.D.E. on a real project. It covers the same ground at more depth, plus the parts this exercise skips. Ask if you want it — but you do not need it for this exercise.

---

## 0. What we assume you have

**The exercise starts from a working environment** — an IDE with an AI agent, `git` over SSH, a signed-in Azure CLI, and access to the repos. Setting those up, and fixing them when they break, is covered in [`exercise-prerequisites.md`](exercise-prerequisites.md).

## How branching works

**`main` is frozen and protected on all six repos.** You cannot push to it and you cannot approve a merge into it — the branch policy requires a project administrator and you are not one. It stays the clean starting point the next participant clones.

**Your course branch is your `main`.** Everything you produce is committed and pushed there, and that is where it stays. Nothing merges to the real `main`.

**Why each increment gets its own pull request.** In execution, an approved pull request is the signal that an increment is done — that is how A.I.D.E. expects the stage to close, and it works the same here even though the target is your own branch rather than a shared trunk. No policy guards your branch, so you review and complete your own pull requests.

**Why not a pull request against `main`.** There is nothing to review, nothing to merge, and nobody who will approve it.

---

## 1. Clone the course repos

**What each repo is for.** The **project workspace** holds `project.toml` and the framework. The knowledge base and the specifications each get a repository of their own, and everything sits **side by side** as siblings — nothing nested inside anything else.

**Sibling versus nested is decided before you clone.** The exercise uses sibling. The two are not interchangeable, because every repository path in `project.toml` differs between them, so switching later means rewriting the config. IntelliJ needs the nested layout — see the [appendix](#why-intellij-needs-the-nested-layout).

**Picking a branch name.** Pick a name that is unique to you — `jdoe/aide-deep-dive` or `aide-2026-09-jdoe`. Reusing one name across all six repos is not enforced by anything; it just means you never have to think about which branch you are on in which folder.

**Delegating it.** "Create a branch called `<your-branch>` in each of these three course repos and check it out" is a complete instruction. The three application repos get the same branch in step 4. If your instructor cut branches for you already, "find the branch matching `<your-branch>` in each repo and check it out" works the same way — and the agent will tell you which repo is missing one rather than leaving you on `main` without noticing.

---

## 2. Install A.I.D.E.

**How the install works.** A.I.D.E. installs by having your coding agent follow its own `INSTRUCTIONS.md`. Nothing is compiled and nothing runs; you end up with a folder of markdown. The install is idempotent, so re-running it is also how you upgrade.

**Why the install prompt names the SSH URL and `R2.6.0`.** Without that, weaker models fall back to anonymous HTTP, hit a misleading `404` on a private repository, and report "repo not found" when the real problem is authentication.

**Release tags are `R<semver>`, not `v<semver>`.** `R2.6.0` exists; `v2.6.0` does not, and a pin written that way fails with an unhelpful unknown-ref error.

### What you should end up with

```
~/dev/aide-course/
├── course-project-workspace/        <- the install lands here
│   ├── README.md  .gitignore        <- what the repo shipped with
│   ├── AIDE/                        <- the framework itself
│   │   ├── .agents/skills/<stage>/<skill>/SKILL.md
│   │   ├── config/  templates/  references/
│   │   └── .aide-install.json       <- records source, ref, commit SHA, install time
│   ├── .agents/skills/<skill>/SKILL.md   <- 52 generated flat shims
│   └── AGENTS.md                    <- generated orientation for any agent opening this folder
├── course-knowledge-base/           <- cloned in step 1, still empty
└── course-specifications-repo/      <- cloned in step 1, still empty
```

The two course siblings stay empty until steps 6 and 7 fill them. The three application repos are cloned in step 4.

**Why the shims exist.** They are duplication on purpose: hosts scan `.agents/skills/` flat, while A.I.D.E. organises skills by stage, and the install bridges the two.

**Why `/` works before the workspace file exists.** The skills live in this folder and are discovered from it either way.

**Why commit the install.** It is a large first commit and it is meant to be — pinning the framework alongside your work is what makes the workspace reproducible later.

---

## 3. Write the workspace file and open it

**Why all six roots have to be listed.** The agent reads and writes per workspace folder root, so a knowledge base, a specifications repo, or an application repo that is not a root is a directory the agent cannot reliably reach. The six roots are the workspace folder itself, `course-knowledge-base`, `course-specifications-repo`, `electric-wealth-web`, `electric-wealth-api-spring`, and `electric-wealth-db`.

**The file is written before those three application repos exist.** Step 4 clones them into the paths this file already names. Open the workspace file after that clone, or reload the window if you opened it early — until the folders are on disk, those three roots have nothing to show.

**Writing the file is not the same as opening it, and this is the step people miss.** Your skills are not what is at stake — they live in the workspace folder, so they show up whether you open the folder or the workspace file. What you do not get from the plain folder is the siblings: the agent cannot read the knowledge base, the specifications, or the application source, and none of the reverse-engineering or build steps will work.

**Why you lose your chat.** Opening the workspace file gives you a fresh window, so the conversation does not carry over. Files on disk are untouched.

**What `AGENTS.md` does here.** Copilot and Cursor auto-attach `AGENTS.md` from *every* workspace folder root, which is what gives each chat both the framework orientation and any coding standards a source repository publishes. That only happens for folders the workspace file declares.

**The two checks fail for different reasons:**

- **No skills under `/`** — the problem is the install, not the workspace file. Confirm `.agents/skills/` exists at the root of your workspace clone and re-run the install prompt from step 2.
- **Only one root in the explorer** — you opened the folder rather than the `.code-workspace` file.
- **The three application roots are missing** — step 4 has not been cloned yet, or the window was not reloaded after it was.

---

## 4. Clone the Electric Wealth repos beside the others

**What you get.** Three clones, one application component each: the web app, the Spring API, and the database. Each lands on a frozen `main` — the application as it stands before anybody adds an RRSP calculator — and your own branch comes off that.

**Why beside the workspace folder, not inside it.** It keeps everything the framework writes out of each application's source tree.

**Same branch name as the course repos.** Step 1 already picked it. Use it again here so every folder is on `jdoe/aide-deep-dive`, not a mixture of `main` and your branch.

---

## 5. Run `/start-project`

**The five paths are what you have to get right.** Four of them do not default to where you want them and the fifth does not default at all, so this is the one screen in the exercise worth slowing down on.

**`{project-root}`** is whatever directory holds `project.toml`, so `../` reaches the siblings. Nothing is baked in at answer time — any of it can be fixed by editing `project.toml` afterwards.

**Why skip the decision table.** Unanswered codes are recorded as "no answer recorded", and the skills that need an answer ask you at the time. Answering fourteen project-level questions up front is work this exercise does not need.

**Why keep `features.toml`.** `/start-project` writes it without asking. It puts a directory named for your feature on the end of every artifact path, so your increments land in `course-specifications-repo/implementation/<your-feature>/INC-001.md`. You will meet it again in step 8.

### How many repository rows

**Here, one clone is one row.** A `[[project.repos]]` row is a reverse-engineering unit: one archetype, one technology stack, one extraction pass. Each Electric Wealth repository is already that unit, so declare three rows — one `code`, one `name`, one `path`, one `role` each:

```toml
[[project.repos]]
code = "ew-db"
name = "Electric Wealth DB"
path = "{project-root}/../electric-wealth-db"
role = "database"

[[project.repos]]
code = "ew-api"
name = "Electric Wealth API"
path = "{project-root}/../electric-wealth-api-spring"
role = "api"
primary = true

[[project.repos]]
code = "ew-web"
name = "Electric Wealth Web"
path = "{project-root}/../electric-wealth-web"
role = "ui"
```

Each row gets its own extraction, its own `source-docs/<code>/`, its own `repos/<code>/README.md`, and its own namespace under `components/`. A web change and an API change stop competing for the same documentation surface, and `/navigate-knowledge-base` can return the screen without dragging the API and the schema along with it.

**Still one knowledge base.** `knowledge_base_path` is a single path and there is no multi-KB configuration. Separate application repositories means separate **repo codes inside the one KB repo** — `source-docs/ew-web/` beside `source-docs/ew-api/`, `components/ew-web/` beside `components/ew-api/`.

**Order the three passes.** Shared data first, then the API, then the UI — so database, API, web. Run `/analyze-repository` once per repository and follow the skill it names. Do not run the three extractions in parallel.

**Declare only the rows you will actually extract.** `/build-knowledge-base` checks that every declared row has a `source-docs/<code>/` directory and halts with **F-006** when one does not. Declaring three rows and finishing one pass does not give you a partial knowledge base — it gives you a failed build.

### Target shape

```
~/dev/aide-course/
├── course-project-workspace/    <- {project-root}
│   ├── project.toml
│   ├── features.toml
│   ├── AIDE/
│   └── aide-course.code-workspace
├── course-knowledge-base/       <- ARCHITECTURE.md, components/, source-docs/
├── course-specifications-repo/  <- requirements / design / implementation / quality
├── electric-wealth-web/         <- the web app
├── electric-wealth-api-spring/  <- the API
└── electric-wealth-db/          <- the database
```

> **This is the shape a real project uses, not a teaching simplification.** The separate repositories buy specifications their own pull-request stream and reviewers, give the knowledge base a steward and an access boundary, and keep `project.toml`'s history readable instead of burying config changes under a flood of artifact commits. You are setting it up here so that the first time you do it is not on a live engagement.

---

## 6. Reverse-engineer the repositories

**What `/analyze-repository` does.** Point it at one repository at a time. It classifies that repository and names the exact skill — or skills — to run next, in order. Database, then API, then web.

**Why read the file-structure gate.** It proposes headers only and writes nothing until you say yes. This is where an oversized extraction gets pruned.

**Where the output lands.**

- **Internal docs** — for whoever will *change* the code — land in that source repository at `docs/<domain>-internal/`.
- **Consumer docs** — for whoever will *call* it — land in the knowledge base at `course-knowledge-base/source-docs/<code>/<domain>-consumer/`. Each application repo has its own `<code>`.

You will also see these called **Part A** (internal) and **Part B** (consumer) in A.I.D.E.'s own documentation.

**Tests.** Co-located tests do not get their own run — the application's reverse-engineering skill documents them in its `## Testing` section.

---

## 7. Build the knowledge base

**Why the copy step exists.** Consumer docs are already in the knowledge base; internal docs are in each source repository, where the build cannot see them. Copy each repo's `docs/<domain>-internal/` into `course-knowledge-base/source-docs/<code>/<domain>-internal/`, using the `code` you gave that repo in `project.toml`. Three repos, three copies.

**Why it is the most common failure.** Skip a copy and the build still succeeds — it just produces a thin knowledge base rather than failing loudly. Skipping all three leaves out every application component.

**Folder naming.** The build reads everything under `source-docs/<code>/`, so the exact folder name is yours to choose. Keeping the `<domain>-internal` name just keeps the two halves legible side by side.

**Symlinks.** A symlink works on macOS and Linux; on Windows it needs Developer Mode or an elevated shell, so copying is the portable answer. Either way it is a snapshot — re-run reverse engineering and you copy again.

**`ARCHITECTURE.md` is the payoff.** It is the first thing every later skill reads.

**Why push the KB now.** It is a repository with a remote, so treat it like one — and it is the artifact you are most likely to want back when you pick this up again.

---

## 8. Start the feature

**Why `/select-feature` comes first.** The stage skills will stop and tell you to run it otherwise. That is `features.toml` working as intended, not a broken setup.

**What `/navigate-knowledge-base` returns.** At most seven component files with a reason for each. Read those rather than the repository.

**Why let the agent install the toolchain.** The application is three stacks — a web app, a Spring API, and a database — and standing one of them up means whatever that repo builds with. **This is the demo, not a detour.** Onboarding onto an unfamiliar stack is one of the most tedious parts of joining a project, and watching it resolve into one prompt is worth seeing even if you never get to the RRSP calculator.

**Why each increment closes with a pull request.** The approval is what marks the increment done.

**Why finishing is not the goal.** Framework state lives on disk rather than in the chat, so stopping mid-flow costs you nothing — reopen the workspace later and carry on from wherever you stopped.

**`/framework-navigator`** takes a plain-English goal and names one skill. The host agent works too — it has already read the framework.

---

## 9. Push all six repos

**Why all six.** Your work is spread across six repositories, and a commit in one does not cover the others.

**Why ask the agent.** It is faster than typing the commands, and it will report the one repo that had nothing to commit instead of silently exiting non-zero. The same goes for anything on the Azure DevOps side — raising a pull request, checking a policy, finding a branch. The agent has `git` and `az` and is better at remembering the flags than you are.

**The three application repos are the ones people forget.** Reverse engineering wrote internal docs into each of them at `docs/<domain>-internal/`, and any code you wrote during execution is there too.

**Nothing reaches `main`.** Your branch is the deliverable.

---

## Appendix — IntelliJ IDEA

### Why IntelliJ needs the nested layout

**IntelliJ has no multi-root equivalent**, so the sibling layout will not work. An IntelliJ project has exactly one root — the directory containing `.idea/` — and skills are only discovered beneath it. Unlike VS Code there is no setting to register an extra discovery root, so root placement is the only lever.

**Decide before you clone.** Every repository path in `project.toml` differs between the two layouts.

**Why the shipped `.gitignore` matters.** It already lists the nested clones: the knowledge base, the specifications repo, and the three application repos. Without those lines Git records the nested checkouts as gitlinks and `git status` becomes noise.

**Multiple Git roots are fine.** IntelliJ handles several Git roots in one project well; commits and branches stay per-repo.

**Why the explicit checks.** JetBrains sees less exercise than the other two hosts, so these checks are explicit rather than assumed. Cross-repo access is the one worth checking most carefully.
