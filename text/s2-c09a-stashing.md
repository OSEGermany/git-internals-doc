<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Stashing

Stashing is a pretty simple concept
that is incredibly useful and very easy to use.
If you are working on your code
and you need to switch to another branch for some reason
and don't want to commit your current state
because it is only partially completed,
you can run `git stash`,
which will basically take the changes from your last commit
to the current state of your working directory
and store it temporarily.

In the following example,
I have a change to my 'lib/simplegit.rb' file,
but it's not complete.

```shell
$ git status
# On branch master
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#
#       modified:   lib/simplegit.rb
#
no changes added to commit (use "git add" and/or "git commit -a")

$ git stash
Saved working directory and index state "WIP on master: c110d7f... made the ls-tree function recursive and list trees"
(To restore them type "git stash apply")
HEAD is now at c110d7f made the ls-tree function recursive and list trees

$ git status
# On branch master
nothing to commit (working directory clean)
```

Now I can see that my working directory is clean,
as if I had committed,
but I did not.
Now I can switch branches,
work for a while somewhere else,
then switch back.
So where did that change go? How do I get it back? Well,
I can see my stashes by running `git stash list`.

```shell
$ git stash list
stash@{0}: WIP on experiment: 89e6d12... trying git archive
stash@{1}: WIP on master: c110d7f... made the ls-tree function recursive
stash@{2}: WIP on master: c110d7f... made the ls-tree function recursive
```

I see I have two stashes on the 'master' branch,
both saved off of working from the same commit,
and I have one stashed change off the 'experiment' branch.
However,
I can't remember which stash was the one I want,
so I can use `git stash show` to figure it out.

```shell
$ git stash show stash@{1}
 lib/simplegit.rb |    4 ++++
 1 files changed, 4 insertions(+), 0 deletions(-)

$ git stash show stash@{2}
 lib/simplegit.rb |    8 ++++++++
 1 files changed, 8 insertions(+), 0 deletions(-)
```

I can also use any normal git tools that will take a tree on it,
for instance,
`git diff`:

```shell
$ git diff stash@{1}
diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index e939f77..b03bc9c 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -21,10 +21,6 @@ class SimpleGit
    command("git ls-tree -r -t #{treeish}")
  end

-  def commit(message = 'commit message')
-    command("git commit -m #{message}")
-  end
-
  private

    def command(git_cmd)
```

And finally,
I can apply it:

```shell
$ git stash apply stash@{1}
# On branch master
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#
#       modified:   lib/simplegit.rb
#
no changes added to commit (use "git add" and/or "git commit -a")
```

Now we can see that our working directory is back to where it was,
with one file in an unstaged state.
Now I would have to `git add` and `git commit` it
if I wanted to keep the change.

Normally it's not even this complicated.
If you run `git stash apply` without the actual stash reference,
it will just apply the last stash you saved on that branch.
Normally I will just use `git stash` to save something,
go work elsewhere,
then come back and run `git stash apply` to get back to where I was.

- [git stash](http://www.kernel.org/pub/software/scm/git/docs/git-stash.html)
