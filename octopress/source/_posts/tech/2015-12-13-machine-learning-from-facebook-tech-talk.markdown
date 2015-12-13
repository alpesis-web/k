---
layout: post_tech
title: "Machine Learning from Facebook Tech Talk"
date: 2015-12-13 22:39:48 +0800
comments: true
categories: [tech]
tags: [machine learning, tech talks]
toc: true
---

There were three topics about machine learning on Facebook Tech Talk 2015 (Shanghai).

- Ads Click
- Friend Recommendation
- Content Feeds

Login Facebook, and go to the user dashboard, content feeds are in the middle, friend 
recommendataion is located at the right sidebar (top right), ads click is located at 
the right sidebar (behind the friend recommendation)

```
    |------------------------------------------------------------------|
    |             |                                  |                 |
    |             |                                  |                 |
    |             | content feeds                    | friends         |
    |             |                                  | ads             |
    |             |                                  |                 |
    |------------------------------------------------------------------|
```

## 1. Ads Click

NOTE:

If you play a song on piano, some phrases might be easy to play, some might be hard, then
 you have to practice the hard parts again and again, memorizing the techniques by hands.

For machine learning, somelike as the same. One algorith might be fitted for some data, 
some might be not, for improving the accurency, you'd better apply other algorithms which 
are fit these kinds of the dataset to the incorrect outputs, retrain the data again and 
again.

### Overview

<img src="https://s-media-cache-ak0.pinimg.com/736x/9f/87/62/9f87621c1ed63452776097be842713de.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/5f/64/13/5f641372540309676f956a4771034e24.jpg" />

### Layer 1

<img src="https://s-media-cache-ak0.pinimg.com/474x/d2/73/ad/d273adf93b99c3b0813584976a77f8ce.jpg" />

### Layer 2

<img src="https://s-media-cache-ak0.pinimg.com/736x/8d/5d/4f/8d5d4fa8dc863fa4d226c36b91a07bc7.jpg" />

### Model

<img src="https://s-media-cache-ak0.pinimg.com/474x/67/4e/26/674e268a17b893d14ac2a2a3d8b507c6.jpg" />

### Outputs

<img src="https://s-media-cache-ak0.pinimg.com/474x/f4/b8/7c/f4b87c2a0afda71f33786bfc06bb186b.jpg" />


When a user views the ads once, he/she might not click these ads, but he/she might click 
the ads a few minutes later, then we'd better show the ads to the user by time ranges (
such as 15 minutes later, 1 hour laster etc), that's why we use the queue.

### Evaluation

<img src="https://s-media-cache-ak0.pinimg.com/474x/1e/64/a2/1e64a2626be15098c6d7aa13643cc05b.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/3c/ba/02/3cba02e1a2b4d9548c974d86980f94dc.jpg" />


<img src="https://s-media-cache-ak0.pinimg.com/474x/fb/5e/6d/fb5e6d541ea0e57a4f303b1ade61ce5c.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/99/69/35/996935ee6fa3a20152453c9c0d20a755.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/0e/68/60/0e6860c4634db9ff38097662332d00c7.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/1b/e7/5b/1be75bf3f92ab17af4a73b0398b2f7a1.jpg" />



## 2. Friend Recommendation

<img src="https://s-media-cache-ak0.pinimg.com/474x/9e/ec/14/9eec1418e533540b4ec5d64a14f1a137.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/fc/75/b7/fc75b7ce4dd488307eae141995f21db4.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/c0/3b/be/c03bbe0c49f67684b094ddbf5244c02b.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/736x/c4/ee/a9/c4eea9ff648979eb05650e0288458c61.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/69/9e/11/699e1119bc29df039a907fcb3e58a00f.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/cc/fc/ce/ccfcce6747a28857f567caa0dd8d1bf5.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/26/f6/45/26f6453e61da588965960e78c9260fd1.jpg" />



## 3. Content Feeds

<img src="https://s-media-cache-ak0.pinimg.com/474x/76/67/70/7667706db9c43f7cbe5f672b75daee03.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/0d/83/b9/0d83b9c9cd86bcfde10ff208efcadb79.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/38/dc/72/38dc72e03b726c3025d3dfcb37725d9f.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/4f/22/e9/4f22e9f3d21b05c84d634e3a7dd113d9.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/0b/38/82/0b3882c2b3c956bb8ccf8ff343e2d88a.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/04/38/4e/04384edef1d3726b9b2ea4debd3a3012.jpg" />

<img src="https://s-media-cache-ak0.pinimg.com/474x/c0/f9/e5/c0f9e5c432913bbfd310e7f4c9ea6a87.jpg" />

