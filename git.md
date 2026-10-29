# Git and GitHub — Complete Practical Notes

## 1. Git Workflow

**Working Directory → Staging Area → Local Repository → Remote Repository (GitHub)**

- Working Directory: Create or modify files.
- Staging Area: Select changes for the next commit.
- Local Repository: Store commits locally.
- Remote Repository: Share commits through GitHub.

| Command | Purpose |
|---|---|
| `git add` | Working directory → staging area |
| `git commit` | Staging area → local repository |
| `git push` | Upload local commits to remote |
| `git fetch` | Download remote updates without integrating them |
| `git pull` | Fetch and integrate remote updates |
| `git restore` | Restore file content or unstage changes |
| `git reset` | Unstage changes or move a branch reference |
| `git revert` | Create a new commit that reverses an earlier commit |

## 2. Installation and Configuration

```bash
git --version
git -v
git config --global --list
git config --global user.name "Srinu"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list --show-origin
```

- `git --version` — Check installed Git version.
- `git config --global --list` — View global configuration.
- `git config --global user.name` and `user.email` — Set your commit identity.
- `git config --list --show-origin` — Display configuration values and their source files.

Replace the example name and email with your own details.

## 3. Initialize and Check Repository Status

```bash
git init
git status
git status -sb
git add devops.txt
git add .
git add -A
git add *.txt
git rm devops.txt
git rm --cached devops.txt
```

| Command | Purpose |
|---|---|
| `git init` | Initialize a Git repository |
| `git status` | Display current changes and branch status |
| `git status -sb` | Display concise status |
| `git add devops.txt` | Stage one file |
| `git add .` | Stage new and modified files under the current directory; stages tracked deletions too |
| `git add -A` | Stage additions, modifications, and deletions throughout the working tree |
| `git add *.txt` | Add matching text files; shell expansion can affect the result |
| `git rm devops.txt` | Delete a tracked file and stage its deletion |
| `git rm --cached devops.txt` | Stop tracking a file while keeping its local copy |

**Note:** `git add *` is not always equivalent to `git add .`. For ordinary staging, use `git add .` or `git add -A`.

## 4. Commit and View History

```bash
git add .
git commit -m "Add deployment script"
git commit -am "Update deployment script"
git log
git log --oneline
git log --oneline --graph --all
git show <commit-id>
git show --pretty="" --name-only <commit-id>
git show --stat <commit-id>
```

- `git commit -m "message"` — Commit staged changes.
- `git commit -am "message"` — Stage and commit modifications to tracked files; it does not automatically add new files.
- `git log` — Display commit history.
- `git log --oneline` — Display compact commit history.
- `git log --oneline --graph --all` — Visualize branches and commits.
- `git show <commit-id>` — Display commit details and changes.
- `git show --pretty="" --name-only <commit-id>` — Display file names changed in a commit.
- `git show --stat <commit-id>` — Display a summary of changed files and line counts.

## 5. Compare Changes

```bash
git diff
git diff --staged
git diff main..f1
git diff main...f1
```

- `git diff` — Compare working-directory changes with the staging area.
- `git diff --staged` — Compare staged changes with the latest commit.
- `git diff main..f1` — Compare the content at the tips of two branches.
- `git diff main...f1` — Compare branch `f1` against the common ancestor of `main` and `f1`.

**Real-time use case:** Review your staged changes before committing them.

## 6. Restore, Reset, and Revert

### Git Restore

```bash
git restore devops.txt
git restore --staged devops.txt
git restore --source=<commit-id> devops.txt
```

- `git restore devops.txt` — Discard unstaged changes to a tracked file.
- `git restore --staged devops.txt` — Unstage a file while keeping its working-directory changes.
- `git restore --source=<commit-id> devops.txt` — Restore file content from a specific commit.

**Warning:** `git restore devops.txt` can discard your unstaged edits to that file.

### Git Reset

```bash
git reset devops.txt
git reset --mixed <commit-id>
git reset --soft <commit-id>
git reset --hard <commit-id>
```

