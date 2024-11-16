---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2019-09-26T23:08:51Z"
guid: http://morgajel.net/?p=58
id: 58
title: 'Unfinished Drafts: Useful Utility: tar'
url: /2019/09/26/58
---

This is another article that sat in the drafts folder for far too long- Last edited Feb 21st, 2006.

I fear writing about tar, and that is why I’m determined to finish it in this sitting, so it won’t fester and scare me off of this series. Why am I scared of writing about tar? Well, this is their flags list verbatim from the man page:

```
       [  --atime-preserve  ] [ -b, --blocking-factor N ] [ -B, --read-full-records ] [ --backup BACKUP-TYPE ] [ --block-com-
       press ] [ -C, --directory DIR ] [ --check-links ] [ --checkpoint ] [ -f, --file [HOSTNAME:]F ] [ -F,  --info-script  F
       --new-volume-script F ] [ --force-local   ] [ --format FORMAT ] [ -g, --listed-incremental F ] [ -G, --incremental ] [
       --group GROUP ] [ -h, --dereference ] [ --help ] [ -i, --ignore-zeros ] [ --ignore-case ] [ --ignore-failed-read  ]  [
       --index-file  FILE  ]  [ -j, --bzip2 ] [ -k, --keep-old-files ] [ -K, --starting-file F ] [ --keep-newer-files ] [ -l,
       --one-file-system ] [ -L, --tape-length N ] [ -m, --touch, --modification-time ] [ -M, --multi-volume ] [ --mode  PER-
       MISSIONS  ]  [  -N,  --after-date DATE, --newer DATE ] [ --newer-mtime DATE ] [ --no-anchored ] [ --no-ignore-case ] [
       --no-recursion ] [ --no-same-permissions ] [  --no-wildcards  ]  [  --no-wildcards-match-slash  ]  [  --null      ]  [
       --numeric-owner  ]  [  -o,  --old-archive, --portability, --no-same-owner ] [ -O, --to-stdout ] [ --occurrence NUM ] [
       --overwrite ] [ --overwrite-dir ] [ --owner USER ] [ -p, --same-permissions, --preserve-permissions ]  [  -P,  --abso-
       lute-names  ] [ --pax-option KEYWORD-LIST ] [ --posix ] [ --preserve ] [ -R, --block-number ] [ --record-size SIZE ] [
       --recursion ] [ --recursive-unlink ] [ --remove-files ] [ --rmt-command CMD ] [ --rsh-command  CMD  ]  [  -s,  --same-
       order, --preserve-order ] [ -S, --sparse ] [ --same-owner ] [ --show-defaults ] [ --show-omitted-dirs ] [ --strip-com-
       ponents NUMBER, --strip-path NUMBER (1) ] [ --suffix SUFFIX ] [ -T, --files-from F ] [ --totals   ]  [  -U,  --unlink-
       first ] [ --use-compress-program PROG ] [ --utc ] [ -v, --verbose ] [ -V, --label NAME ] [ --version  ] [ --volno-file
       F ] [ -w, --interactive, --confirmation ] [ -W, --verify ] [ --wildcards ] [  --wildcards-match-slash  ]  [  --exclude
       PATTERN  ]  [  -X,  --exclude-from  FILE  ]  [  -Z,  --compress,  --uncompress  ] [ -z, --gzip, --gunzip, --ungzip ] [
       -[0-7][lmh] ]
```

So it’s a bit overwhelming. The good news is there are two common uses for tar- creating tarballs and opening tarballs. This will be a majority of your interaction with it. You get all sorts of fun options with tar, such as using different compression libraries, but it’s still pretty straight forward.

### Simple Archive

Tar produces tarballs, which in its simplest form is a bunch of data files run together into a larger file. in the following instance, -c means create, and -f means “create the following as a file called foo.tar”

```
tar -cf foo.tar bar/
```

This takes the bar directory and throws it all into a single file called foo.tar. Apart from some binary mojo to mark the separators between files, it’s almost as it all of the files were pasted end-to-end inside another file. if foo.tar is copied to another machine or place, you could untar the file with the following command:

```
tar -xf foo.tar
```

Again you see the -f flag, but the -c flag has been replaced by the extract flag, -x. This will create a directory called bar/ which will contain the contents identical to the original.

### Compressing Archives

You also have the option of compressing tarballs in the process of creating them. There are three types of compression built into the version of tar I’m using: -Z, which uses the compress utility (ancient?); -z which uses gzip (old standard); and -j, which uses b2zip, which is good for compressing binaries (appears to be the new standard).

When creating a tarball that is compressed, it’s generally expected that you label it as such by appending the type to the filename, for example:

```
tar -cZf foo1.tar.Z bar1/
tar -czf foo2.tar.gz bar2/
tar -cjf foo3.tar.bz2 bar3/
```

Unless you have a specific reason, you’ll probably want to use bz2. You’ll probably never deal with a tar.Z file, but if you do, you’ll know how to deal with it. To uncompress these puppies, switch out the -c flag for the -x flag like we did in the previous example.

```
tar -xZf foo1.tar.Z
tar -xzf foo2.tar.gz
tar -xjf foo3.tar.bz2
```

One last option you may want to look at is -v. It will show you files as they’re being processed, and can be good for troubleshooting.