# Crash Course Exercise — Instructions

*Written against A.I.D.E. R2.6.0. The why behind each step, troubleshooting, and the longer explanations are in [`exercise-companion-guide.md`](exercise-companion-guide.md).*

**Goal:** a multi-root workspace with A.I.D.E. installed, **ElectricWealthAIDE** beside it, a knowledge base built from it, and a feature started through the brownfield flow.

**IntelliJ IDEA users:** go to the [appendix](#appendix--intellij-idea) before you clone anything.

---

## 0. What we assume you have

These instructions assume all of the following is already set up:

- **An IDE with an AI agent** — VS Code with GitHub Copilot in agent mode, or Cursor — with a model selected
- `git` **with SSH access** to Azure DevOps
- **The Azure CLI** with the `azure-devops` extension, signed in
- **Clone access** to `AIDE`, `course-project-workspace`, `course-knowledge-base`, `course-specifications-repo`, and `ElectricWealthAIDE`

**If any of that is missing, stop here** and work through [`exercise-prerequisites.md`](exercise-prerequisites.md) first.

## Branching rules

- `main` is frozen on all four repos. Do not push to it or raise a PR against it.
- Work on **your own branch**, with the same name in all four repos — e.g. `jdoe/aide-deep-dive`.
- For each increment, branch off your course branch and raise a PR back into it. Review and complete it yourself.

---



## 1. Clone the course repos

**Option A — run the commands:**

```
mkdir -p ~/dev/aide-course && cd ~/dev/aide-course
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/course-project-workspace
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/course-knowledge-base
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/course-specifications-repo
(cd course-project-workspace   && git checkout -b <your-branch>)
(cd course-knowledge-base      && git checkout -b <your-branch>)
(cd course-specifications-repo && git checkout -b <your-branch>)
```

**Option B — ask the agent:**

```text
In the current workspace, clone course-project-workspace, course-knowledge-base, and course-specifications-repo from git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/, then create a branch called <your-branch> in each.
```



## 2. Install A.I.D.E.

From `~/dev/aide-course/course-project-workspace`, 

**Block 1:**

```text
Install AIDE from git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/AIDE using AIDE_REF=R2.6.0 per its INSTRUCTIONS.md.
```

**Check:** type `/` in agent chat — **52 skills** appear. Then **commit the install** to your branch.

## 3. Write the workspace file and open it

**Option A — create the file yourself** at `course-project-workspace/aide-course.code-workspace`:

```json
{
  "folders": [
    { "path": ".",                             "name": "workspace (project root)" },
    { "path": "../course-knowledge-base",      "name": "knowledge-base" },
    { "path": "../course-specifications-repo", "name": "specifications" },
    { "path": "../electric-wealth-web",        "name": "ElectricWealthWeb" }
    { "path": "../electric-wealth-api-spring", "name": "ElectricWealthAPISpring" }
    { "path": "../electric-wealth-db",         "name": "ElectricWealthDB" }
  ]
}
```

**Option B — ask the agent:**

```text
Create course-project-workspace/aide-course.code-workspace with four roots: the workspace folder itself, and its siblings course-knowledge-base, course-specifications-repo, and ElectricWealthAIDE.
```

Either way, **open the file itself** — File → Open Workspace from File. This opens a new window; start a new chat there.

**Check:** `/` shows 52 skills, and the explorer shows **all four** folders.

## 4. Clone ElectricWealthAIDE beside the others

**Option A — run the commands:**

```
cd ~/dev/aide-course
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/electric-wealth-web
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/electric-wealth-api-spring
git clone git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/electric-wealth-db
(cd electric-wealth-web && git checkout -b <your-branch>)
(cd electric-wealth-api-spring && git checkout -b <your-branch>)
(cd electric-wealth-db && git checkout -b <your-branch>)
```

**Option B — ask the agent:**

```text
Clone electric-wealth-web, electric-wealth-api-spring, electric-wealth-db from https://dev.azure.com/IntelliwareDev/AI_Knowledge_Repo into this root workspace, sibling to the other repos, then within each, create a branch called <your-branch> in it.
```

Either way, you end up with:

```
~/dev/aide-course/
├── course-project-workspace/    <- {project-root}
├── course-knowledge-base/
├── course-specifications-repo/
└── electric-wealth-web/
└── electric-wealth-api-spring/
└── electric-wealth-db/
```



## 5. Run `/start-project`

Choose **brownfield**, then:


| Question                   | Answer                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------ |
| Flow                       | **brownfield**                                                                                         |
| The four `*_package` paths | `{project-root}/../course-specifications-repo/<stage>`                                                 |
| `knowledge_base_path`      | `{project-root}/../course-knowledge-base` — **no default, type it**                                    |
| Repos                      | **One row:** a `code`, a `name`, `path` = `{project-root}/../<electric-wealth-repos...>`, and a `role` |
| Dedicated QE repo          | No                                                                                                     |
| Decision-table codes       | **Skip**                                                                                               |
| Glossary, change log       | Skip or accept defaults                                                                                |


The paths should come out as:

```toml
[project.paths]
requirements_package   = "{project-root}/../course-specifications-repo/requirements"
design_package         = "{project-root}/../course-specifications-repo/design"
implementation_package = "{project-root}/../course-specifications-repo/implementation"
quality_package        = "{project-root}/../course-specifications-repo/quality"
knowledge_base_path    = "{project-root}/../course-knowledge-base"
```

**Keep the `features.toml` it writes.** Wrong answers can be fixed later by editing `project.toml`.

## 6. Reverse-engineer the repositories

```
/analyze-repository
```

Run the skill(s) it names, **one at a time**.

- **Read the file-structure gate** before you approve it.
- Close out by **attesting**: accept the `automated` verdict, or run `/review-reverse-engineering-package`.

## 7. Build the knowledge base

Either by using the specific commands below or by prompting the agent yourself, build and verify the knowledge base

```
/build-knowledge-base
/verify-knowledge-base-coverage --mode=baseline
```

**Read `course-knowledge-base/ARCHITECTURE.md`.** Then commit and push the KB on your branch.

## 8. Start the feature

```
/select-feature
```

Mint one, e.g. `rrsp-calculator`. Then:

```
/navigate-knowledge-base
<describe the RRSP calculator change>
```

Read the components it returns, not the repository. Then work as far as you get, with the review gate at each stage:

```
requirements package -> solution design -> planning -> execution
```

- **To run the app**, ask the agent: `Install whatever I need to build and run this app, then start the mobile simulator.`
- **Each increment closes with a PR** into your course branch. Raise, review, approve, complete.
- **Stuck?** `/framework-navigator`, or ask the agent.



## 9. Push all repos

**Option A — run the commands:**

```
cd ~/dev/aide-course
(cd course-project-workspace   && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
(cd course-knowledge-base      && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
(cd course-specifications-repo && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
(cd electric-wealth-api-spring         && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
(cd electric-wealth-web         && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
(cd electric-wealth-db         && git add -A && git commit -m "aide deep dive: <what you got to>" && git push -u origin HEAD)
```

**Option B — ask the agent:**

```text
Commit and push everything on my branch in all repos.
```

**Do not forget the electric wealth app repos** — they holds the internal docs and any code you wrote.

---



## Appendix — IntelliJ IDEA

Use the **nested** layout: everything inside `course-project-workspace`. [Why](exercise-companion-guide.md#why-intellij-needs-the-nested-layout).

```
~/dev/aide-course/
└── course-project-workspace/     <- IntelliJ project root, {project-root}
    ├── .idea/
    ├── AIDE/  .agents/skills/  AGENTS.md  project.toml
    ├── course-knowledge-base/
    ├── course-specifications-repo/
    └── electric-wealth-web/
    └── electric-wealth-api-spring/
    └── electric-wealth-db/
```

1. Clone `course-project-workspace`, cut your branch, and run the install (step 2) from **inside it**.
2. Clone the other repos **into** it, each on your branch. Leave the shipped `.gitignore` lines for them alone.
3. **File → Project Structure → Modules** — import each nested directory as a module.
4. **Settings → Version Control → Directory Mappings** — register each as its own Git root.
5. In `/start-project`, drop the `../` from every path: `{project-root}/course-knowledge-base`, `{project-root}/course-specifications-repo/<stage>`, `{project-root}/<electric-wealth-repos...>`.

**Check all four:**

- [ ] `/` shows the full skill list
- [ ] `/framework-navigator` responds as a skill
- [ ] The agent can read `project.toml` and state the project code
- [ ] The agent can quote a line from a file in ElectricWealthAIDE