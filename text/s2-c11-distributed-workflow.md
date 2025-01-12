<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2008 Scotty <schacony@gmail.com>
SPDX-FileCopyrightText: 2023 Richard Soderberg <rsoderberg@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

## Distributed Workflow Examples

Now we've gone over most of the basic commands
that you'll use on a day to day basis as a single developer.
This chapter covers some examples
of what you will use in order to collaborate with other developers
on a code base.

<!-- SIDEBAR
---

#### Distributed Workflow Screencast

This screencast demonstrates a distributed workflow.
It takes two personas,
creating a project in GitHub and pushing to it in the first persona,
then cloning that project in the second.
The second sets up a public,
read-only HTTP repository on his own server.
The first then fetches from that,
merges changes and pushes back to GitHub.
It also demonstrates an instance
in which the Author and Committer fields can differ for a commit.

movie. c8-dist-workflow.mov

---
SIDEBAR -->

### Cloning

If you want to begin working on an existing project,
you will need to get an initial version of it -
copy its repository of objects and references to your machine.
This is done with a clone.
Git can clone a repository over several transports,
including local,
HTTP,
HTTPS,
SSH,
its own `git` protocol,
and `rsync`.
The `git` protocol and SSH are preferred,
because they are more efficient and not difficult to set up.

When you clone a repository,
it in essence copies all the git objects to a new directory,
checks you out a single local branch
named the same as the HEAD branch on the cloned repo
(normally 'master'),
and stores all the other branches under a remote reference,
by default named 'origin'.

That means that if we cloned the repo in the previous examples,
instead of 'story84' being a local branch you can switch to,
it becomes 'origin/story84'
that you have to create a local branch to pull into,
in order to work on
(eg: `git checkout --track story84 origin/story84`)\
The '---track' indicates
that you may want to pull from or push to the origin of this branch later,
so remember where it came from.

#### Local Clones

Local clones are the simplest types of clones -
it is basically the equivalent of copying the .git directory
and doing a checkout.
The only major difference is
that it adds all the original branches in as origin branches.
Often you will do this when creating a bare repository
(that is,
a repository without a working directory)
for putting on a public server,
or if you're working with people using a shared repository over NFS
or something similar.

```shell
git clone --bare simplegit/.git simplegit-bare.git
```

#### SSH and Git Transports

Cloning over SSH requires
that you have user credentials on the machine you are cloning from.
The git transport does not have this authentication
and so is normally used for fetching only.

```shell
git clone git@github.com:schacon/ticgit.git ticgit_directory
```

#### HTTP and HTTPS Transports

Another popular way to clone a repository is over HTTP,
just because it is so simple.
You don't need to setup any special service or give out user credentials
(made easier by services like gitosis),
you simply scp your bare repository
into any web server's static content directory.
It is not as efficient as the other protocols -
it will transfer loose objects and packfiles
over a number of calls instead of packing them up,
but it is simple.

```shell
git clone https://git.gitorious.org/piston/mainline.git piston
```

Once you have run one of these commands,
you will have a copy of the git repository,
full of all the history -
basically every blob and tree and commit that project has ever had.
This is really a full backup of the repository -
if the main server ever goes down or gets corrupted,
everyone who has ever cloned it has a fully capable backup
that can replace it.
With Git,
there is really no single point of failure.

- [git clone](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-clone.html)

### Fetching and Pulling

So let's say that we're going to hack on TicGit,
our git based ticket tracking system project.
After we clone it,
we look through the source code but don't do anything right away.
After some time passes
we come back to the project but it may not still be up to date -
changes may have occurred in the meantime.
So we fetch an update.

```shell
git fetch origin
```

This will contact the server over the same protocol we used to clone it
and grab all of the objects and references
that have been added since our clone
and update our 'origin/\[branch\]' branches
to point to what the server is pointing at now.

So,
if we did create a tracking branch on 'story84'
and it was changed on the server (someone pushed an update),
before we fetch,
our local 'story84' branch and our remote 'origin/story84' branch
will be the same.
After we fetch,
they will be different.
'origin/story84' will now point to one of the new commit objects
we downloaded during the fetch.

At this point,
we may want to merge 'origin/story84' into our local 'story84' branch.
That's easy enough,
but if we want to do it automatically every time we fetch,
we can use `git pull`,
which is just a 'fetch' and then a 'merge' command.

So,
these commands are functionally equivalent:

```shell
git pull origin/story84
```

```shell
git fetch origin/story84; git merge origin/story84
```

- [git fetch](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-fetch.html)
- [git pull](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-pull.html)

### Pushing

Now we can get updates from other repositories,
but how can we push changes to them?
If we have commit rights on the repository (normally over SSH),
we can simply run `git push`.

```shell
git push origin master
```

The 'origin' in that case will be inferred if you leave it out,
but if you've used a different name for your remote
or you are trying to push one of your other branches,
you can do that,
too.

```shell
git push scott-public experimental
```

