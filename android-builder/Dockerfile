#
# Copyright (C) 2024 Vladimir `rifux` Blinkov
#
# SPDX-License-Identifier: Apache-2.0
#

# Selecting Ubuntu 24.04 LTS 
FROM ubuntu:24.04

# About
LABEL description="Envify: AOSP build system" \
      maintainer="Vladimir Blinkov <contact@rifux.dev>"

# Setting up the time zone
RUN ln -snf /usr/share/zoneinfo/UTC /etc/localtime

# Updating
RUN apt-get update && \
      apt-get install -y software-properties-common && \
      apt-get full-upgrade -y

# Installing building dependencies
RUN apt-get install -y wget bc bison build-essential ccache curl flex g++ g++-multilib \
      gcc gcc-multilib git git-lfs gnupg gperf imagemagick lib32readline-dev lib32z1-dev \
      libelf-dev liblz4-tool libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop \
      pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev

# Installing additional building dependencies
RUN apt-get install -y apktool autoconf automake axel clang cmake expat gawk libc6-dev \
      libcap-dev libexpat1-dev libgmp-dev ^liblz4-.* ^liblzma.* libmpc-dev libmpfr-dev \
      libtool ^lzma.* maven ncftp ncurses-dev patch patchelf pkg-config pngquant re2c \
      subversion texinfo unzip w3m lzip libxml-simple-perl libswitch-perl apt-utils \
      python-is-python3

# Installing usable developing packages
RUN apt-get install -y sudo 

# Cleaning up the cached packages
RUN apt-get autoclean

# Creating user for building
RUN useradd -m -s /usr/bin/bash user
RUN usermod -aG sudo user
RUN echo "user:e" | chpasswd

# Installing repo
RUN curl --create-dirs -L -o /usr/local/bin/repo -O -L https://storage.googleapis.com/git-repo-downloads/repo && \
      chmod a+rx /usr/local/bin/repo

# Installing platform tools
RUN curl --create-dirs -L -o /tmp/platformTools/platform-tools-latest-linux.zip -O -L https://dl.google.com/android/repository/platform-tools-latest-linux.zip
RUN unzip /tmp/platformTools/platform-tools-latest-linux.zip -d /tmp/platformTools && \
      rm /tmp/platformTools/platform-tools-latest-linux.zip
RUN chmod a+rx /tmp/platformTools/platform-tools/*
RUN mv /tmp/platformTools/platform-tools/lib64/libc++.so /usr/lib64/libc++.so && \
      rmdir /tmp/platformTools/platform-tools/lib64
RUN mv /tmp/platformTools/platform-tools/* /usr/bin/ && \
      rm -r /tmp/platformTools

# Installing 
RUN curl --create-dirs -L -o /tmp/payloadDumper/payload-dumper-go.tar.gz -O -L https://github.com/ssut/payload-dumper-go/releases/download/1.2.2/payload-dumper-go_1.2.2_linux_amd64.tar.gz
RUN tar xf /tmp/payloadDumper/payload-dumper-go.tar.gz -C /tmp/payloadDumper
RUN chmod a+rx /tmp/payloadDumper/payload-dumper-go
RUN mv /tmp/payloadDumper/payload-dumper-go /usr/bin/payload-dumper-go
RUN rm -r /tmp/payloadDumper