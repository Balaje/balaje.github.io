---
layout: post
mathjax: true
title: Tracking Objects without color
tags: [opencv,objecttracking,imageprocessing]
---
Object tracking is one of the most important aspects of Computer
Vision. Nowadays human security guards are being replaced by automated
tracking robots for two main reasons; ONE: robots can detect even the
subtlest of motions and record them efficiently; TWO: robots don’t get
bored staring into the same space, even for days.

Object tracking can be done in numerous ways. A simple way is to
detect the color of the object and track it. But this has many
problems. First, the object must be of a uniform color. No object in
nature has a color so uniform, that it covers the entire object, like
a tree which has green as well as yellow leaves. Second, the
background. Suppose we want to track a red object in a red
background. This can’t be done using color filtering.

So what is the solution? The answer is quite simple. Detect changes in
the environment directly from sequence of images i.e. subtract two
different frames to find out the changes. Let $$F(t_1)$$ be the input
frame at time $$t_1$$ and similarly $$F(t_2)$$ be the input frame at time
$$t_2$$. Now the difference between $$F(t_1)$$ and $$F(t_2)$$ gives us only the
pixels which has changed in the time-interval $$t_2-t_1$$.

$$
    I = | F(t_2) - F(t_1) |
$$

The resulting image I has the information about the moving object
only. Contours can be extracted from this image to track the object. A
sample [video](https://www.youtube.com/watch?v=tGXxYSEEtE4) (made by
me!) demonstrates this method.

This video attempts to track the ball as it jumps across the frame,
and the result is quite good. This is done using OpenCV’s Image
Processing Library. OpenCV offers this function to compute the
absolute difference between the two Image matrices.

``` cpp
cv::absdiff(firstimage, secondImage, outputImage);
```

Stay Tuned.