If you don't specify a branch,
it will infer that you want to push every branch
that you and the server have in common.
So,
if you have pushed your 'master' branch and your 'experimental' branch
to the 'scott-public' server at any point,
running this will update the server
to have the newest versions of **both** of them:

```shell
git push scott-public
```

Whereas this will only update the 'master' branch:

```shell
git push scott-public master
```

> **NOTE** \
In Git,
the opposite of 'push' is not 'pull',
but 'fetch'.
A 'pull' is a 'fetch' and then a 'merge'.

- [git push](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-push.html)

### Multiple Remotes

Although a bit different in syntax maybe,
most of that should seem familiar to any users of other SCM systems.
However,
this is where the 'decentralized' part comes in.
In Git,
there is really no special repository.
You can add as many remote repositories
that are related to your codebase in some way as you want.
You can add each of your co-workers repositories as read-only repositories,
you can have a centralized one you all share,
one out on your production servers outside the firewall,
a public one for stable or sanitized pushes on your personal webserver,
one on your build server,
etc,
etc.

Pushing to and pulling from multiple sources is easy and straightforward.
You simply add remotes :

```shell
git remote add mycap git@github.com:schacon/capistrano.git
git remote add official git://github.com/jamis/capistrano.git
```

Then,
if the the project is updated,
I can pull in the changes from one remote,
merge them locally,
and then push to another remote.

```shell
git fetch official
git merge official/master
git push mycap master
```

I can also add several remotes to pull and merge from,
in this case,
one for every developer with a public fork of that project
that might push changes I care to try.

![](../artwork/diagrams/fetch-pull.svg)

You can also remove remotes at any time,
which simply removes the lines
that contain the URL in your `.git/config` file
and the references to their remote branches
in `.git/refs/[remote_name]` directory.
It will not remove any of the git objects,
so if you decide to add it again and fetch,
very little will be transferred.

You can also view useful information about a remote branch
by using the @remote show@ command.
For example,
if I run this on a checkout of the Git source code itself,
I will see this:

```shell
$ git remote show origin
* remote origin
  URL: git://git.kernel.org/pub/scm/git/git.git
  Remote branch(es) merged with 'git pull' while on branch master
    master
  Stale tracking branches in remotes/origin (use 'git remote prune')
    old-next
  Tracked remote branches
    html maint man master next pu todo
```

- [git remote](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-remote.html)

### Possible Workflows

The idea of having multiple remote repositories
that you can push to and/or pull from is probably new to you,
and many people have a hard time figuring out
what their workflow should look like,
especially if they are moving from a centralized SCM system.
I will present a couple of possible workflows that I have seen,
so you can determine what will work best for you and your team.

Most of these are simply a matter of convention,
not even configuration.
Each of the models can pretty easily change to another
with minimal configuration changes -
maybe some permissions tweaked here or there.

#### Central Repository Model

There is a single repository that all developers push to and pull from.

![All developers push to a single server](../artwork/diagrams/workflow-star.svg)

This model works just like a centralized SCM
and Git can work that way just fine.
If you setup a repository for your team on a server
that everyone has SSH or NFS access to,
Git can very easily function as a centralized repository.
This may be common on small teams with non-public projects
where you don't want to worry about a hierarchy -
the strength of this model is
that it forces everyone to stay up to date with each other
and it doesn't depend on a single role.

Even large teams could use this,
but in general
there are a lot of gains to be made in larger teams
with a different or hybrid model.

#### Dictator and Lieutenant Model

This is a highly hierarchical model
where one individual has commit rights to a blessed repository
that everyone else fetches from.
Changes are fetched from developers by lieutenants
responsible for specific subsystems and merged and tested.
Lieutenant branches are then fetched by the dictator
and merged and pushed into the blessed repository,
where the cycle starts over again.

![Approved features gradually make their way up the ladder](
../artwork/diagrams/workflow-dictator.svg)

This is a model something like the Linux kernel uses,
Linus being the benevolent dictator.
This model is much better for large teams,
and can be implemented
with multiple and varied levels of lieutenants and sub-lieutenants
in charge of various subsystems.
At any stage in this process,
patches or commits can be rejected -
not merged in and sent up the chain.

#### Integration Manager Model

This is where each developer has a public repository,
but one is considered the 'official' repository -
it is used to create the packages and binaries.
A person or core team has commit rights to it,
but many other developers have public forks of that repository.
When they have changes,
they issue a pull request to an integration manager,
who adds them as a remote if they haven't already -
then merges,
tests,
accepts and pushes.

![Private and public repositories driven by read-only pull requests](
../artwork/diagrams/workflow-integration-manager.svg)

This is largely how community-based git repositories
like GitHub were built to work
and how many smaller open source projects operate.

In the end,
there is really no single right way to do it -
being a decentralized system,
you can have a model with all of these aspects to it,
or any combination you can think of.
You can also have subgroups using different models on the same codebase.
For example,
if your company might have an internal fork of the Linux kernel
that is managed by the Integration Manager model
in addition to pulling in changes occasionally from Linuses branch.
In the end,
you and your team will have to think about what will work best for you.
