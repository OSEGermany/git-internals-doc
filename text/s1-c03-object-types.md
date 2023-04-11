<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2023 Robin Vobruba <hoijui.quaero@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Git Object Types

Git *objects* are the actual data of Git,
the main thing that the repository is made up of.
There are four main object types in Git,
the first three being the most important
to really understand the main functions of Git.

All of these types of objects are stored in the Git **Object Database**,
which is kept in the **Git Directory**.
Each object is compressed (with Zlib)
and referenced by the SHA-1 value of its contents plus a small header.
In the examples,
I will use the first 6 characters of the SHA-1 for simplicity,
but the actual value is 40 characters long.

> **NOTE** \
SHA stands for Secure Hash Algorithm.
A SHA creates an identifier of fixed length
that uniquely identifies a specific piece of content.
SHA-1 succeeded SHA-0 and is the most commonly used algorithm.
[Wikipedia](https://en.wikipedia.org/wiki/SHA1) has more on the topic.

To demonstrate these examples,
we will develop a small ruby library
that provides very simple bindings to Git,
keeping the project in a Git repository.
The basic layout of the project is this:

![Sample project with files and directories](../artwork/diagrams/layout.eps)

Let's take a look at what Git does when this is committed to a repository.

### The Blob

In Git,
the contents of files are stored as **blobs**.

![Files are stored as blobs](../artwork/diagrams/blobs.eps)

It is important to note that it is the *contents* that are stored,
not the files.
The names and modes of the files are not stored with the blob,
just the contents.

This means that if you have two files anywhere in your project
that are exactly the same,
even if they have different names,
Git will only store the blob once.
This also means that during repository transfers,
such as clones or fetches,
Git will only transfer the blob once,
then expand it out into multiple files upon checkout.

![The contents of a blob,
uncompressed](../artwork/diagrams/blob-expand.eps)

### The Tree

Directories in Git basically correspond to **trees**.

![Trees are pointers to blobs and other trees](../artwork/diagrams/trees.eps)

A tree is a simple list of trees and blobs that the tree contains,
along with the names and modes of those trees and blobs.
The contents of a tree object file is a list of entries in a compact,
binary format (`git cat-file tree <sha>`),
which simply consists of *mode*,
*type*,
*name* and *sha* for each entry.
You can see it in a clear-text,
human-readable form with `git cat-file -p <sha>`,
which shows *mode*,
*type*,
*sha* and *name* for each entry (same fields,
different order).

![An uncompressed tree](../artwork/diagrams/tree-expand.eps)

### The Commit

So,
now that we can store arbitrary trees of content in Git,
where does the 'history' part of 'tree history storage system' come in?
The answer is the **commit** object.

![A commit references a tree](../artwork/diagrams/commit.eps)

The commit is very simple,
much like the tree.
It simply points to a tree and keeps an *author*,
*committer*,
*message* and any *parent* commits that directly preceded it.

![Uncompressed initial commit](../artwork/diagrams/commit-expand.eps)

Since this was my first commit,
there are no parents.
If I commit a second time,
the commit object will look more like this:

![A commit with a parent](../artwork/diagrams/commit-expand2.eps)

Notice how the *parent* in that commit
is the same SHA-1 value of the last commit we did?
Most times a commit will only have a single parent like that,
but if you merge two branches,
the next commit will point to both of them.

> **NOTE** \
The current record for number of commit parents in the Linux kernel
is 12 branches merged in a single commit!

### The Tag

The final type of object you will find in a Git database is the **tag**.
This is an object that provides a permanent shorthand name
for a particular commit.
It contains an *object*,
*type*,
*tag*,
*tagger* and a *message*.
Normally the *type* is `commit`
and the *object* is the SHA-1 of the commit you're tagging.
The tag can also be GPG signed,
providing cryptographic integrity to a release or version.

![Uncompressed tag](../artwork/diagrams/tag-expand.eps)

We'll talk a little bit more about tags
and how they differ from *branches* (which also point to commits,
but are not stored as objects) in the next section,
where we'll pull all of this together
into how all these objects relate to each other conceptually.