- `git reset devops.txt` — Unstage the file while preserving working-directory changes.
- `git reset --mixed <commit-id>` — Move the current branch back; changes from later commits become unstaged.
- `git reset --soft <commit-id>` — Move the current branch back; changes from later commits remain staged.
- `git reset --hard <commit-id>` — Move the branch back and discard tracked changes after that point.

`--mixed` is the default mode. Therefore, `git reset <commit-id>` is equivalent to `git reset --mixed <commit-id>`.

**Warning:** `git reset --hard` can permanently discard uncommitted work and make commits harder to recover.

### Git Revert

```bash
git revert <commit-id>
```

Creates a new commit that reverses the changes introduced by an earlier commit.

**Real-time use case:** Undo a faulty change that has already been pushed to GitHub without rewriting shared history.

```bash
git log --oneline
git revert <commit-id>
git push origin main
```

If conflicts occur, resolve them and stage the resolved files before continuing.

### Amend the Latest Commit

```bash
git commit --amend -m "Correct commit message"
```

Modify the latest commit's message and include any staged changes. Avoid amending shared commits unless you understand the consequences of rewriting history.

## 7. Branch Management

```bash
git branch
git branch dev
git switch dev
git switch -c f1
git checkout -b f1
git branch -d dev
git branch -D f1
git branch -m old-name new-name
git branch -M main
git branch -r
git branch -a
git switch -
```

| Command | Purpose |
|---|---|
| `git branch` | List local branches |
| `git branch dev` | Create a branch |
| `git switch dev` | Switch to an existing branch |
| `git switch -c f1` | Create and switch to a new branch |
| `git checkout -b f1` | Alternative command to create and switch branches |
| `git branch -d dev` | Delete a merged local branch |
| `git branch -D f1` | Force-delete a local branch, even if unmerged |
| `git branch -m old-name new-name` | Rename a local branch |
| `git branch -M main` | Force-rename the current branch to `main` if necessary |
| `git branch -r` | List remote-tracking branches |
| `git branch -a` | List local and remote-tracking branches |
| `git switch -` | Switch to the previous branch |

**Note:** `git branch -D` deletes only the local branch.

## 8. Merge and Merge Conflicts

```bash
git switch main
git merge f1
git merge --abort
git log --oneline --graph --all
```

- `git merge f1` — Merge `f1` into the current branch.
- `git merge --abort` — Attempt to cancel an in-progress merge.
- `git log --oneline --graph --all` — Visualize merge history.

### Fast-Forward Merge

A fast-forward merge moves the target branch forward when it has no divergent commits. It does not require a merge commit.

### Three-Way Merge

Git uses the common ancestor and both branch tips to combine changes. Git may create a merge commit.

### ORT Merge Strategy

ORT is the modern default strategy for ordinary two-branch merges in current Git versions.

### Resolve a Merge Conflict

Conflict markers look like this:

```text
<<<<<<< HEAD
Current branch content
=======
Incoming branch content
>>>>>>> feature-login
```

Resolve the file, remove the markers, and then run:

```bash
git add <resolved-file>
git commit
```

## 9. Remote Repositories and GitHub

```bash
git remote -v
git remote add origin <URL>
git remote set-url origin <URL>
git remote remove origin
git clone <URL>
git clone -b qa <URL>
git fetch origin
git fetch --prune origin
git pull origin main
git push origin main
git push -u origin main
git push origin --delete dev
```

| Command | Purpose |
|---|---|
| `git remote -v` | Display remote URLs |
| `git remote add origin <URL>` | Add a remote named `origin` |
| `git remote set-url origin <URL>` | Change the remote URL |
| `git remote remove origin` | Remove remote configuration locally |
| `git clone <URL>` | Clone a repository |
| `git clone -b qa <URL>` | Clone and check out branch `qa` |
| `git fetch origin` | Download remote updates without integrating them |
| `git fetch --prune origin` | Fetch updates and remove stale remote-tracking references |
| `git pull origin main` | Fetch and integrate `main` into the current branch |
| `git push origin main` | Push local `main` commits to remote `main` |
| `git push -u origin main` | Push and set the upstream tracking branch |
| `git push origin --delete dev` | Delete the remote `dev` branch |

**Correction:** Use `git fetch --prune origin`, not `git fetch prune`.

