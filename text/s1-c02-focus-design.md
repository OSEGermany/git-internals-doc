<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Focus and Design

There are a number of areas that the developers of Git,
including and especially Linus,
have focused on in conceiving and building Git.
There may be a lot of things that Git is not good at,
but these things are what Git is *very* good at.

### Non-Linear Development

Git is optimized for cheap and efficient branching and merging.
It is built to be worked on simultaneously by many people,
having multiple branches developed by individual developers,
being merged,
branched and re-merged constantly.
Because of this,
branching is incredibly cheap and merging is incredibly easy.

### Distributed Development

Git is built to make distributed development simple.
No repository is special or central in Git -
each clone is basically equal and could generally replace any other one at any time.
It works completely offline or with hundreds of remote repositories
that can push to and/or fetch from each other over several simple and standard protocols.

### Efficiency

Git is very efficient.
Compared to many popular SCM systems,
it seems downright unbelievably fast.
Most operations are local,
which reduces unnecessary network overhead.
Repositories are generally packed very efficiently,
which often leads to surprisingly small repo sizes.

> **NOTE** \
The Ruby on Rails Git repository download,
which includes the full history of the project -
every version of every file,
weighs in at around 13M,
which is not even twice the size of a single checkout of the project (\~9M).
The Subversion server repository for the same project is about 115M.

Git also is efficient in its network operations -
the common Git transfer protocols transfer only packed versions
of only the objects that have changed.
It also won't try to transfer content twice,
so if you have the same file under two different names,
it will only transfer the content once.

### A Toolkit Design

Git is not really a single binary,
but a collection of dozens of small specialized programs,
which is sometimes annoying to people trying to learn Git,
but is pretty cool when you want to do anything non-standard with it.
Git is less a program and more a toolkit
that can be combined and chained to do new and interesting things.

> **NOTE** \
For a long time,
Git was just the raw toolkit
and the project to wrap those into a user friendly SCM
was called *Cogito*.
That project has since been abandoned as Git itself became easier to use.

The tools can be more or less divided into two major camps,
often referred to as the *porcelain* and the *plumbing*.
The plumbing is not really meant to be used by people on the command line,
but rather to do simple things flexibly
and are combined by programs and scripts into porcelain programs.
The porcelain programs are largely what we will be focusing on in this book ---
the user-oriented interfaces to do SCM type things ---
hiding the low-level fun.
