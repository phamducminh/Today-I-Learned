# Git useful command for daily work

1. Discard local file modifications

```bash
git checkout -- <file_path>     # discard specific file
git checkout -- .               # discard all unstaged change
```

2. Undo local commits

```bash
git reset HEAD~1
git reset --hard HEAD~1
```

3. Edit a commit message

```bash
git commit --amend
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

10. Log

```bash
git log             # show log
git log --oneline   # show log in one line
git log -n1         # show log of last commit
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