### Fetch vs Pull

- `git fetch origin` downloads remote changes without integrating them into your current branch.
- `git pull origin main` fetches remote `main` and integrates it into the current branch. The integration can use merge or rebase, depending on configuration and options.

### Push a New Branch

```bash
git switch -c feature-login
git push -u origin feature-login
```

After setting the upstream, you can generally use `git push` and `git pull` without specifying the remote and branch.

### Fast-Forward-Only Pull

```bash
git pull --ff-only origin main
```

Integrates remote changes only when a fast-forward is possible. It fails rather than creating a merge commit if histories have diverged.

## 10. SSH Authentication

### Generate an SSH Key

Recommended Ed25519 key:

```bash
cd ~
mkdir -p ~/.ssh
ssh-keygen -t ed25519 -C "you@example.com" -f ~/.ssh/srinu-rsa
```

The filename `srinu-rsa` is custom; the key type in this example is Ed25519.

If RSA is specifically required:

```bash
ssh-keygen -t rsa -b 4096 -C "you@example.com" -f ~/.ssh/srinu-rsa
```

### Display the Public Key

```bash
cat ~/.ssh/srinu-rsa.pub
```

Copy the public key and add it under GitHub → Settings → SSH and GPG keys → New SSH key.

**Never share your private key.** The private key is the file without `.pub`.

### Test SSH Authentication

```bash
ssh -T git@github.com
```

### Change the Repository to SSH

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
git remote -v
```

Replace `USERNAME` and `REPOSITORY` with the actual values.

## 11. Rebase

```bash
git switch feature-login
git rebase main
```

Replays feature commits on top of `main`, producing new commit IDs for the replayed commits.

### Resolve a Rebase Conflict

```bash
# Resolve the conflict in the file
git add <resolved-file>
git rebase --continue
```

Cancel the rebase:

```bash
git rebase --abort
```

**Real-time use case:** Update your feature branch with recent `main` changes while keeping a linear history.

**Warning:** Avoid rebasing commits already shared with teammates unless your team agrees to rewrite that history.

## 12. Cherry-Pick

```bash
git switch main
git cherry-pick <commit-id>
```

Applies the changes from a specific commit onto the current branch as a new commit.

**Real-time use case:** Apply a bug fix from `dev` to `main` without merging every change from `dev`.

## 13. Tags and Releases

```bash
git tag
git tag billing-1.0.1
git tag -a billing-1.0.1 -m "Billing release 1.0.1"
git show billing-1.0.1
git push origin billing-1.0.1
git push origin --tags
git tag -d billing-1.0.1
git push origin --delete billing-1.0.1
```

- `git tag` — List local tags.
- `git tag billing-1.0.1` — Create a lightweight tag.
- `git tag -a ... -m ...` — Create an annotated tag with a message.
- `git show billing-1.0.1` — Inspect the tag and its commit.
- `git push origin billing-1.0.1` — Push one tag.
- `git push origin --tags` — Push all local tags.
- `git tag -d billing-1.0.1` — Delete the local tag.
- `git push origin --delete billing-1.0.1` — Delete the remote tag.

Annotated tags are generally preferable for release versions.

## 14. Stash

Stash temporarily saves uncommitted work so you can switch tasks or branches.

```bash
git stash push -m "WIP login"
git stash push -u -m "WIP login"
git stash list
git stash show -p stash@{0}
git stash apply stash@{0}
git stash pop
git stash pop stash@{1}
git stash drop stash@{0}
git stash clear
```

| Command | Purpose |
|---|---|
| `git stash push -m "message"` | Stash tracked changes with a message |
| `git stash push -u -m "message"` | Stash tracked and untracked files |
| `git stash list` | List saved stashes |
| `git stash show -p stash@{0}` | Display the changes in a stash |
| `git stash apply stash@{0}` | Apply a stash and keep it in the stash list |
| `git stash pop` | Apply the latest stash and remove it if successful |
| `git stash pop stash@{1}` | Apply a specific stash and remove it if successful |
| `git stash drop stash@{0}` | Delete one stash |
| `git stash clear` | Delete all stash entries |

Prefer `git stash push -m "message"` over the older `git stash save "message"` syntax.

**Real-time use case:** Save your feature work before switching to `main` to fix a production issue.

```bash
git stash push -u -m "Feature work in progress"
git switch main
# Fix and commit the urgent issue
git switch feature-login
git stash pop
```

## 15. Clean Untracked Files

```bash
git clean -n
git clean -n -- dev.txt
git clean -f -- dev.txt
git clean -fdn
git clean -fd
```

- `git clean -n` — Preview untracked files that would be deleted.
- `git clean -n -- dev.txt` — Preview the removal of `dev.txt`.
- `git clean -f -- dev.txt` — Force-delete the untracked `dev.txt`.
- `git clean -fdn` — Preview removal of untracked files and directories.
- `git clean -fd` — Force-delete untracked files and directories.

**Warning:** Always preview with `-n` before using `-f`. Git clean does not remove tracked files.

## 16. Ignore Files

Create a `.gitignore` file in the repository.

Example:

```gitignore
.env
*.log
node_modules/
target/
```

- `.env` — Ignore a local environment file.
- `*.log` — Ignore log files.
- `node_modules/` — Ignore Node.js dependencies.
- `target/` — Ignore a common Maven build-output directory.

**Note:** Adding an already-tracked file to `.gitignore` does not stop tracking it automatically.

## 17. Additional Useful Commands

```bash
git ls-files
git branch --merged
git remote show origin
git merge --abort
git rebase --abort
git reflog
git blame <file>
git status --ignored
git fetch origin main
```

- `git ls-files` — List tracked files.
- `git branch --merged` — List branches merged into the current branch.
- `git remote show origin` — Display remote branch and tracking information.
- `git merge --abort` — Attempt to cancel a merge in progress.
- `git rebase --abort` — Cancel a rebase in progress.
- `git reflog` — Inspect recent local `HEAD` movements; useful for recovery.
- `git blame <file>` — Show who last changed each line.
- `git status --ignored` — Include ignored files in status output.
- `git fetch origin main` — Fetch updates from the remote `main` branch.

## 18. Common Real-Time GitHub Workflow

```bash
git status
git switch main
git pull --ff-only origin main
git switch -c feature-login

