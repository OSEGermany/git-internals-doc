<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2008 Scotty <schacony@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

# Using Git

Now that you *hopefully* understand
what Git is designed to do at a fundamental level -
how it tracks and stores content,
how it stores branches and merges and tracks remote copies of the repository,
let's see how to actually use it.
This next section presents some of the basic commands
that you will need to know in order to use Git effectively.

At the end of each chapter,
there will be a link to the official Git documentation
for each of the commands used in that section,
in case you want to learn more or see all the options for that command.

## Setting Up Your Profile

<!-- SIDEBAR
---

#### Setup, Init and Cloning Screencast

I have produced a series of short screencasts
demonstrating the topics of several of the chapters in this book
so you can see them in practice on a real command line.
You should be able to download these movies
with this book from the PeepCode website.

The first in this series is *Git Setup, Initialization and Cloning*
and shows you how to setup your Git configuration,
how to initialize a new repository
and how to clone an existing repository
over both the Git transport and the HTTP transport.

movie. c1-init.mov

---
SIDEBAR -->

For every commit you do,
Git will try to associate a name and email address.
One of the first things you'll want to do in Git is to set these values.
You can set them as global configuration values
with the `git config` command:

```shell
git config --global user.name "Scott Chacon"
git config --global user.email "schacon@gmail.com"
```

This will create a new `~/.gitconfig` file that will look like this:

```shell
$ cat ~/.gitconfig
[user]
        name = Scott Chacon
        email = schacon@gmail.com
```

You can change those variables at any time,
either by editing that file,
or running the `git config` commands again.

If you want to set different values for a specific project,
just leave out the `--global`
and it will write the same snippet into your `.git/config` file
in that repository,
which will overwrite your global values.

- [git config](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-config.html)
