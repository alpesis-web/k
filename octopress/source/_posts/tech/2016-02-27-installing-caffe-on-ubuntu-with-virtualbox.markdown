---
layout: post_tech
title: "Installing Caffe on Ubuntu with VirtualBox"
date: 2016-02-27 22:47:46 +0800
comments: true
categories: [tech]
tags: [caffe, virtualbox, ubuntu, cuda]
toc: true
---

## Ubuntu + VirtualBox

- download Ubuntu ISO file
- create a new VM in VirtualBox (Linux/Ubuntu/64bit/DynamicHD/8GbRAM/…)
- install Ubuntu OS and update and upgrade

```bash
$ sudo apt-get install build-essential
$ sudo apt-get install linux-headers-`uname -r`
```

VirtualBox

- VirtualBox ToolBar -> Devices -> Insert Guest Additions CD Image -> Install GuestAddition
- VirtualBox VM -> Preferences -> General -> Advanced -> Shared Clipboard

## Cuda

[CUDA 7.5](https://developer.nvidia.com/cuda-downloads)

```bash
$ sudo apt-get install curl
$ cd ~/Downloads/
$ curl -O "http://developer.download.nvidia.com/compute/cuda/6_5/rel/installers/cuda_6.5.14_linux_64.run"
$ chmod +x cuda_6.5.14_linux_64.run
$ sudo ./cuda_6.5.14_linux_64.run --kernel-source-path=/usr/src/linux-headers-`uname -r`/
# - Accept the EULA
# - DO NOT INSTALL DRIVER
# - Install the toolkit
# - Install the symbolic link
# - Install samples

$ echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
$ echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/lib' >> ~/.bashrc
$ source ~/.bashrc
```

## Caffe

```bash
$ sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev protobuf-compiler gfortran libjpeg62 libfreeimage-dev libatlas-base-dev git python-dev python-pip libgoogle-glog-dev libbz2-dev libxml2-dev libxslt-dev libffi-dev libssl-dev libgflags-dev liblmdb-dev python-yaml
$ sudo easy_install pillow

$ cd ~
$ git clone https://github.com/BVLC/caffe.git
$ cd caffe
$ cat python/requirements.txt | xargs -L 1 sudo pip install
$ sudo ln -s /usr/include/python2.7/ /usr/local/include/python2.7
$ sudo ln -s /usr/local/lib/python2.7/dist-packages/numpy/core/include/numpy/ /usr/local/include/python2.7/numpy

$ cp Makefile.config.example Makefile.config
$ nano Makefile.config
# - uncomment # CPU_ONLY := 1
# - PYTHON_INCLUDE: 
#   - /usr/lib/python2.7/dist-packages/numpy/core/include -> /usr/local/lib/python2.7/dist-packages/numpy/core/include


$ make pycaffe
$ make all
$ make test
# if something error, then it could not recompile, try to delete build and .build_release
# and then rerun the commands
# or use `make clean`
```

## Test

```bash
$ ./scripts/download_model_binary.py models/bvlc_reference_caffenet
$ ./data/ilsvrc12/get_ilsvrc_aux.sh
```

## Reference

[Ubuntu 14.04 VirtualBox VM](https://github.com/BVLC/caffe/wiki/Ubuntu-14.04-VirtualBox-VM)
