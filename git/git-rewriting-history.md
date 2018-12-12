# Rewriting History

## Changing the Last Commit

```bash
$ git commit --amend
```

**_It’s like a very small rebase – don’t amend your last commit if you’ve already pushed it._**

## Changing Multiple Commit Messages

If you want to change the last three commit messages, use:

```bash
git rebase -i HEAD~3
```

Running this command gives you a list of commits in your text editor that looks something like this:

```bash
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

# Rebase 710f0f8..a5f4a0d onto 710f0f8 #
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message # x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom. #
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted. #
# Note that empty commits are commented out
```

It’s important to note that these commits are listed in the opposite order than you normally see them using the log command. If you run a log, you see something like this:

```bash
$ git log --pretty=format:"%h %s" HEAD~3..HEAD 
5f4a0d added cat-file
310154e updated README formatting and added blame 
f7f3f6d changed my name a bit
```

### Stop for amending

If you want to stop at the commit you want to edit, change the word `‘pick’` to the word `‘edit’` for each of the commits you want the script to stop after

```bash
edit f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

When you save and exit the editor, Git rewinds you back to the last commit in that list and drops you on the command line with the following message:

```bash
$ git rebase -i HEAD~3
Stopped at f7f3f6d... changed my name a bit You can amend the commit now, with
       git commit --amend
Once you’re satisfied with your changes, run
       git rebase --continue
```

If you change pick to edit on more lines, you can repeat these steps for each commit you change to edit. Each time, Git will stop, let you amend the commit, and continue when you’re finished.

### Reordering Commits

To remove the “added cat-file” commit and change the order in which the other two commits are introduced, you can change the rebase script from this

```bash
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

to this:

```bash
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```

### Squashing Commits

Instead of `“pick”` or `“edit”`, you specify `“squash”`, Git applies both that change and the change directly before it and makes you merge the commit messages together. So, if you want to make a single commit from these three commits, you make the script look like this:

```bash
pick f7f3f6d changed my name a bit
squash 310154e updated README formatting and added blame
squash a5f4a0d added cat-file
```

When you save and exit the editor, Git applies all three changes and then puts you back into the editor to merge the three commit messages:

```bash
# This is a combination of 3 commits.
# The first commit's message is: 
changed my name a bit

# This is the 2nd commit message: 
updated README formatting and added blame 

# This is the 3rd commit message:
added cat-file
```

When you save that, you have a single commit that introduces the changes of all three previous commits.

### Splitting a Commit

Suppose you want to split the middle commit of your three commits. Instead of “updated RE- ADME formatting and added blame”, you want to split it into two commits: “up- dated README formatting” for the first, and “added blame” for the second. Change to this:

```bash
pick f7f3f6d changed my name a bit
edit 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

Then

```bash
$ git reset HEAD^
$ git add README
$ git commit -m 'updated README formatting'
$ git add lib/simplegit.rb
$ git commit -m 'added blame'
$ git rebase --continue
```

And your history looks like this:

```bash
$ git log -4 --pretty=format:"%h %s" 
1c002dd added cat-file
9b29157 added blame
35cfb2b updated README formatting
f3cc40e changed my name a bit
```

HAVE FUN :)





