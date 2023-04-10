<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Log - the Commit History

So,
now we have all this history in our Git repository.
So what? What can we do with it?
How can we see this history?

<!-- SIDEBAR
---

#### Git Log Options Screencast

The next screencast is on `git log`,
which demonstrates most of the major features and options to the `git log` command.
It includes showing the `stat`,
`short-stat` and `name-stat` options,
the `--pretty` options,
the `since` and `until` limiters,
the path limiter and author field searching.

movie. c4-git-log.mov

---
SIDEBAR -->

The answer is the very powerful `git log` command.
The `log` command can show you nearly anything you want to know
about your commit history.
Also,
since the entire history is stored locally,
it's really fast compared with most other SCM systems,
especially if your repository is packed
(see the "Care and Feeding" section).

If you just run *git log*,
you will get output like this:

```shell
$ git log
commit cf25cc3bfb0ece7dc3609b8dc0cc4a1e19ffbcd4
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:20 2008 -0700

    committing all changes

commit 0c8a9ec46029a4e92a428cb98c9693f09f69a3ff
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

commit 0576fac355dd17e39fd2671b010e36299f713b4d
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    my second commit, which is better than the first

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

This will show you the SHA-1 of each commit,
the committer and date of the commit,
and the full message,
starting from the last commit on your current branch
and going backward in reverse chronological order
(so if there are multiple parents,
it just squishes them together,
interleaving the commits ordered by date)

### Formatting Log Output

The default format takes up a lot of space though,
so there are ways to limit and format this output differently.
`--pretty` is a useful option for formatting the output in different ways.

For example,
we can list the commit SHA-1s
and the first line of the message with `--pretty=oneline`:

```shell
$ git log --pretty=oneline
cf25cc3bfb0ece7dc3609b8dc0c committing all changes
0c8a9ec46029a4e92a428cb98c9 changed the verison number
0576fac355dd17e39fd2671b010 my second commit, which is..
a11bef06a3f659402fe7563abf9 first commit
```

With `--pretty`,
you can choose between *oneline*,
*short*,
*medium*,
*full*,
*fuller*,
*email*,
*raw* and *format:(string)*,
where (string) is a format you specify with variables
(ex: `--format:"%an added %h on %ar"`
will give you a bunch of lines like
"Scott Chacon added `f1cc9df` 4 days ago").

### Filtering Log Output

There are also a number of options for filtering the log output.
You can specify the maximum number of commits you want to see with `-n`,
you can limit the range of dates you want to see commits for
with `--since` and `--until`,
you can filter it on the author or committer,
text in the commit message and more.
Here is an example showing at most 30 commits
between yesterday and a month ago by me :

```shell
git log -n 30 --since="1 month ago" --until=yesterday --author="schacon"
```

- [git log](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-log.html)
