---
layout: post_tech
title: "Installing Caffe on Ubuntu 14.04"
date: 2016-02-03 05:07:12 +0800
comments: true
categories: [tech]
tags: [deepdream, caffe, python, machine learning]
toc: true
---

## Prequisitions

- Vagrant/VirtualBox
- Ubuntu 14.04
- VM Memory: 4096 MB

## Caffe Installation

Requirements

- CUDA
- BLAS
- OpenCV
- Anaconda
- Boost
- Caffe dependencies
- Protobuf
- Caffe

### CUDA

If installed in the VirtualBox, `VBoxGuestAddition` should be installed first.
Otherwise, it could be booted.

```bash
# reference: https://gist.github.com/titipata/f0ef48ad2f0ebc07bcb9
# check version
$ lspci | grep -i nvidia
$ uname -m && cat /etc/*release
$ gcc --version

# CUDA
$ cd /tmp
$ wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_6.5-14_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu1404_6.5-14_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda
$ sudo reboot
```

### BLAS

```bash
$ sudo apt-get install libopenblas-dev
```

### OpenCV

```bash
$ sudo apt-get install git unzip
$ cd /tmp
$ wget https://raw.githubusercontent.com/jayrambhia/Install-OpenCV/master/Ubuntu/2.4/opencv2_4_9.sh
$ sudo chmod +x opencv2_4_9.sh
$ ./opencv2_4_9.sh
```

### Anaconda

```bash
$ cd /tmp
$ wget http://09c8d0b2229f813c1b93-c95ac804525aac4b6dba79b00b39d1d3.r79.cf1.rackcdn.com/Anaconda-2.1.0-Linux-x86_64.sh
$ sudo bash Anaconda-2.1.0-Linux-x86.sh
# path: /usr/local/anaconda
```

### Boost

```bash
$ sudo apt-get install libboost-all-dev
```


### Caffe dependencies

```bash
# reference: http://caffe.berkeleyvision.org/install_apt.html
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-devlibhdf5-serial-dev
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler
```

### Protobuf

```bash
$ sudo apt-get install python-pip
$ sudo pip install protobuf
```

### Caffe

```bash
$ cd /usr/local
$ sudo git clone https://github.com/BVLC/caffe
$ cd caffe/
$ sudo cp Makefile.config.example Makefile.config
# modify Makefile.config if needed
###################################################
# CPU-only switch (uncomment to build without GPU support).
CPU_ONLY := 1
###################################################

$ sudo make all
$ sudo make test
$ sudo vim ~/.bashrc              # update paths
$ source ~/.bashrc
$ make runtest
...
[----------] Global test environment tear-down
[==========] 1019 tests from 144 test cases ran. (34669 ms total)
[  PASSED  ] 1019 tests.
```

`~/.bashrc` config:

```bash
# CUDA
export PATH=/usr/local/cuda-7.5/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-7.5/lib64:$LD_LIBRARY_PATH
export PATH
# Anaconda
export PATH=/usr/local/anaconda/bin:$PATH
# Caffe Root
export CAFFE_ROOT=/usr/local/caffe
```

config python library

```bash
$ sudo make pycaffe
$ sudo make distribute
# make dir for custom python modules, install caffe
$ mkdir ~/pycaffe
$ mv distribute/python/caffe ~/pycaffe
#################################################################
# set PYTHONPATH (this should go in your .bashrc or whatever
# export PYTHONPATH=${HOME}/pycaffe:$PYTHONPATH
export PYTHONPATH=/usr/local/caffe/python:$PYTHONPATH
#################################################################
```

Install the python dependencies

```bash
$ cd /usr/local/cafffe/python
$ sudo pip install -r requirements.txt
```

import caffe on IPython

```bash
# fixed Anaconda issue
$ sudo rm /usr/local/anaconda/lib/libm.*
$ ipython
$ conda install protobuf
# import caffe
$ cd /usr/local/caffe/python
$ import caffe
```

import caffe on python script

```
# import matplotlib for fixing the issue when importing caffe
# so, matplotlib is a must
import matplotlib
matplotlib.use('Agg')

import sys
sys.path.insert(0, '/usr/local/caffe/python/')
import caffe
```

## DeepDream

### IPython Notebook (Optional)

```bash
$ sudo pip install ipython
$ sudo pip install jupyter
$ ipython notebook --ip 0.0.0.0
```

ipython notebook url: http://localhost:8888


### Dependencies

- Python: scipy, numpy, pillow

```bash
# python
$ sudo apt-get install python-dev python-virtualenv
# pillow
$ sudo apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev \
libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk
# scipy
$ sudo apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran
```

`base.txt`: python requirements

```
# NOTE: if it doesn't run on anaconda, `scikit-image` and `cython` 
# must be installed on PYTHONPATH. Otherwise, it will show some 
# errors when `import caffe`.
numpy==1.10.4
Pillow==3.1.0
scipy==0.17.0
protobuf==2.6.1
scikit-image==0.11.3
cython==0.23.4
```


```bash
$ sudo pip install -r base.txt
```

### Repo

```bash
$ git clone https://github.com/google/deepdream.git
$ cd deepdream
```

### Models

```bash
$ wget wget -P /usr/local/caffe/models/bvlc_googlenet http://dl.caffe.berkeleyvision.org/bvlc_googlenet.caffemodel
```

### Run

```bash
$ ipython notebook ./dream.ipynb
```
