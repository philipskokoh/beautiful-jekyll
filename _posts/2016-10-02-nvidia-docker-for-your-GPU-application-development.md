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

`--rm` flag is useful for running short-term foreground processes. You can omit this option for debugging and development. It is always advisable to name your container when you keep your container using `--name`. If you write your python codes in jupyter notebook for your development, you can also specify port mapping using `-p` flag.

{% highlight shell %}
## run bash shell in tensorflow container
$ nvidia-docker run -it --name bash-tensorflow compute.nvidia.com/nvidia/tensorflow bash

## run jupyter notebook in tensorflow image and publish to host port 8888
$ nvidia-docker run -it -p 8888:8888 --name my-tensorflow compute.nvidia.com/nvidia/tensorflow jupyter notebook
{% endhighlight %}

You can access your notebook at `localhost:8888` in your host machine.


References:

- [NVIDIA Docker: GPU Server Application Deployment Made Easy](https://devblogs.nvidia.com/parallelforall/nvidia-docker-gpu-server-application-deployment-made-easy/)
- [nvidia-docker wiki](https://github.com/NVIDIA/nvidia-docker/wiki)