# Make changes
git add .
git diff --staged
git commit -m "Add login feature"
git push -u origin feature-login
```

Open a Pull Request on GitHub to review and merge the feature into `main`, following your team's workflow.

### If Push Is Rejected Because the Branch Is Behind

First inspect the repository:

```bash
git status
git fetch origin
git log --oneline --graph --all
```

If your current branch is intended to update remote `main` and your team's workflow permits rebasing:

```bash
git pull --rebase origin main
git push origin main
```

Resolve any conflicts before continuing. If the histories are unrelated or local changes would be overwritten, handle those conditions first rather than forcing a push.

## 19. Git Best Practices

1. Commit small, logical units of completed work. Do not wait until the entire project is finished.
2. Test your code before committing.
3. Use clear commit messages and include a JIRA ticket ID when required by your team.
4. Review `git status` and `git diff --staged` before committing.
5. Fetch or pull remote updates regularly and resolve conflicts carefully.
6. Review changes before merging; never merge blindly.
7. Use feature branches and Pull Requests for team projects.
8. Delete obsolete branches only after confirming they are merged and no longer needed.
9. Delete stashes only after confirming the saved work is unnecessary.
10. Never run destructive commands without understanding their effects.
11. Never commit passwords, access keys, tokens, or private SSH keys.
12. Use `.gitignore` for generated files and local settings.
13. Avoid rewriting shared history unless your team agrees.
14. Follow branch protection rules and maintain appropriate backups.

## 20. Must-Practice Commands

```bash
git status
git add .
git commit -m "message"
git log --oneline
git diff
git diff --staged
git branch
git switch -c feature
git merge feature
git remote -v
git fetch origin
git pull origin main
git push -u origin main
git restore
git reset
git revert
git stash
git rebase
git cherry-pick
```

**Learning order:** Basic workflow → commits and history → branches and merges → GitHub remotes → SSH → undo and recovery → stash → rebase and cherry-pick → tags and real-time workflows.
