# Contributing to SO-exam

First of all, welcome to SO-exam!
I'm happy that you are interested in contributing to this project.
New features, bug fixes, improvements, maintenance and everything in between as contributions are welcomed!

This project is hosted on GitHub, and I encourage you to follow these guidelines before contributing to the Q&A sections!

## Fork the project

Ideally, for less merge conflicts and better integration I advise core members and future contributors to **fork** this project and work on their mirror.

## Work on your own branch, checked out from master

```console
$ git checkout -b <your branch name>
```

**Note**: It is recommended to have your branch name something along the lines:

```console
<your first name + first initial of last name/wip>
<your first name + first initial of last name/name-of-feature-you-re-working-on>
```

For example, for the username `mariasfiraiala`, the working branch would be `marias/wip`.

## Have an ongoing PR with your work from the branch you've just created

**Note**: Either mark it as draft or keep it as default.
Someone will be assigned by our team to look over your PR.

## Commit your changes with meaningful messages

Must have: no more than 50 characters, all verbs at Present Tense!

Also, sign-off your patches:

```console
$ git add .
$ git commit -sm "Add initial CONTRIBUTING.md file"
$ git push -u origin <your branch name>
```

**Note**: If your changes are somewhat harder to understand, add a description to your commit:

```console
$ git commit -sm "Add initial CONTRIBUTING.md file"
$ git commit --amend
```

No more that 72 characters per line are to be accepted for a commit description!

## Add a commit only if you're introducing a new feature or change

If you need to correct something from a previous commit, add your changes then rebase the new commit to the old one:

```console
$ git add .
$ git commit -sm "Fix CONTRIBUTING.md file"
$ git rebase -i HEAD~2

---> your editor will show you this message,
---> fixup, added in front of the second commit will merge it into the previous one without keeping the message

pick 2b34ced Add initial CONTRIBUTING.md file
f edce107 Fix CONTRIBUTING.md file

# Rebase 059e7ae..edce107 onto 059e7ae (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out

---> save and exit your editor

$ git push -f origin <your branch name>
```