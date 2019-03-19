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
git branch -d branchname
git branch -D branchname
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
```

14. See who changed what and when in file_name

```bash
git blame <file_name>
```

15. Show a log of changes to the local repository’s HEAD

```bash
git reflog
```

