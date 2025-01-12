<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Branching

This is the fun part of Git that you'll come to love like a child.
When you first initialize a git repository,
or clone one,
you'll get a 'master' branch by default if you don't specify something else.
This is really just a git suggestion and you don't have to use it -
like just about everything in Git,
it can be overridden.

### Switching Branches

However,
let's say we're working on our project
and we want to add a new function to our library,
so we'll make a new branch called 'newfunc' and switch to it.
There are two ways we can do this,
one is to create the branch and then switch to it:

```shell
git branch newfunc; git checkout newfunc
```

The other way is to checkout a branch that doesn't exist yet
and tell git you want to create it by passing the '-b' flag:

```shell
git checkout -b newfunc
```

Now,
to check which branch we are on,
we just type `git branch`:

```shell
$ git branch
  master
* newfunc
```

We can see we are now on our new branch.
This means that if we modify a file and commit it,
this branch will include that change,
but the 'master' branch will not have it yet.
So,
we add a new method to our library and commit it.

```shell
$  vim lib/simplegit.rb; git commit -a -m 'added lstree function'
Created commit 1a8c32e: added lstree function
 1 files changed, 4 insertions(+), 0 deletions(-)
```

Now we want to change something in the README in the 'master' branch,
but we haven't tested this function yet
so we don't want to merge our new branch in yet.
That's fine,
we just switch back and make the change.

```shell
$ git checkout master

$  vim README         # add some text

$ git commit -a -m 'added more description'
Created commit d6fad7d: added more description
 1 files changed, 1 insertions(+), 1 deletions(-)
```

Now lets see what the differences in our branches are.

```shell
$ git diff --stat master newfunc
 README           |    4 ++--
 lib/simplegit.rb |    4 ++++
 2 files changed, 6 insertions(+), 2 deletions(-)
```

We could also get a patch file for one to apply to the other,
but what we really want to do next is merge the two.

- [git branch](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-branch.html)
- [git checkout](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-checkout.html)
