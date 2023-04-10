<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Exporting Git

If you want to create a release of your code,
or provide some poor non-git user with a snapshot of just a specific tree,
you can use the **git-archive** command.

> **NOTE** \
'git-archive' used to be called 'git-tar-tree',
in case you ever see that command around in older articles

You can create the archive in either 'tar' or 'zip' formats,
the default being 'tar'.
You can use the '---prefix' argument to determine what directory,
if any,
the files are expanded into.
To create a gzipped tarball,
you'll have to pipe the output through 'gzip' first.

```shell
git-archive --prefix=simplegit/ v0.1 | gzip > simple-git-0.1.tgz
```

Then,
if you email that tarball to someone,
they would get this when they opened it:

```shell
$ tar zxpvf simple-git-0.1.tgz
simplegit/README
simplegit/Rakefile
simplegit/TODO
simplegit/lib/
simplegit/lib/simplegit.rb
```

You can also archive parts of your project.
This command will create a zip file of just the 'lib' directory
of the first parent of your master branch
that will expand out into the current directory:

```shell
git-archive --format=zip master^ lib/ > simple-git-lib.zip
```

Which will unzip like this:

```shell
$ unzip simple-git-lib.zip
Archive:  simple-git-lib.zip
ce9b0d5551762048735dd67917046b44176317e0
   creating: lib/
  inflating: lib/simplegit.rb
```

- [git archive](http://www.kernel.org/pub/software/scm/git/docs/git-archive.html)
