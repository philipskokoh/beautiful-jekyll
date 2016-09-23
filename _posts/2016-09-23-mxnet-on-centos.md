---
layout: post
title: Install MXNet on CentOS 7
tags: [mxnet, centOS]
published: true
---

MXNet [installation guide](http://mxnet.readthedocs.io/en/latest/how_to/build.html) does not provide a guideline to install MXNet on CentOS 7. Here are the steps to build MXNet on CentOS 7:

1. Install dependencies

    ```bash
    yum groupinstall "Development Tools" 
    yum install atlas-devel opencv-devel

    ```
  
2. Clone MXNet project

    ```bash
    git clone --recursive https://github.com/dmlc/mxnet

    ```
  
3. Set MSHADOW_LDFLAGS in `mshadow/make/config.mk` from `-lcblas` to `-lsatlas` (line 56)

    ```bash
    MSHADOW_LDFLAGS += -lsatlas
    ```
  
4. Make

    ```bash
    make -j$(nproc)

    ```
  
5. Follow official MXNet [Python package installation](http://mxnet.readthedocs.io/en/latest/how_to/build.html#python-package-installation) guide for MXNet python package installation
