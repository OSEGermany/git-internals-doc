<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Simple Merging

<!-- SIDEBAR
---

#### Branching and Merging Screencast

In this screencast,
we take you through a workflow where we branch,
stash and merge several times.
It demonstrates the `branch` and `show-branch` commands,
how to switch branches,
how to stash changes,
how to list and apply stashes,
how to resolve conflicts,
how to create and delete topic branches,
and what fast-forward merges are.

movie. c6-branch-merge.mov

---
SIDEBAR -->

So now we want to move the changes in our `newfunc` branch
back into our `master` branch and remove it.
This will require us merging one branch into another.
Since we're already in our `master` branch,
we'll merge in the `newfunc` branch like this:

```shell
git merge newfunc
```

Easy peasy.
We can see that the `simplegit.rb` file now has four new lines
and the `README` file was auto-merged.

Now we can get rid of our 'newfunc' branch with a simple:

```shell
$ git branch -d newfunc
Deleted branch newfunc.
```

### Resolving Conflicts

That was a fairly simple problem,
but what if we branch our code
and then edit the same place in a file in different ways in each branch?
In that case,
we'll get a conflict when we try to merge them back together.
Git is not too aggressive in trying to resolve conflicts,
since you don't want it to make assumptions that are not necessarily correct,
so bugs aren't introduced without your knowledge.

Let's say that we created a `versioning` branch
and then modified the version in the `Rakefile` to different versions
in both the new branch and the `master` branch,
then tried to merge them together.

```shell
$ git merge versioning
Auto-merged Rakefile
CONFLICT (content): Merge conflict in Rakefile
Automatic merge failed; fix conflicts and then commit the result.
```

It tells us that there was a conflict
and so the new commit object was not created.
We will have to merge the conflicted file manually
and then commit it again.
The output tells us the files that had conflicts,
in this case it was the `Rakefile`.

```ruby
spec = Gem::Specification.new do |s|
    s.platform  =   Gem::Platform::RUBY
    s.name      =   "simplegit"
<<<<<<< HEAD:Rakefile
    s.version   =   "0.1.2"
=======
    s.version   =   "0.2.0"
>>>>>>> versioning:Rakefile
    s.author    =   "Scott Chacon"
    s.email     =   "schacon@gmail.com"
    s.summary   =   "A simple gem for using Git in Ruby code."
    s.files     =   FileList['lib/**/*'].to_a
    s.require_path  =   "lib"
end
```

We can see that in the `master` branch,
the version was changed to `0.1.2` and in the `versioning` branch,
the same line was changed to `0.2.0`.
All we have to do is choose which one is correct
and remove the rest of the lines,
like so:

```ruby
spec = Gem::Specification.new do |s|
    s.platform  =   Gem::Platform::RUBY
    s.name      =   "simplegit"
    s.version   =   "0.2.0"
    s.author    =   "Scott Chacon"
    s.email     =   "schacon@gmail.com"
    s.summary   =   "A simple gem for using Git in Ruby code."
    s.files     =   FileList['lib/**/*'].to_a
    s.require_path  =   "lib"
end
```

Now we add and commit that file,
and we're good.

```shell
$ git add Rakefile
$ git commit -m 'fixed conflict'
Created commit 47c668a: fixed conflict
```

### Undoing a Merge

Assume we have gone through some massive merge
because someone on your team hasn't committed in a while,
or you have a branch that was created some time ago
but hasn't been rebasing and you want to pull it in.
So you try to `git merge old_branch` it
and you get conflict after conflict
and it is just too much trouble to deal with
and you just want to undo it all.

This is where `git reset` comes in.
To reset your working directory and index back to what it was
before you tried the merge,
simply run:

```shell
git reset --hard HEAD
```

The `--hard` makes sure both your index file and working directory
are changed to match what it used it be.
By default it will only reset your index,
leaving the partially merged files in your working directory.
If you happen to have worked through it all and committed,
then decided that it was a mistake
because all of your tests break or something,
you can still go back (and throw away that commit) by running:

```shell
git reset --hard ORIG_HEAD
```

This is only helpful if you want to undo the latest change or changes.
If you happen to commit again
then decide that you want to keep the latest commit,
but undo a commit that was added sometime before that,
you'll need to use `git revert`,
which is a bit too dangerous to cover here.

- [git branch](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-branch.html)
- [git merge](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-merge.html)
- [git reset](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-reset.html)
- [git revert](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-revert.html)
