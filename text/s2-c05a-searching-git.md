<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Searching Git

Git has an easy way for searching through trees in your repository
without having to check them out into your working directory
to do it manually.
It is called 'git-grep'
and works very much like the traditional UNIX 'grep' command,
with the difference
that instead of listing the files you want to search as an argument,
you list the trees you want to search.

For example,
if we wanted to search for the string 'log_syslog'
in versions 1.0 and 1.5.3.8 of the Git source code in the C files only,
we can find that very easily.

```shell
$ git grep -n 'log_syslog' v1.5.3.8 v1.0.0 -- *.c
v1.5.3.8:daemon.c:16:static int log_syslog;
v1.5.3.8:daemon.c:92:   if (log_syslog) {
v1.5.3.8:daemon.c:768:        if (log_syslog)
v1.5.3.8:daemon.c:1055:         log_syslog = 1;
v1.5.3.8:daemon.c:1063:         log_syslog = 1;
v1.5.3.8:daemon.c:1112:         log_syslog = 1;
v1.5.3.8:daemon.c:1177: if (log_syslog) {
v1.0.0:daemon.c:13:static int log_syslog;
v1.0.0:daemon.c:45:     if (log_syslog) {
v1.0.0:daemon.c:423:          if (log_syslog)
v1.0.0:daemon.c:615:            log_syslog = 1;
v1.0.0:daemon.c:623:            log_syslog = 1;
v1.0.0:daemon.c:653:    if (log_syslog)

$ git grep -n -c 'log_syslog' v1.5.3.8 v1.0.0 -- *.c
v1.5.3.8:daemon.c:7
v1.0.0:daemon.c:6
```

You can see that you can view the number of lines that match,
or the actual lines,
and you can list as many trees (in this case,
I used tags to reference them) as you want to search.

Another interesting example
is to see which files in these versions
do not contain the string 'git' anywhere in them:

```shell
$ git grep -L -v git v1.5.3.8 v1.0.0
v1.5.3.8:contrib/fast-import/git-p4.bat
v1.5.3.8:contrib/p4import/README
v1.5.3.8:t/t5100/patch0007
v1.5.3.8:t/t5100/patch0008
v1.5.3.8:templates/this--description
v1.0.0:debian/git-arch.files
v1.0.0:debian/git-cvs.files
v1.0.0:debian/git-doc.files
v1.0.0:debian/git-email.files
v1.0.0:debian/git-svn.files
v1.0.0:debian/git-tk.files
v1.0.0:templates/this--description
```

- [git grep](http://www.kernel.org/pub/software/scm/git/docs/git-grep.html)
