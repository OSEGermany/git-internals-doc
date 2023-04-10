<!--
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## The Care and Feeding of Git

Git requires a bit of tender loving care from time to time.
It may seem a bit odd,
but occasionally you should run a few commands on your repositories
to make sure they're healthy and running as quickly as possible.

### garbage collection

The `git gc` command is an important one to remember.
It will pack up your objects into the delta-compressed format,
saving you a lot of space and seriously speeding up several commands.

```shell
$ git gc
Generating pack...
Done counting 91 objects.
Deltifying 91 objects...
 100% (91/91) done
Writing 91 objects...
 100% (91/91) done
Total 91 (delta 33), reused 0 (delta 0)
Pack pack-9ca918b18abca509b2de71439522a62178415ebd created.
Removing unused objects 100%...
Done.
```

If can turn gc'ing automatically on and off
by setting a configuration setting to '1' or '0':

```shell
$ git config --global gc.auto 1
```

This will make git automatically gc itself occasionally.
You may want to setup a cron to do this at night,
however,
as it can take a while sometimes on really large repositories.

### fsck and prune

If you want to check the health of your repository,
you can run 'git-fsck',
which will tell you if you have any unreachable
or corrupted objects in your database
and help you fix them.

```shell
$ git fsck
dangling tree 8276318347b8e971733ca5fab77c8f5018c75261
dangling blob 2302a5a4baec369fb631bb89cfe287cc002dc049
dangling blob cb54512d0a989dcfb2d78a7f3c8909f76ad2326a
dangling tree 8e1088e1cc1bc67e0ef01e018707dcb07a2a562b
dangling blob 5e069ed35afae29015b6622fe715c0aee10112ad
```

Which you can then remove with 'git-prune'
(you can run it with '-n' first to see what it will do)

```shell
$ git prune -n
2302a5a4baec369fb631bb89cfe287cc002dc049 blob
5e069ed35afae29015b6622fe715c0aee10112ad blob
8276318347b8e971733ca5fab77c8f5018c75261 tree
8e1088e1cc1bc67e0ef01e018707dcb07a2a562b tree
cb54512d0a989dcfb2d78a7f3c8909f76ad2326a blob
$ git prune
$ git fsck
```

- [git gc](http://www.kernel.org/pub/software/scm/git/docs/git-gc.html)
- [git fsck](http://www.kernel.org/pub/software/scm/git/docs/git-fsck.html)
- [git prune](http://www.kernel.org/pub/software/scm/git/docs/git-prune.html)
