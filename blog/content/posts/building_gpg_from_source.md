---
author: "Nolan"
title: "Bulding GPG on Amazon linux from source"
date: "2022-08-06"
categories: ["amazonlinux", "gpg"]
draft: false
description: "Building GPG"
tags: ["docker", "amazonlinux", ]
ShowToc: false
TocOpen: false
---


## Bulding a newer version of GPG on AL2

Amazon linux 2, based on centos 7 is using a older version GPG making it incompatible with some projects [see this thread](https://github.com/argoproj/argo-cd/issues/8086).

And so one solution, is to build a newer version of GPG from source.

Here is the github repo: 

```Dockerfile
FROM amazonlinux

RUN yum -y install bzip2 wget gcc make tar

COPY install_gpg.sh .
RUN chmod +x install_gpg.sh && ./install_gpg.sh

ENV PATH="/var/src/gnupg2/gnupg-2.2.15/dest/bin:$PATH"
```

And the script compiling from source:

```sh
#... bulding all the dependent libraries...
echo "Building and installing GnuPG version ${GNUPG_VER}"
wget -c https://gnupg.org/ftp/gcrypt/gnupg/gnupg-${GNUPG_VER}.tar.bz2
wget -c https://gnupg.org/ftp/gcrypt/gnupg/gnupg-${GNUPG_VER}.tar.bz2.sig
gpg --verify gnupg-${GNUPG_VER}.tar.bz2.sig
tar xjof gnupg-${GNUPG_VER}.tar.bz2
cd gnupg-${GNUPG_VER} || exit 1
echo "Creating dest folder for binaries"
mkdir dest
autoreconf -fi 2>/dev/null
./configure --sysconfdir=/etc --localstatedir=/var \
--enable-g13 --enable-symcryptrun --enable-large-secmem --with-capabilities --prefix=/var/src/gnupg2/gnupg-${GNUPG_VER}/dest
make -s
make check
make install
```
Full script [here](https://github.com/emirot/amazon-linux-gpg/blob/main/install_gpg.sh).
