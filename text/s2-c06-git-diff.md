<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Git Diff

Git has a great diff utility built in
that can give you statistics or a patch file
given any combination of tree objects,
working directory and index.

Two common uses of this
include seeing what you've worked on but not committed yet,
and creating a patch file to send to someone over email
(though there is a much preferred way to share changes
which we will learn about in the "distributed workflow" section a bit later).

### What has changed?

If you simply run `git diff` with no arguments,
it will show you the differences between your current working directory
and your index,
that is,
the last time you ran `git add` on your files.

For example,
if I add my email to the README file and run it,
I will see this:

```shell
$ git diff
diff --git a/README b/README
index 569b350..26c6ac8 100644
--- a/README
+++ b/README
@@ -5,4 +5,4 @@ This library calls git commands and returns the output.

 It is an example for the Git Peepcode book.

-Author : Scott Chacon
+Author : Scott Chacon (schacon@gmail.com)
```

You can also use `git diff` to show you some spiffy stats for a diff,
rather than a patch file,
if you want to see a wider overview of what changed,
then drill down into specific files later.
Here are some examples getting stats,
the first for the differences between two commits
and the second a summary between a commit and the current `HEAD`.

```shell
$ git diff --numstat a11bef06a3f65..cf25cc3bfb0
3       1       README
1       1       Rakefile
4       5       lib/simplegit.rb

$ git diff --stat 0576fac35..
 README           |    4 +++-
 Rakefile         |    2 +-
 lib/simplegit.rb |    4 ++++
 3 files changed, 8 insertions(+), 2 deletions(-)
```

If you want to see what the specific difference is in one of those files,
you can just add a path limiter to the diff command.

```shell
$ git diff a11bef06a3f65..cf25cc3bfb0 -- Rakefile
diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gmail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."
```

You can use this command to detect changes between your index and any tree,
or your working directory and any tree,
your working directory and your index,
etc.

### Generating Patch Files

The default output of the `git diff` command is a valid patch file.
If you pipe the output into a file and email it to someone,
they can apply it with the 'patch' command.
If you've done some work off of a project in an 'experiment' branch,
you could create a patch file this way:

```shell
git diff master..experiment > experiment.patch
```

You can then email that file to anyone,
who could apply it with the '-p1' argument:

```shell
$ patch -p1 < ~/experiment.patch
patching file lib/simplegit.rb
```

- [git diff](https://www.kernel.org/pub/software/scm/git/docs/git-diff.html)
