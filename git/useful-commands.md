# Git useful command for daily work

1. Discard local file modifications

```bash
git checkout -- <file_path>     # discard specific file
git checkout -- .               # discard all unstaged change
git checkout <commit>           # discard unstaged changes since <commit>.
```

2. Undo local commits

```bash
git reset HEAD~1
git reset --hard HEAD~1 # discard staged and unstaged changes since the most recent commit.
```

3. Edit a commit message

```bash
git commit --amend  # add your staged changes to the most recent commit
git commit --amend -m "New message"
```

4. Delete local branch

```bash
git branch -d branchname  # Delete branchname if it is already merged
git branch -D branchname  # Force delete branchname
```

5. Delete remote branch

```bash
git push --delete origin branchname
git push origin :branchname
```

6. Reverting pushed commits

```bash
# reverts the commit with the specified id
git revert c761f5c

# reverts the second to last commit
git revert HEAD^

# reverts a whole range of commits
git revert develop~4..develop~2

# undo the last commit, but don't create a revert commit 
git revert --no-commit HEAD = git revert -n HEAD
```

7. Avoid repeated merge conflicts

```bash
git config --global rerere.enabled true
```

8. Find the commit that broke something after a merge

Tracking down the commit that introduced a bug after a big merge can be quite time consuming. Luckily `git` offers a great binary search facility in the form of `git-bisect`. First you have to perform the initial setup:

```bash
git bisect start         # starts the bisecting session
git bisect bad           # marks the current revision as bad
git bisect good revision # marks the last known good revision
```

After this git will automatically checkout a revision halfway between the known “good” and “bad” versions. You can now run your specs again and mark the commit as “good” or “bad” accordingly.

```bash
git bisect good # or git bisec bad
```

This process continues until you get to the commit that introduced the bug.

9. 

```bash
git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch secrets.txt' \
--prune-empty --tag-name-filter cat -- --all
```

This will remove the file `secrets.txt` from every branch and tag. It will also remove any commits that would be empty as a result of the above operation.

10. Show commit history

```bash
git log                 # show log
git log --oneline       # show log in one line
git log -n1             # show log of last commit
git log -p <file_name>  # show log for a file
```

11. Show configuration list

```bash
git config --list           # see all setting of your git
git config --local --list   # see only local setting
git config --global --list  # see only global setting
```

12. Unset configuration

```bash
git config --unset key
git config --unset --global key
```

13. See all file changes locally

```bash
git diff
git diff <file_name>    # Show changes for only one file
git diff dev            # Displays changes between the working tree and the dev branch
git diff master..dev    # Displays changes between the branches master and dev
```

14. See who changed what and when in file_name

```bash
git blame <file_name>
```

15. Show a log of changes to the local repository’s HEAD

```bash
git reflog
```

---

Update 02/10/2020

16. Delete all local branches that have been merged into master or develop

```bash
[alias]
  cu = !git branch --merged | egrep -v \"(^\\*|master|develop)\" | xargs git branch -d
```

Then type "git cu" (Git Clean Up)

17. Add local file modifications:

```bash
git add <file/folder>     # Add file/folder to staging area
git add . or git add -A   # Add all files/folders in all the folders of the repository
git add . -u              # Add only tracked files/folders
```

18. Branching

```bash
git branch                    # List all local branches
git branch -r                 # List all remote branches
git branch -a                 # List all local/remote branches
```

```bash
git branch <branch_name>      # Create new branch with name <branch_name> based off of current branch
git branch -m <old_name> <new_name> # Rename branch from <old_name> to <new_name>
git branch -m <new_name>      # Rename the current branch to <new_name>
```

```bash
git checkout <branch_name>    # Switch to <branch_name>
git checkout -b <branch_name> # Create new <branch_name> and switch to it
```

---

Update 02/19/2020

19. Clone git repository

```bash
git clone <repo_url> <to_directory>         # Clone repo into to specific directory
git clone <repo_url> --branch <branch_name> # Clone specific branch
git clone -depth=<depth_level> <repo_url>   # Clone a certain level of history’s depth
```

20. Working with remote

```bash
# List remote connections with URL
git remote -v

# Add another remote connection
git remote add <new_remote_name> <new_remote_url>

# Rename connection
git remote rename <remote_old_name> <remote_new_name>

# Delete connection
git remote rm <remote_name>
git remote remove <remote_name>
```

21. Dơwnload the latest code base with fetching

```bash
# Fetch all remote repositories
git fetch --all

# Fetch <remote_name> repository, usually origins
git fetch <remote_name>

# Fetch a branch from a remote repository
git fetch <remote_name> <branch_name>
```

