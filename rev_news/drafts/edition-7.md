---
title: Git Rev News Edition 7 (September 9th, 2015)
layout: default
date: 2015-09-09 21:06:51 +0100
author: chriscool
categories: [news]
navbar: false
---

## Git Rev News: Edition 7 (September 9th, 2015)

Welcome to the 7th edition of [Git Rev News](http://git.github.io/rev_news/rev_news.html),
a digest of all things Git. For our goals, the archives, the way we work, and how to contribute or to
subscribe, see [the Git Rev News page](http://git.github.io/rev_news/rev_news.html) on [git.github.io](http://git.github.io).

This edition covers what happened during the month of August 2015.

## Discussions

<!---
### General
-->

### Reviews

* [t5004: test ZIP archives with many entries](http://thread.gmane.org/gmane.comp.version-control.git/275682/focus=276393)

Johannes Schauer reported that git-archive does not use the zip64
extension and therefore is unable to properly create zip archives with
more than 16k entries.

René Scharfe agreed that there is a problem. It comes from the fact
that without the zip64 extension, the zip field for the number of
entries has only two bytes. The limit is then 65535 entries.

René then sent a patch series to add tests for this problem and then
fix it. The first patch contains the following code, which tests that
a suitable `zipinfo` command is available on the current machine, and
sets the ZIPINFO prerequesite if this is the case:

```
+ZIPINFO=zipinfo
+
+test_lazy_prereq ZIPINFO '
+	n=$("$ZIPINFO" "$TEST_DIRECTORY"/t5004/empty.zip | sed -n "2s/.* //p")
+	test "x$n" = "x0"
+'
```

Eric Sunshine replied that unfortunately the above would work neither
on MacOS X where the `zipinfo` output is different, nor on FreeBSD
where `zipinfo` has been removed in favor of `unzip -Z`.

Eric then discussed in details the possibility of using `unzip -Z`
instead of `zipinfo` to have a portable test, but it appears that this
doesn't work well on files using the zip64 extension on MacOS X and
FreeBSD .

After further discussing this, Eric, René and Junio agreed that it was
good enough that the patch and the above `zipinfo` check work on Linux
as we don't need to test the archive generated by `git archive` on
every platform.

* [git-p4: add option to store files in Git LFS on import](http://thread.gmane.org/gmane.comp.version-control.git/276719/)

Lars Schneider, who is in the process of migrating huge Perforce repositories to
Git, posted a RFC (Request For Comments) patch to add an option to
git-p4 to store some big files into [Git LFS](https://git-lfs.github.com/).

Luke Diamand, who has been working a lot on git-p4 during the past
years, reviewed his patch saying first that it would be better to have
both a generic mechanism to handle big files and a separate Git LFS
extension rather than a specific mechanism for Git LFS. Luke also
noticed that the changes seem to require Python 3.

The discussion then focused on the merit of migrating git-p4 from
Python 2 to Python 3 until John Keeping chimed in writing:

> Documentation/CodingGuidelines currently says:
>
>  - As a minimum, we aim to be compatible with Python 2.6 and 2.7.
>
>  - Where required libraries do not restrict us to Python 2, we try to
>    also be compatible with Python 3.1 and later.

and then explaining the reason why it's difficult to migrate.

Fortunately it seems that this has not discouraged Lars to send a
[second version of his patch](http://thread.gmane.org/gmane.comp.version-control.git/277227/)
that starts to address Luke's concerns and that Luke has already reviewed.

Hopefully, thanks to this ongoing work, we will soon have an easy way
to migrate Perforce repos that contains big files.

<!---
### Support
-->

## Releases

* Git for Windows 2.5.0 [was released](http://article.gmane.org/gmane.comp.version-control.msysgit/21805). It is the first release based on Git 2.x, the first release based on [MSys2](https://msys.github.io/) and the first release dropping the *-preview* suffix.

## Other News

__Various__

* Johannes Schindelin, aka Dscho, wrote
  [a personal note](http://thread.gmane.org/gmane.comp.version-control.git/277194)
  to the list explaining that since mid-August he has been working
  full time for Microsoft on Git for Windows. He has already been the
  Git for Windows maintainer in his spare time since the beginning of
  this project, as well as a Git developer for more than 10 years, but
  now he is as he says "really excited to join the club of Git
  developers who get paid to work on Git as part of their day-jobs".
  Congratulations to him!

__Light reading__


__Git tools and sites__


## Credits

This edition of Git Rev News was curated by Christian Couder &lt;<christian.couder@gmail.com>&gt;,
Thomas Ferris Nicolaisen &lt;<tfnico@gmail.com>&gt; and Nicola Paolucci &lt;<npaolucci@atlassian.com>&gt;,
with help from XXX.