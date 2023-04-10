<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Browsing Git

Git also gives you access to a number of lower level tools
that can be used to browse the repository,
inspect the status and contents of any of the objects,
and are generally helpful for inspection and debugging.

<!-- SIDEBAR
---

#### Git Object Browsing Screencast

In this screencast,
we show how to browse and inspect raw Git objects.
The major tools covered are the `git cat-file`
and `git ls-tree` commands to inspect the object contents,
and then we cover some of the included graphical browsers,
`gitk` and `gitweb`.

movie. c5-git-browsing.mov

---
SIDEBAR -->

### Showing Objects

The `git show` command is really useful
for presenting any of the objects in a very human readable format.
Running this command on a file will simply output the contents of the file.
Running it on a tree will just give you the filenames
of the contents of that tree,
but none of its subtrees.
Where it's most useful is using it to look at commits.

#### Showing Commits

If you call it on a tree-ish that is a commit object,
you will get simple information about the commit
(the author,
message,
date,
etc.)
and a diff of what changed between that commit and its parents.

```shell
$ git show master^
commit 0c8a9ec46029a4e92a428cb98c9693f09f69a3ff
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

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

#### Showing Trees

Instead of the `git show` command,
it's generally more useful to use the lower level `git ls-tree` command to view trees,
because it gives you the SHA-1s of all the blobs and trees that it points to.

```shell
$ git ls-tree master^{tree}
100644 blob 569b350811e7bfcb2cc781956641c3   README
100644 blob 8f94139338f9404f26296befa88755   Rakefile
040000 tree ae850bd698b2b5dfbac1ab5fd95a48   lib
```

You can also run this command recursively,
so you can see all the subtrees as well.
This is a great way to get the SHA-1 of any blob anywhere in the tree
without having to walk it one node at a time.

```shell
$ git ls-tree -r -t master^{tree}
100644 blob 569b350811e7bfcb2cc781956641c    README
100644 blob 8f94139338f9404f26296befa8875    Rakefile
040000 tree ae850bd698b2b5dfbac1ab5fd95a4    lib
100644 blob 7e92ed361869246dc76f0cd0e526e    lib/simplegit.rb
```

The `-t` makes it also show the SHA-1s of the subtrees themselves,
rather than just all the blobs.

#### Showing Blobs

Lastly,
you may want to extract the contents of individual blobs.
The `cat-file` command is an easy way to do that,
and can also serve to let you know what type of object a SHA-1 is,
if you don't know.
It is sort of a catch-all command that you can use to inspect objects.

```shell
$ git cat-file -t ae850bd698b2b5dfbac
tree
$ git cat-file -p ae850bd698b2b5dfbac
100644 blob 7e92ed361869246dc76    simplegit.rb
$ git cat-file -t 569b350811
blob
$ git cat-file -p 569b350811
SimpleGit Ruby Library
======================

This library calls git commands and returns the output.

It is an example for the Git Peepcode book.

Author : Scott Chacon

```

With those basic commands,
you should be able to explore and inspect any object in any git repository
relatively easily.

### Graphical Interfaces

There are two major graphical interfaces that come with Git
as tools to browse the repository.

#### Gitk

A very popular choice for browsing Git repositories
is the Tcl/Tk based browser called `gitk`.
If you want to see a simple visualization of your repository,
`gitk` is a great tool.

![](../artwork/bitmap/gitk.png)

Gitk will also take most of the same arguments that `git log` will take,
including `--since`,
`--until`,
`--max-count`,
revision ranges and path limiters.
One of the most interesting visualizations that I regularly use
is `gitk --all`,
which will show all of your branches next to each other
(rather than just the one you are currently on).

#### Instaweb

If you don't want to fire up Tk,
you can also browse your repository quickly
via the `git instaweb` command.
This will basically fire up a web server
running the [gitweb](https://git-scm.com/docs/gitweb)
CGI script using lighttpd,
apache or webrick.
It then tries to automatically fire up your default web browser
and points it at the new server.

```shell
$ git instaweb --httpd=webrick
[2008-04-08 20:32:29] INFO  WEBrick 1.3.1
[2008-04-08 20:32:29] INFO  ruby 1.8.4 (2005-12-24) [i686-darwin8.8.2]
```

![](../artwork/bitmap/instaweb.png)

When you are done,
you can run the following to shut down the server:

```shell
git instaweb --stop
```

This is a quick way to throw up a web interface on your git repository
for sharing with others
or simply browsing it in a different way.

For a more long term web interface to your repository,
you can put the gitweb Perl files that come with Git
into your `cgi-bin` directory.

- [git show](http://www.kernel.org/pub/software/scm/git/docs/git-show.html)
- [git ls-tree](http://www.kernel.org/pub/software/scm/git/docs/git-ls-tree.html)
- [git cat-file](http://www.kernel.org/pub/software/scm/git/docs/git-cat-file.html)
- [gitk](http://www.kernel.org/pub/software/scm/git/docs/gitk.html)
- [git instaweb](http://www.kernel.org/pub/software/scm/git/docs/git-instaweb.html)
