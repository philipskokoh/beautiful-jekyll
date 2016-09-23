---
layout: post
title: Install MXNet on CentOS 7
tags: [mxnet, centOS]
published: true
---

MXNet [installation guide](http://mxnet.readthedocs.io/en/latest/how_to/build.html) does not provide a guideline to install MXNet on CentOS 7. Here are the steps to build MXNet on CentOS 7:

#### 1. Install dependencies

{% highlight shell %}
yum groupinstall "Development Tools" 
yum install atlas-devel opencv-devel
{% endhighlight %}
  
#### 2. Clone MXNet project

{% highlight shell %}
git clone --recursive https://github.com/dmlc/mxnet
{% endhighlight %}
  
#### 3. Set MSHADOW_LDFLAGS in `mshadow/make/config.mk` from `-lcblas` to `-lsatlas` (line 56)

{% highlight shell %}
MSHADOW_LDFLAGS += -lsatlas
{% endhighlight %}
  
#### 4. Make

{% highlight shell %}
make -j$(nproc)
{% endhighlight %}
  
#### 5. MXNet Python package installation

Follow official MXNet [Python package installation](http://mxnet.readthedocs.io/en/latest/how_to/build.html#python-package-installation) guide
