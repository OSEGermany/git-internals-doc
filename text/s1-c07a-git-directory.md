<!--
SPDX-FileCopyrightText: 2008 - 2010 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## The Git Directory

When you initialize a Git repository, either by cloning an existing one or creating a new one, the first thing Git does is create a Git directory. This is the directory that stores all the object data, tags, branches, hooks and more. Everything that Git permanently stores goes in this single directory. When you clone someone else's repository, it basically just copies the contents of this directory to your computer. Without a checkout (called a *working directory*) this is called a *bare* Git repo and moving it to another computer backs up your entire project history. It is the soul of Git.

When you run `git init` to initialize your repository, the Git directory is by default installed in the directory you are currently in as `.git`. This can be overridden with the *GIT_DIR* environment variable at any time. In fact, the Git directory does not need to be in your source tree at all. It's perfectly acceptable to keep all your Git directories in a central place (ex: `/opt/git/myproject.git`) and just make sure to set the GIT_DIR variable when you switch projects you are working on (ex: `/home/username/projects/myproject`).

The Git directory for our little project looks something like this:

```
.
|-- HEAD
|-- config
|-- hooks
|   |-- post-commit
|   `-- pre-commit
|-- index
|-- info
|   `-- exclude
|-- objects
|   |-- 05
|   |   `-- 76fac355dd17e39fd2671b010e36299f713b4d
|   |-- 0c
|   |   `-- 819c497e4eca8e08422e61adec781cc91d125d
|   |-- 1a
|   |   `-- 738da87a85f2b1c49c1421041cf41d1d90d434
|   |-- 47
|   |   `-- c6340d6459e05787f644c2447d2595f5d3a54b
|   |-- 99
|   |   `-- f1a6d12cb4b6f19c8655fca46c3ecf317074e0
|   |-- a0
|   |   `-- a60ae62dd2244a68d78151331067c5fb5d6b3e
|   |-- a1
|   |   `-- 1bef06a3f659402fe7563abf99ad00de2209e6
|   |-- a8
|   |   `-- 74b732e12a5c04b5a73d7f1123c249997b0b2d
|   |-- a9
|   |   `-- 06cb2a4a904a152e80877d4088654daad0c859
|   |-- e1
|   |   `-- b3ececb0cbaf2320ca3eebb8aa2beb1bb45c66
|   |-- fe
|   |   `-- 897108953cc224f417551031beacc396b11fb0
|   |-- info
|   `-- pack
`-- refs
    |-- heads
    |   `-- master
    `-- tags
        `-- v0.1
```

For more in-depth information on the Git directory layout, see the [git repository layout docs.](http://www.kernel.org/pub/software/scm/git/docs/gitrepository-layout.html).

For now, let's go over some of the more important contents of this directory.

### .git/config

This is the main Git configuration file. It keeps your project specific Git options, such as your remotes, push configurations, tracking branches and more. Your configuration will be loaded first from this file, then from a `~/.gitconfig` file and then from an `/etc/gitconfig` file, if they exist.

Here is an example of what a config file might look like:

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@github.com:username/myproject.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```

See the [git-config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html) docs for more information on available configuration options.

### .git/index

This is the default location of the `index` file for your Git project. This location can be overridden with the *GIT_INDEX* environment variable, which is sometimes useful for temporary tree operations. See the chapter on the "Git Index" for more information on what this file is used for.

### .git/objects/

This is the main directory that holds the data of your Git objects - that is, all the contents of the files you have ever checked in, plus your commit, tree and tag objects.

The files are stored by their SHA-1 values. The first two characters make up the subdirectory and the last 38 is the filename. For example, if the SHA-1 for a blob we've checked in was

```
a576fac355dd17e39fd2671b010e36299f713b4d
```

the file we would find the Zlib compressed contents in is:

```
[GIT_DIR]/objects/a5/76fac355dd17e39fd2671b010e36299f713b4d
```

### .git/refs/

This directory normally has three subdirectories in it - *heads*, *remotes* and *tags*. Each of these directories will hold files that correspond to your local branches, remote branches and tags, respectively.

For example, if you create a `development` branch, the file `.git/refs/heads/development` will be created and will contain the SHA-1 of the commit that is the latest commit of that branch.

### .git/HEAD

This file holds a reference to the branch you are currently on. This basically tells Git what to use as the parent of your next commit. The contents of it will generally look like this:

```
ref: refs/heads/master
```

### .git/hooks

This directory contains shell scripts that are invoked after the Git commands they are named after. For example, after you run a commit, Git will try to execute the `post-commit` script, if it has executable permissions.

See the [online hooks documentation](http://www.kernel.org/pub/software/scm/git/docs/githooks.html) for more information on what you can do with hooks.
