<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Normal Workflow Examples

Now that we have our repository,
let's go through some normal workflow examples
of a single person developing.

<!-- SIDEBAR
---

#### Normal Workflow Screencast

The second screencast we've provided is *Normal Workflow in Git*,
which demonstrates how to setup your `.gitignore` file,
how to use and interpret `git status` output,
how to add and remove files from your index,
how to commit and what git does in the object database
during these operations.

movie. c2-normal-workflow.mov

---
SIDEBAR -->

### Ignoring

First off,
we will often want Git to automatically ignore certain files -
often ones that are automatically generated during our development.
For example,
in Rails development we often want to ignore the log files,
the production specific configuration files,
etc.
To do this,
we can add patterns into the `.gitignore` file
to tell Git that we don't want it to track them.

Here is an example `.gitignore` file.

```
tmp/*
log/*
config/database.yml
config/environments/production.rb
```

- [.gitignore](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/gitignore.html)

### Adding and Committing

Now we'll do some development and periodically commit our changes.
We have a few options here -
we can commit individual files
or we can tell the *commit* command to automatically add all modified files
in our working directory to the index,
then commit it.

A good way to find out what you're about to commit
(that is,
what is in your index)
is to use the `status` command.

```shell
$ git status
# On branch master
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#
#       modified:   README
#       modified:   Rakefile
#       modified:   lib/simplegit.rb
#
no changes added to commit (use "git add" and/or "git commit -a")
```

In this example,
I can see that I've modified three files in my working directory,
but none of them have been added to the index yet -
they are not staged and ready to be committed.
If I want to make these changes in two separate commits,
or I have completed work on some of them and would like to push that out,
I can specify which files to add individually and then commit.

```shell
$ git add Rakefile
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   Rakefile
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#
#       modified:   README
#       modified:   lib/simplegit.rb
#
```

You can see that if we commit at this point,
only the Rakefile will show up as changed in the commit.

![](../artwork/diagrams/add-commit.svg)

If we want to commit all our changes,
we can use this shorthand,
which will automatically run a `git add` on every modified file to our index,
then commit the whole thing:

```shell
git commit -a -m 'committing all changes'
```

![](../artwork/diagrams/commit-a.svg)

If you would like to give a more useful commit message,
you can leave out the `-m` option.
That will fire up your `$EDITOR` to add your commit message.

> **NOTE** \
Give special care to the first line of your commit message -
it will often be the only thing people see
when they are looking through your commit history.

Now we can continue this loop - modifying,
adding and committing - during our development.

- [git status](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-status.html)

### Interactive Adding

Although that will work for all of your development needs -
many developers simply use `-a` nearly every time they commit
to just automatically add everything to the index -
there is another way of adding files
that makes for a more controlled and thematic set of commits.
This is called *interactive* adding,
and it is a very powerful tool to controlling what goes into each commit.

<!-- SIDEBAR
---

#### Interactive Add Screencast

The next screencast is `Interactive Add in Git`,
which demonstrates how to use the `git add --interactive` command.
It covers all of the major features of interactive adding,
including `status`,
`update`,
`revert`,
`add untracked`,
`patch` and `diff`.

movie. c3-add-interactive.mov

---
SIDEBAR -->

Let's say we add a new function to our `lib/simplegit.rb` file,
add a new task to our `Rakefile`
and then add a new `TODO` file to our project.
Later we come back and want to commit,
but we don't remember which files had to do with each other
and we don't just want to commit them all together
because that's confusing for collaborators trying to review our code.
Interactive mode lets us modify our index interactively before committing.
To fire it up,
type `git add -i`:

```shell
$ git add -i
           staged     unstaged path
  1:    unchanged        +5/-0 Rakefile
  2:    unchanged        +4/-0 lib/simplegit.rb

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```

We can see that we have two files that are being tracked
(have been added at some point in the past)
that have been modified.
We cannot yet see our new TODO file,
though.
To add that,
type `4` for the `add untracked` option and hit `Enter`.

```shell
What now> 4
  1: TODO
Add untracked>> 1
* 1: TODO
Add untracked>>
added one path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```

You will see all the untracked files in your working directory.
Type the numbers of the files you want to add,
or a range (i.e.: `1-5`),
and hit enter twice when you're done.
This will drop you back to the main menu.
You can then type `1` to see what your index looks like now.

```shell
What now> 1
           staged     unstaged path
  1:    unchanged        +5/-0 Rakefile
  2:        +5/-0      nothing TODO
  3:    unchanged        +4/-0 lib/simplegit.rb
```

You can see that the TODO file is now staged (in the index),
but the other two are not.
Let's add the Rakefile,
but not the `lib/simplegit.rb` file and commit it.
To do that,
we hit `2`,
which lists the files we can update,
type `1` and enter to add the `Rakefile`,
then hit enter again to go back to the main menu.
Then we hit `7` to exit and run the `git commit` command

```shell
What now> 2
           staged     unstaged path
  1:    unchanged        +5/-0 Rakefile
  2:    unchanged        +4/-0 lib/simplegit.rb
Update>> 1
           staged     unstaged path
* 1:    unchanged        +5/-0 Rakefile
  2:    unchanged        +4/-0 lib/simplegit.rb
Update>>
updated one path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 7
Bye.

$ git commit -m 'rakefile and todo file added'
Created commit 4b0780c: rakefile and todo file added
 2 files changed, 9 insertions(+), 0 deletions(-)
 create mode 100644 TODO
```

The interactive shell is pretty simple and very powerful -
playing with it instead of running `git add` commands directly
may help in understanding what's happening,
since you can see the status of your files in the index
versus the working directory more clearly.
It helps visualize that what is in your index
(the `staged` column)
is what will be committed when you run `git commit`.

You can also do more complicated things,
like going through all of your change patches hunk by hunk,
deciding if each hunk should be applied to the next commit or not.
This means that if you've made a bunch of changes to one file,
you can commit *part* of those changes in one commit,
and the rest in a second.
Try the `patch` menu option in the *Interactive Adding* menu
to try this out.

> **NOTE** \
Beware of using interactive adding
if you are already used to running `git commit -a`.
If you run through the whole interactive add process
and then run `git commit -a`,
it will basically ignore everything you just did
and just add all modified files.

#### Removing

For removing files from your tree,
you can simply run:

```shell
git rm <filename>
```

This will remove that file from the index
(and thus from the next commit)
as well as from your working directory.
On your next commit,
the tree that commit points to
will simply not contain that file anymore.
