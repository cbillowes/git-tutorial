# Welcome to the Git CLI

## Why the Command Line Interface?

- **Exposure**: to how Git works under the hood so you know what you are doing.

- **Faster**: than a typical GUI because you don't have to navigate to find features.
  However, there are interfaces that offer shortcuts that make this statement less applicable.

- **Efficient**: you can chain multiple operations in a single command.

- **Hacker**: it's the hacker way being one with the terminal.

## Basic boring commands

Initialize a Git repository.

```bash
git init
```

```
git remote add origin git@github.com:cbillowes/git-tutorial.git
git branch -M main
git push -u origin main
```

See all file two levels deep in a tree.

```bash
❯ tree -aL 2
.
├── .git <<--------
│   ├── HEAD
│   ├── config
│   ├── description
│   ├── hooks
│   ├── info
│   ├── objects
│   └── refs
└── README.md
```

Create two files to play around with.

```bash
❯ echo README.md
❯ echo Hello World >> hello.txt
```

- Check the status of the repository. Notice no commits have been made.
- Untracked files are files that Git does not yet know about.
- Tracked files let us spy on them to see when things have changed and how they've changed.
- You can add them to Git. Do so one by one or using a pattern.
- It's important to know what you are committing by checking it's diff.
- Make sure files that are sensitive are not committed.
  You can ignore them by adding its name or a pattern to the `.gitignore` file.

```bash
❯ git status
```

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
        hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Check the diff of what has changed. These were newly added files so there is nothing
different to see so the output is empty.

```bash
❯ git diff
```

Let's add `hello.txt`.

```bash
❯ git add hello.txt
```

Notice how the status has two sections now.
Changes to be committed are STAGED changes.
Untracked files are UNSTAGED.
Staging is not yet committed to git.
It's a staging area where you craft your commit.
Add relevant changes only. NOT JUST EVERYTHING you've changed.
If you want to unstage you can use `git rm --cached hello.txt`
and it will go back to the pool of ready to be committed files.

```bash
❯ git status
```

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```

Finalize the commit.

```bash
❯ git add hello.txt
❯ git commit -m "Hello world"
```

```
[main (root-commit) 3abdb13] Hello world
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

See your history.

```bash
❯ git log
```

```
commit 3abdb131eb538052c91aa59f854c4ac2bb5077ef (HEAD -> main)
Author: Clarice Bouwer <cbillowes@gmail.com>
Date:   Fri Jul 7 09:13:58 2023 +0400

    Hello world
```

Change `hello.txt`. Now things get _diff_y.

```bash
❯ echo Hello Galaxy >> hello.txt
❯ git status
```

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

See the diff patch.

```diff
❯ git diff
diff --git a/hello.txt b/hello.txt
index 557db03..ee4d00b 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 Hello World
+Hello Galaxy
```

Add and commit in one go.
Omit the `-m` switch to open your message in your Git editor (Vim, Nano, something else).
This gives you the ability to provide a multi-line message.

First line is a simple subject. Present tense, not ending in full stop.
Next line is empty.
Next lines describe the reasoning - not the what - of the commit and links to resources
should there be any.

```bash
❯ git add hello.txt && git commit -m "Hello galaxy"
```

```
[main 0e9194f] Hello galaxy
 1 file changed, 1 insertion(+)
```

## Configuration

Open `~/.gitconfig`

```
# This is Git's per-user configuration file.
[user]
        name = Arthur Dent
        email = arthur.dent@hitchhikers.com
[alias]
        pp = log --graph --abbrev-commit --decorate --date=relative --all
        st = status --short --branch
        lg = log --pretty='%C(yellow)%h%Creset | %C(yellow)%d%Creset %s %Cgreen(%cr)%Creset %C(cyan)[%an]%Creset' --graph
        dp = diff --word-diff --unified=10
[diff]
        algorithm = histogram
[core]
        autocrlf = input
        editor = vim
[pull]
        rebase = false
[init]
        defaultBranch = "main"
```

### Aliases

```bash
git pp
git st
git lg
git dp
```

## Interesting commands

Running `git diff` now is going to return nothing because nothing has changed.
But you can get the diff between a range of commits.

```bash
git diff 3abdb13..0e9194f
```

```bash
❯ echo Hello Something >> hello.txt
```

Crafting a commit interactively.

```bash
❯ git add -i
```

```
           staged     unstaged path
  1:    unchanged        +1/-0 hello.txt

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 8
status        - show paths with changes
update        - add working tree state to the staged set of changes
revert        - revert staged set of changes back to the HEAD version
patch         - pick hunks and update selectively
diff          - view diff between HEAD and index
add untracked - add contents of untracked files to the staged set of changes
```

## The Cloud

Create a remote repository on the cloud.
This can be your own instance or hosted by GitHub, BitBucket etc.
Origin is the name of the remote you are giving it. It's the default.
And the branch is the name of the branch you are working on - mine is main.
I'm using SSH below, like one should. Good.


```bash
git remote add origin git@github.com:cbillowes/git-cli.git
```

Push your changes to remote.

```bash
git push -u origin main
```

Make more changes to your branch.
Run the following.

```bash
git diff origin/main..HEAD
git log origin/main..HEAD
```

HEAD is the tip of your branch.

## Branching

Create a branch off of HEAD. You will need to switch it yourself.

```bash
git branch awesome
```

```bash
git checkout -b awesome
```

You can switch using `checkout` or `git switch main`.

- Stashing
- Cherry-picking
- Merging and rebasing