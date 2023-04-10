<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2008 Scotty <schacony@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

# Commands Overview

This section is meant to be a really quick reference
to the commands we have reviewed in Git
and a quick description of what they do,
where we have talked about them
and where to find out more information on them.

## Basic Git

### [git config](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-config.html)

Sets configuration values for things like your user name,
email,
and gpg key,
your preferred diff algorithm,
file formats to use,
proxies,
remotes and tons of other stuff.
For a full list,
see the [git-config docs](
https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-config.html)

### [git init](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-init.html)

Initializes a git repository -
creates the initial '.git' directory in a new or existing project.

### [git clone](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-clone.html)

Copies a Git repository from another place
and adds the original location as a remote you can fetch from again
and possibly push to if you have permission.

### [git add](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-add.html)

Adds changes in files in your working directory to your index,
or staging area.

### [git rm](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-rm.html)

Removes files from your index and your working directory
so they will stopped being tracked.

### [git commit](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-commit.html)

Takes all of the changes staged in the index (that have been `git add`ed),
creates a new commit object pointing to it,
and advances the branch to point to that new commit.

### [git status](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-status.html)

Shows you the status of files in your index versus your working directory.
It will list out files that are untracked (only in your working directory),
modified (tracked but not yet updated in your index),
and staged (added to your index and ready for committing).

### [git branch](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-branch.html)

Lists existing branches,
including remote branches if '-a' is provided.
Creates a new branch if a branch name is provided.
Branches can also be created with '-b' option to `git checkout`.

### [git checkout](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-checkout.html)

Checks out a different branch -
makes your working directory look like the tree of the commit
that branch points to
and updates your HEAD to point to this branch now,
so your next commit will modify it.

### [git merge](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-merge.html)

Merges one or more branches into your current branch
and automatically creates a new commit if there are no conflicts.

### [git reset](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-reset.html)

Resets your index and working directory to the state of your last commit,
in the event that something screwed up and you just want to go back.

### [git rebase](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-rebase.html)

An alternative to merge
that rewrites your commit history to move commits since you branched off,
to apply to the current head instead.
A bit dangerous as it discards existing commit objects.

### [git stash](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-stash.html)

Temporarily saves changes that you don't want to commit immediately for later.
Can re-apply the saved changes at any time.

### [git tag](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-tag.html)

Tags a specific commit with a simple,
human readable handle that never moves.

### [git fetch](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-fetch.html)

Fetches all the objects
that a remote version of your repository has
that you do not yet have,
so you can merge them into yours or simply inspect them.

### [git pull](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-pull.html)

Runs a `git fetch` then a `git merge`.

### [git push](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-push.html)

Pushes all the objects that you have
that a remote version does not yet have
to that repository and advances its branches.

### [git remote](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-remote.html)

Lists all the remote versions of your repository,
or can be used to add and delete them.

## Inspecting Repositories

### [git log](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-log.html)

Shows a listing of commits on a branch or involving a specific file
and optionally details about what changed between it and its parents.

### [git show](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-show.html)

Shows information about a git object,
normally used to view commit information.

### [git ls-tree](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-ls-tree.html)

Shows a tree object,
including the mode and name of each node
and the SHA-1 value of the blob or tree that it points to.
Can also be run recursively to see all subtrees as well.

### [git cat-file](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-cat-file.html)

Used to view the type of an object if you only have the SHA-1 value,
or used to redirect contents of files
or view raw information about any object.

### [git grep](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-grep.html)

Lets you search through your trees of content for words and phrases
without having to actually check them out.

### [git diff](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-diff.html)

Generates patch files or statistics of differences between paths or files
in your git repository,
or your index or your working directory.

### [gitk](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/gitk.html)

Graphical Tcl/Tk based interface to a local Git repository.

### [git instaweb](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-instaweb.html)

Wrapper script to quickly run a web server
with an interface into your repository
and automatically directs a web browser to it.

## Extra Tools

### [git archive](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-archive.html)

Creates a tar or zip file
of the contents of a single tree from your repository.
Easiest way to export a snapshot of content from your repository.

### [git gc](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-gc.html)

Garbage collector for your repository.
Packs all your loose objects for space and speed efficiency
and optionally removes unreachable objects as well.
Should be run occasionally on each of your repos.

### [git fsck](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-fsck.html)

Does an integrity check of the Git "filesystem",
identifying dangling pointers and corrupted objects.

### [git prune](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-prune.html)

Removes objects that are no longer pointed to by any object
in any reachable branch.

### [git-daemon](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-daemon.html)

Runs a simple,
unauthenticated wrapper on the git-upload-pack program,
used to provide efficient,
anonymous and unencrypted fetch access to a Git repository.
