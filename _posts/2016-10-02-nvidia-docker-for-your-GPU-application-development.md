---
layout: post
title: nvidia-docker for your GPU application development
tags: [NVIDIA, GPU, DGX-1, docker]
published: true
---

We just finished installing DGX-1. Yayy!! :tada::tada::tada: 

NVIDIA suggests the use of `nvidia-docker` to develop and prototype GPU application on DGX-1. The reason is that many popular deep learning frameworks such as torch, mxnet, tensorflow, theano, caffe, CNTK, and DIGITS use specific version of NVIDIA driver, libraries, and configurations. Docker container provides hardware and software encapsulation, allowing multiple containers run on the same systems using their own specific configurations. [nvidia-docker](https://github.com/NVIDIA/nvidia-docker/wiki) is a thin wrapper around docker command which enables portability in GPU-based images that use NVIDIA GPUs by providing driver agnostic CUDA images. 

This post shows basic commands to get started using nvidia-docker for GPU application development. You need to have NVIDIA driver, Docker, and `nvidia-docker` installed in your system. Check these links to install docker and nvidia-docker:

- [Docker installation](https://docs.docker.com/engine/installation/)
- [nvidia-docker installation](https://github.com/NVIDIA/nvidia-docker#quick-start)

`nvidia-docker` wrapper only modifies docker `run` and `create` commands. The other commands are just pass-through to the docker-cli. To list all docker images in your system:
{% highlight shell %}
$ nvidia-docker images
{% endhighlight %}

Use `nvidia-docker run` to run a container. The format is

> `nvidia-docker` run &lt;docker-options&gt; &lt;image_name&gt; command

Here are some examples:
{% highlight shell %}
## run nvcc --version in compute.nvidia.com/nvidia/cuda image
$ nvidia-docker run -it --rm compute.nvidia.com/nvidia/cuda nvcc --version

## run nvidia-smi to monitor NVIDIA GPU devices in compute.nvidia.com/nvidia/cuda image
$ nvidia-docker run -it --rm compute.nvidia.com/nvidia/cuda nvidia-smi

## run tensorflow mnist demo in compute.nvidia.com/nvidia/tensorflow image
$ nvidia-docker run -it --rm compute.nvidia.com/nvidia/tensorflow python -m tensorflow.models.image.mnist.convolutional
{% endhighlight %}

In these examples, `-it` is docker option to set an interactive connection and assign a terminal inside the new container, `--rm` flag tells docker to automatically clean up and remove the container when it exits. Docker images which start with `compute.nvidia.com/nvidia/` are official docker images from NVIDIA.

`--rm` flag is useful for running short-term foreground processes. You can omit this option, if you want to keep a container when it exits for debugging and development. It is always advisable to name your container when you keep your container using `--name`. To start your container again, use `nvidia-docker start`.

{% highlight shell %}
## run bash shell in tensorflow container
## the container will not be removed after it exits
$ nvidia-docker run -it --name my-tensorflow compute.nvidia.com/nvidia/tensorflow bash

## start and connect back to previously created container my-tensorflow
$ nvidia-docker start my-tensorflow
$ nvidia-docker attach my-tensorflow

## delete the container
$ nvidia-docker rm my-tensorflow
{% endhighlight %}

If you write your python codes in jupyter notebook for your development, you can specify port mapping using `-p` flag. You can also mount a directory in your host machine to the container using `-v` flag. `-w` flag indicates the location where the command will be executed

{% highlight shell %}
## run jupyter notebook (using default parameter) in compute.nvidia.com/nvidia/tensorflow image
## publish jupyter default port 8888 to port 8080 in the host machine
## mount /home/philips/data to /opt/data in the container
## jupyter will be run in the mounted /opt/data directory 
## You can access your notebook at localhost:8080 in the host machine.
$ nvidia-docker run -it -p 8080:8888 --name tensorflow-notebook -v /home/philips/data:/opt/data -w /opt/data compute.nvidia.com/nvidia/tensorflow jupyter notebook
{% endhighlight %}


References:

- [NVIDIA Docker: GPU Server Application Deployment Made Easy](https://devblogs.nvidia.com/parallelforall/nvidia-docker-gpu-server-application-deployment-made-easy/)
- [nvidia-docker wiki](https://github.com/NVIDIA/nvidia-docker/wiki)