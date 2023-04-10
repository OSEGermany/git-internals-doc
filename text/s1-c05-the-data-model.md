<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## The Git Data Model

In computer science speak,
the Git object data is a *directed acyclic graph*.
That is,
starting at any commit you can traverse its parents in one direction
and there is no chain that begins and ends with the same object.

All commit objects point to a tree and optionally to previous commits.
All trees point to one or many blobs and/or trees.
Given this simple model,
we can store and retrieve vast histories of complex trees
of arbitrarily changing content quickly and efficiently.

This section is meant to demonstrate how that model looks.

### References

In addition to the Git objects,
which are immutable - that is,
they cannot ever be changed,
there are references also stored in Git.
Unlike the objects,
references can constantly change.
They are simple pointers to a particular commit,
something like a tag,
but easily moveable.

Examples of references are branches and remotes.
A branch in Git is nothing more than a file in the `.git/refs/heads/` directory
that contains the SHA-1 of the most recent commit of that branch.
To branch that line of development,
all Git does is create a new file in that directory
that points to the same SHA-1.
As you continue to commit,
one of the branches will keep changing to point to the new commit SHA-1s,
while the other one can stay where it was.
If this is confusing,
don't worry.
We'll go over it again later.

### The Model

The basic data model I've been explaining looks something like this:

![](../artwork/vector/DAG_Model.eps)

The) We have three trees,
three blobs and a single commit that points to the top of the tree.
The current branch points to our last commit
and the `HEAD` file points to the branch we're currently on.
This lets Git know which commit will be the parent for the next commit.

Now let's assume that we change the `lib/base/base_include.rb` file and commit again.
At this point,
a new blob is added,
which changes the tree that points to it,
which changes the tree that points to that tree
and so on,
to the top of the entire directory.
Then a new commit object is added
which points to its parent and the new tree,
then the branch reference is moved forward.

Let's also say at this point we tag this commit as a release,
which adds a new tag object.

At this point,
we'll have the following in Git:

![](../artwork/vector/Object_DAG_Tree2.eps)

Notice)

At this point,
let's stop to look at the objects we now have in our repository.
From this,
we can easily recreate any of the three directories we committed
by following the graph from the most recent commit object,
and having Git expand the trees that are pointed to.

For instance,
if we wanted the first tree,
we could look for the parent of the parent of the HEAD,
or the parent of the tag.
If we wanted the second tree,
we could ask for the commit pointed to by the tag,
and so on.

![](../artwork/vector/Object_DAG.eps)

So,)
