---
layout: post_tech
title: "Installing OpenCV on Ubuntu"
date: 2016-02-25 18:59:03 +0800
comments: true
categories: [tech]
tags: [opencv, ubuntu, virtualenv, virtualenvwrapper]
toc: true
---

Installation Steps:

- Dependencies
- Python
- OpenCV
- Test

## Dependencies

```bash
$ # update os
$ sudo apt-get update
$ sudo apt-get upgrade

$ # update devtools
$ sudo apt-get install build-essential cmake git pkg-config

$ # image I/O dependencies
$ sudo apt-get install libjpeg8-dev libtiff4-dev libjasper-dev libpng12-dev

$ # highgui dependencies
$ sudo apt-get install libgtk2.0-dev

$ # video dependencies
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev

$ # opencv optimization dependencies
$ sudo apt-get install libatlas-base-dev gfortran
```

## Python

```bash
$ # pip and virtualenv
$ sudo apt-get install python-pip
$ sudo pip install virtualenv virtualenvwrapper
$ vim ~/.bashrc
$ #######################################
$ # virtualenv and virtualenvwrapper
$ export WORKON_HOME=$HOME/.virtualenvs
$ source /usr/local/bin/virtualenvwrapper.sh
$ #######################################
$ source ~/.bashrc
$ mkvirtualenv cv
$ sudo apt-get install python2.7-dev
$ pip install numpy
```

## OpenCV

```bash
$ # opencv
$ cd ~
$ git clone https://github.com/Itseez/opencv.git
$ cd opencv
$ git checkout 3.0.0

$ cd ~
$ git clone https://github.com/Itseez/opencv_contrib.git
$ cd opencv_contrib
$ git checkout 3.0.0

$ cd ~/opencv
$ mkdir build
$ cd build
# when build python, numpy is a must
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_C_EXAMPLES=ON \
        -D INSTALL_PYTHON_EXAMPLES=ON \
        -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
        -D BUILD_EXAMPLES=ON ..
$ make -j4
$ sudo make install
$ sudo ldconfig
```

Installed Paths:

- OpenCV: /usr/local/share/OpenCV
- PyOpenCV: /usr/local/lib/python2.7/site-packages/cv2.so

## Test

```bash
$ # config opencv
$ cd ~/.virtualenvs/cv/lib/python2.7/site-packages/
$ cp /usr/local/lib/python2.7/site-packages/cv2.so cv2.so

$ # test
$ workon cv
$ python
$ >>>import cv2
$ >>>cv2.__version__
```

demo

```python
# import the necessary packages
import numpy as np
import cv2
 
# load the games image
# games.jpg: http://www.pyimagesearch.com/wp-content/uploads/2015/06/games.jpg
image = cv2.imread("games.jpg")
 
# find the red color game in the image
upper = np.array([65, 65, 255])
lower = np.array([0, 0, 200])
mask = cv2.inRange(image, lower, upper)
 
# find contours in the masked image and keep the largest one
(_, cnts, _) = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL,
	cv2.CHAIN_APPROX_SIMPLE)
c = max(cnts, key=cv2.contourArea)
 
# approximate the contour
peri = cv2.arcLength(c, True)
approx = cv2.approxPolyDP(c, 0.05 * peri, True)
 
# draw a green bounding box surrounding the red game
cv2.drawContours(image, [approx], -1, (0, 255, 0), 4)
cv2.imshow("Image", image)
cv2.waitKey(0)
```
