<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2013 Pascal Borreli <pascal@borreli.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Rebasing

<!-- SIDEBAR
---

#### Rebasing Screencast

This screencast follows roughly the same course as the previous one
on branching and merging,
only we replace merging with rebasing.
This screencast also demonstrates the interactive rebase command
`git rebase -i`.
We also demonstrate some slightly more complex branching,
by using both interactive and normal rebasing techniques simultaneously
on separate branches,
then choosing one and deleting the other.

movie. c7-rebase.mov

---
SIDEBAR -->

To review,
rebasing is an alternative to merging
that takes all the changes you've done since you branched off
and applies those changes as patches
to where the branch you are rebasing to
is now,
abandoning your original commit objects.
For clean merges,
this is a relatively simple process.
Say we have been working in a branch called 'story84'
and it's completed and we want to merge it into the master branch.

![](../artwork/bitmap/repo1.png)

If we do a simple merge,
our history will look like this:

![](../artwork/bitmap/repo.png)

But we don't want to mess up our history
with a bunch of branches and merges when it can be clearer.
Instead of running `git merge story84` from the master branch,
we can stay in the 'story84' branch and run `git rebase master`

```shell
$ git rebase master
First, rewinding head to replay your work on top of it...
HEAD is now at 2c0d4d7... added limit to log function

Applying -added todo options

Adds trailing whitespace.
.dotest/patch:12:* add
warning: 1 line adds whitespace errors.
Wrote tree 2d0bd54dc9e4c398769cdcb59256ca03bb482ccb
Committed: b669c78acffaafd5ba34449e7faf88217394864a

Applying limiting log to 30

error: patch failed: lib/simplegit.rb:14
error: lib/simplegit.rb: patch does not apply
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merged lib/simplegit.rb
CONFLICT (content): Merge conflict in lib/simplegit.rb
Failed to merge in the changes.
Patch failed at 0002.

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To restore the original branch and stop rebasing run "git rebase --abort".
```

Many times this goes very smoothly
and you can see all the new commits and trees written in place of the old ones.
In this case,
I had edited the 'lib/simplegit.rb' file differently in each branch,
which caused a conflict.
I will have to resolve this conflict before I can continue the rebase.

This gives us some options,
since the rebase can do this at any point -
say you have 8 commits to move onto the new branch -
each one could cause a conflict
and you will have to resolve them each manually.
The 'rebase' command will stop at each patch if it needs to
and let you do this.

You have three things you can do here,
you can either fix the file,
run a `git add` on it and then run a `git rebase --continue`,
which will move on to the next patch until it's done.
Our second option is to run `git rebase --abort`,
which will reset us to what our repo looked like
before we tried the rebase.
Or,
we can run `git rebase --skip`,
which will leave this one patch out,
abandoning the change forever.

> **NOTE** \
Git rebase options for a conflict:
`--continue` : tries to keep going once you've resolved it,
`--abort` : gives up altogether and returns to the state before the rebase,
`--skip` : skips this patch,
abandoning it forever

In this case we will simply fix the conflict,
run `git add` on the file and then run `git rebase --continue`
which then makes our history look like this:

![](../artwork/bitmap/repo3.png)

Then all we have to do is switch to the master branch
and merge in 'story84'
(which is called a 'fast-forward',
since 'master' is now a direct ancestor of 'story84')
to get this:

![](../artwork/bitmap/repo4.png)

### Interactive Rebasing

Much like Git provides a nicer way to work with your index
before committing with `git add --interactive`,
there is an interactive rebasing option
that can only be fairly described as the "bee's knees".

Assume we have started working on a story
to add the `git add` functionality to our library
and so we've started a new branch called 'story92' and done the work there.
Then we decide that the 'ls-tree' function needs to be recursive
and make that change,
then we tweak the library again,
committing each time.
Meanwhile,
we've pulled in a change
that implements the same 'ls-tree' change differently
into our 'master' branch.

![](../artwork/bitmap/repo-rebasei1.png)

We can see before we try the merge
that the same change is in each branch,
and I can see that the master branch version is better,
so I don't even really want to merge it,
I just want to throw my change away.
Also,
I don't really need the other two commits to be two commits,
because the second one is just a tweak
and should be included in the first one.
Lets use `git rebase -i` to rebase this branch
and make those changes.
When we run the command,
our editor comes up,
showing this:

```
# Rebasing c110d7f..c4f10f2 onto c110d7f
#
# Commands:
#  pick = use commit
#  edit = use commit, but stop for amending
#  squash = use commit, but meld into previous commit
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
pick 2b6ae91 added git.add() function
pick bdfd292 made ls-tree recursive
pick c4f10f2 added verbose option to add()
```

Now we can see all of the commits that we are going to rebase.
If we remove the 'made ls-tree recursive' line,
it effectively ditches that commit
so we'll avoid a conflict and not have to worry about it.
Changing the action on the last line to 'squash',
tells git to just make this and the previous commit
into a single commit.
So if we exit the editor with this as the new text:

```
pick 2b6ae91 added git.add() function
squash c4f10f2 added verbose option to add() function
```

Then git sees we have squashed two commits
and wants us to pick a commit message for it,
giving us the commit messages of both,
for us to create a new one for.

```
# This is a combination of two commits.
# The first commit's message is:

added git.add() function

# This is the 2nd commit message:

added verbose option to add() function

# Please enter the commit message for your changes.
# (Comment lines starting with '#' will not be included)
# Not currently on any branch.
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   lib/simplegit.rb
#
```

So we stick with the first message,
save and exit the editor.

```shell
$ git rebase -i master
Created commit 8341085: added git.add() function
 1 files changed, 4 insertions(+), 0 deletions(-)
Successfully rebased and updated refs/heads/story92.
```

Now we've rebased
and instead of three commits on top of our master
and having to reconcile a useless conflict,
we've just added a single commit with no resolving necessary:

![](../artwork/bitmap/repo-rebasei2.png)

The rebase command is one of the most useful and unique
in the git workflow.
To learn more about some spiffy things you can do with it,
check out the [History Manipulation] and [Advanced Merging] sections.

- [git rebase](http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html)
- [git reset](http://www.kernel.org/pub/software/scm/git/docs/git-reset.html)
