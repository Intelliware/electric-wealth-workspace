# Crash Course Exercise — Prerequisites

*Written against A.I.D.E. R2.6.0. Get through this before you start [`exercise-instructions.md`](exercise-instructions.md), which assumes all of it is already in place.*

**Verify all of this before you start.** Environment trouble is the single biggest risk to the exercise, and it is the one problem that teaches you nothing.

| You need | Detail |
|---|---|
| IDE and AI model | VS Code with GitHub Copilot in **agent mode**, or Cursor v2.3.x or later, with a model selected. IntelliJ IDEA works too — see the appendix of the instructions. |
| `git` and SSH | `git` on `PATH`, and an **RSA** SSH key on your Azure DevOps profile. See [SSH keys](#ssh-keys). |
| Azure CLI | `az` with the `azure-devops` extension, signed in. See [The Azure CLI](#the-azure-cli-and-the-devops-extension). |
| Repo access | Clone access to `AIDE`, `course-project-workspace`, `course-knowledge-base`, `course-specifications-repo`, and `ElectricWealthAIDE` in the `AI_Knowledge_Repo` project. See [Repo access](#repo-access). |

---

## SSH keys

**Every URL in the exercise is SSH.** It is one-time setup, and then git access stops being something you think about.

**Azure DevOps needs an RSA key.** Ed25519 keys are rejected.

```
ssh-keygen -t rsa -b 4096 -C "<your-email>"
cat ~/.ssh/id_rsa.pub
```

Paste the public key into Azure DevOps under **User settings → SSH public keys**, then prove it works:

```
ssh -T git@ssh.dev.azure.com
```

A banner naming you is success. A password prompt or `Permission denied (publickey)` means the key is not registered.

**If you end up with an `https://` remote anyway, change it rather than working around it** — `git remote set-url origin git@ssh.dev.azure.com:v3/...`.

---

## The Azure CLI and the DevOps extension

**Why you need it.** You use it to look at pull requests and branch policies from the command line, and — more to the point — the host agent can drive `az` for you once it is authenticated.

```
az --version
az extension add --name azure-devops
az login
az devops configure --defaults organization=https://dev.azure.com/IntelliwareDev project=AI_Knowledge_Repo
```

**Sign in with your `@intware.com` account.** IntelliwareDev does not use `@electricmind.com` identities, and signing in with the wrong one produces a confusing empty project list rather than an auth error.

**`az login` and the defaults are unrelated.** `az login` authenticates you; the organization and project defaults are a separate setting that the `azure-devops` extension reads. The `az devops configure` line is optional — it just saves you typing `--project` later.

**The extension resolves the project in this order:** an explicit `--project` on the command, then the Azure DevOps remote of the git repository you are standing in, then the configured default. Inside any of your clones the remote resolves it on its own, so the default only matters when you are in `~/dev/aide-course` itself, which is not a repository. `az devops configure --list` shows what you currently have set.

**Troubleshooting:**

- **`az` starts refusing commands partway through.** The `az login` session expires. Re-run it.
- **`WARNING: Auto-detect was enabled but no Azure DevOps remote was found`** means you are outside a clone, not that anything is wrong with SSH — auto-detection reads SSH remotes without trouble. `cd` into one of the repos, or pass `--project`.

---

## Repo access

Confirm you can see the repositories:

```
az repos list --output table
```

**Expect a longer list than five.** The project holds other repos too — what matters is that the five you need are in it and the command did not fail on authorization.

Then confirm you can actually clone, not just list:

```
git ls-remote git@ssh.dev.azure.com:v3/IntelliwareDev/AI_Knowledge_Repo/AIDE R2.6.0
```

One line with a SHA is success. If a repo is missing from the list or the command fails, ask for access before you start.

