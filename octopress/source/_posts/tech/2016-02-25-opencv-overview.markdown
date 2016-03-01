---
layout: post_tech
title: "OpenCV Overview"
date: 2016-02-25 00:13:01 +0800
comments: true
categories: [tech]
tags: [cpp, opencv]
toc: true
---

- installation
- modules
- api

## Installation

- OS: Ubuntu 14.04

```bash
$ sudo apt-get install libopencv-dev python-opencv
$ # sudo apt-get autoremove libopencv-dev python-opencv
$ pkg-config opencv --cflags
$ pkg-config opencv --libs
$ pkg-config --modversion opencv
```

test

```bash
$ vim test.cpp
$ g++ test.cpp -o test
$ ./test
```

## Modules

- `core`: main OpenCV, data structure + basic image processing
- `highgui`: user interface, image + video codecs/capturing/mouse etc.
- `imgproc`: image processing algorithms, image filtering/transformations etc.
- `video`: video analysis, object tracking algorithms + background subtraction etc.
- `objdetect`: object detection + object recognition


## API

### header files

```cpp
#include "opencv2/core/core.hpp"
#include "opencv2/flann/miniflann.hpp"
#include "opencv2/imgproc/imgproc.hpp"
#include "opencv2/photo/photo.hpp"
#include "opencv2/video/video.hpp"
#include "opencv2/features2d/features2d.hpp"
#include "opencv2/objdetect/objdetect.hpp"
#include "opencv2/calib3d/calib3d.hpp"
#include "opencv2/ml/ml.hpp"
#include "opencv2/highgui/highgui.hpp"
#include "opencv2/contrib/contrib.hpp"
#include "opencv2/core/core_c.h"
#include "opencv2/highgui/highgui_c.h"
#include "opencv2/imgproc/imgproc_c.h"
``` 

### namespace

```cpp
#include "opencv2/core/core.hpp"
using namespace cv;

int main()
{
  Mat frame = cvQueryFrame(capture);
  imshow("Video", frame);
}
```

```cpp
#include "opencv2/core/core.hpp"

int main()
{
  cv::Mat frame = cvQueryFrame(capture);
  cv::imshow("Video", frame);
}
```

### data types for arrays

single channel array:

- CV_8U (8 bit unsigned integer)
- CV_8S (8 bit signed integer)
- CV_16U (16 bit unsigned integer)
- CV_16S (16 bit signed integer)
- CV_32S (32 bit signed integer)
- CV_32F (32 bit floating point number)
- CV_64F (64 bit float floating point number)

multi channel array:

- CV_8UC1 (single channel array with 8 bit unsigned integers) 
- CV_8UC2 (2 channel array with 8 bit unsigned integers)
- CV_8UC3 (3 channel array with 8 bit unsigned integers)
- CV_8UC4 (4 channel array with 8 bit unsigned integers)
- CV_8UC(n) (n channel array with 8 bit unsigned integers (n can be from 1 to 512) )


