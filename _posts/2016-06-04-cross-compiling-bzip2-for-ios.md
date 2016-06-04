---
layout: post
title: Cross-compiling bzip2 for iOS
description: Learn to cross-compile bzip2 on Linux for iOS devices.
image: bzip2.png
---

As detailed in my previous blog post, I showed you how to compile Git using an on-device toolchain. Now, I'll show you how to do something similar, but instead, on Linux.

First things first, we're going to need the iOS SDK and Linux Toolchain. You can download the former from [this website](https://jbdevs.org/sdks/) and the latter from [here](https://developer.angelxwind.net/Linux/).

This tutorial assumes you are running Ubuntu 16.04 LTS. If this does not apply to you, then you may need to make some modifications to the following processes.

OK, now that you've got the SDK and Toolchain, it's time to extract them. Suppose my home directory was `/home/citrusui`.  Create two folders in this directory called `sdks` and `toolchain`, then unzip the files to these locations accordingly.

Now, its time to download a script that automates the compiling process. Make sure you run this process as root or sudo!

{% gist 806037423d954191b11b36a45995be08 %}

All right, what does this script do? Well...

- Clears out cache of any previously "built" versions of bzip2
- Sets the current version of bzip2 (1.0.6)
- Specifies which compiler to use (armv7-apple-darwin11-clang)
- Sets some CFLAGS and LDFLAGS in order to build for iOS
- Installs some dependencies needed for the compile process
- Downloads bzip2 source code
- Unzips source code, and compiles it
- Runs lndir to avoid any issues with Cydia stashing
- Copies the current version of bzip2 (1.0.6) to the Debian control file
- Packages the directory in ~/built/bzip2 into a .deb

You may notice that a file, aptly named "bzip2-control" is specified in this script too. This is a file that you'll need to add manually, in the same directory that you've saved bzip2.sh to. It should look something like this:

```
Package: bzip2
Priority: important
Section: Archiving
Maintainer: Your Name <your@email.com>
Architecture: iphoneos-arm
Version: 
Description: Freely available high-quality data compressor
Homepage: http://www.bzip.org/

```

Make sure you leave a blank line at the bottom of your control file, or else `dpkg-deb` will return an error.

**Important Note:** I left out a step that involves running `ldid -S` on all of the bzip2 binaries. Without this step, iOS will refuse to run bzip2. I'll be adding this step in sometime later.

Hopefully this tutorial helped you cross-compile bzip2 for iOS! Feel free to upload the generated package to your personal Cydia repository. If I manage to get Git to properly cross-compile, I might make a follow-up to the previous blog post.
