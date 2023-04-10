<!--
SPDX-FileCopyrightText: 2008 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2008 Scott Chacon <schacon@gmail.com>
SPDX-FileCopyrightText: 2013 Doğan Aydın <dogan1aydin@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

# Installing Git

Before we can start playing with Git,
we'll have to install it.
I'll quickly cover installing Git on Linux,
Mac and Windows.
I will not get into really fine detail,
because others have done that much better,
but I will give you an overview and links
as to where to find more detailed instructions on each platform.

For any of these examples,
you can find a link to the most current Git source code
at <https://github.com/git/git>.

I would recommend compiling from source if possible,
simply because Git is lately making big strides in usability,
so more current versions may be a bit easier to use.

## Installing on Linux

If you are installing from source,
it will go something like the standard:

```shell
wget https://kernel.org/pub/software/scm/git/git-1.5.4.4.tar.bz2
tar jxpvf git-1.5.4.4.tar.bz2
cd git-1.5.4.4
make prefix=/usr all doc info
sudo make prefix=/usr install install-doc install-info
```

If you are running Ubuntu or another Debian based system,
you can run

```shell
apt-get install git-core
```

or on yum based systems,
you can often run:

```shell
yum install git-core
```

## Installing on Mac

**TODO** This section is way outdated; needs revising

You are likely going to want to install Git without the asciidoc dependency,
because it is a pain to install.
Other than that,
what you basically need is Curl and Expat.
With the exception of the Leopard binary OS X installer,
you will also need the Developer Tools installed.
If you don't have the OS X install discs anymore,
you can get the tools from **TODO**.

## Windows

There are two options on Windows currently,
but the popular one is [MSysGit](http://code.google.com/p/msysgit/),
which installs easily and can be run on the Windows command line.
Simply download the exe file from the [downloads list](
http://code.google.com/p/msysgit/downloads/list),
execute it and follow the on-screen instructions.
