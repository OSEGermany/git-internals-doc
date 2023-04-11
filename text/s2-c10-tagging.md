<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2008 Scotty <schacony@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Tagging

As we previously covered,
creating a tag in Git is much like making a branch.
A tag in Git serves is basically a signed branch that never moves -
it is simply an arbitrary string that points to a specific commit.

For example,
if you wanted to tag your code base
every time you released to production
or created a new binary to release,
you would run something like this:

```shell
git tag -a v0.1 -m 'this is my v0.1 tag'
```

As we recall from section one,
that command will create a git object
that looks something like this:

![](../artwork/vector/tag-expand.eps)

and).

### Lightweight Tags

You can also create a tag
that doesn't actually add a Tag object to the database,
but just creates a reference to it in the '.git/refs/tags' directory.
If you run the following command:

```shell
git tag v0.1
```

Git will create the same file as before,
'.git/refs/tags/v0.1',
but it will contain the SHA-1 of the current HEAD commit itself,
not the SHA-1 of a Tag object pointing to that commit.
Unlike object Tags,
these can be moved around easily,
which is generally undesirable.

### Signed Tags

At the other end of the tagging spectrum,
you can sign Tag object with a GPG key
to ensure its cryptographic integrity.
Replacing '-a' with '-s' in the command
will create a Tag object
and sign it with the current users email address GPG key.
If you want to specify a key,
you can run it with '-u' instead:

```shell
git tag -u <key-id> v0.1 -m 'the 0.1 release'
```

Then,
you or others can later verify that signed tag with a '-v'

```shell
git tag -v v0.1
```

- [git tag](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-tag.html)
