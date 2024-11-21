<!--
SPDX-FileCopyrightText: 2013 Geoffrey Grosenbach <boss@topfunky.com>
SPDX-FileCopyrightText: 2023-2024 Robin Vobruba <hoijui.quaero@gmail.com>

SPDX-License-Identifier: CC-BY-SA-3.0
-->

# Git Internals PDF

[![License: CC-BY-SA-3.0](
    https://img.shields.io/badge/License-CC--BY--SA--3.0-blue.svg)](
    LICENSE.txt)
[![REUSE status](
    https://api.reuse.software/badge/github.com/osegermany/git-internals-pdf)](
    https://api.reuse.software/info/github.com/osegermany/git-internals-pdf)

[![In cooperation with Open Source Ecology Germany](
    https://raw.githubusercontent.com/osegermany/tiny-files/master/res/media/img/badge-oseg.svg)][OSEG]

<img src="artwork/title.svg" alt="Git Internals cover image" width="100%">

A document that explains the internal workings
of the [Git source code control system][git].

- [<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/PDF_file_icon.svg/195px-PDF_file_icon.svg.png" alt="PDF file icon" height="32">
  PDF][pdf]
- [<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Epub_logo_color.svg/180px-Epub_logo_color.svg.png" alt="EPub file icon" height="32">
  E-Book (`*.epub`)][epub]
- [<img src="https://cdn4.iconfinder.com/data/icons/hodgepodge-vol-1/32/code_script_html_symbols-32.png" alt="HTML file icon" height="32">
  HTML][html]

## History

- 2008:
  Written an published by [Scott Chacon][Scott].

  [Scott] and [Pluralsight] have generously open sourced it.

  [![Pluralsight](http://s.pluralsight.com/mn/img/sh/logo-v2.png)][Pluralsight]

- 2013:
  [Pluralsight] stopped maintaining the project.

- 2023:
  [Open Source Ecology Germany][OSEG] took over.

  [<img
    src="https://osegermany.gitlab.io/oseg-logo/logo_community_below.svg"
    alt="Open Source Ecology Germany logo"
    height="128">][OSEG]

## Similar Projects

- [GeeksForGeeks - Git Internals](
  https://www.geeksforgeeks.org/git-internals/);
  a very short intro to the topic.
- [A Beginner’s Guide to Git and Git Internals](
  https://medium.com/geekculture/a-beginners-guide-to-git-and-git-internals-1e7dcd89d65e)
- [Git Book - Git Internals (chapter 10)](
  https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain);
  the official documentation about Git internals.
- [A Visual Guide to Git Internals](
  https://www.freecodecamp.org/news/git-internals-objects-branches-create-repo/)
- _Git’s database internals_; GitHub's shot at the topic,
  in parts:
  1. [packed object store](
      https://github.blog/open-source/git/gits-database-internals-i-packed-object-store/)
  2. [commit history queries](
      https://github.blog/open-source/git/gits-database-internals-ii-commit-history-queries/)
  3. [file history queries](
      https://github.blog/open-source/git/gits-database-internals-iii-file-history-queries/)
  4. [distributed synchronization](
      https://github.blog/open-source/git/gits-database-internals-iv-distributed-synchronization/)

[CC-BY-SA-3.0]: https://creativecommons.org/licenses/by-sa/3.0/
[git]: https://git-scm.com/
[epub]: https://osegermany.github.io/git-internals-doc/ebook/doc.epub
[html]: https://osegermany.github.io/git-internals-doc/html/text/s0-c00-title.html
[OSEG]: https://www.ose-germany.de/die-bewegung/
[pdf]: https://osegermany.github.io/git-internals-doc/pdf/doc.pdf
[Pluralsight]: https://www.pluralsight.com
[Scott]: https://github.com/schacon
