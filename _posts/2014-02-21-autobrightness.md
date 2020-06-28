---
layout: page
mathjax: true
title: Autobrightness
tags: [opencv, imageprocessing]
visible: 0
---
**This post was published in 2014 and as such the program might not
be up to date and as a result may not work properly. However it was a
cool project at the time!!**

This post is about an application that I tried to make using the
OpenCV Image Processing Library. This application can detect the
changes in brightness of the environment and adjust your laptop’s
screen brightness accordingly.

Pixel is the smallest element in an image with definite properties
like color, position, intensity etc. A typical colored pixel has three
channels RED, GREEN and BLUE.  Let me denote this as a 3-tuple
$$(R,G,B)$$. Colors like magenta, yellow etc. are derived from these 3
primary colors and hence they are called derived colors. In image
processing the derived colors are the various combinations of
$$(R,G,B)$$. For example If I consider the RED color as $$(255,0,0)$$, BLUE as
$(0,0,255)$ and GREEN as $$(0,255,0)$$. Then YELLOW can be represented as
$$(255,255,0)$$.


Gray-scale images are those images which has only one channel and let
us denote this by $$I$$. Grayscale images are useful when it comes to
processing images, as it reduces three channels to one and thus
eliminates redundancy. This is an example of a typical Grayscale
image.

<p style='text-align: center;'>
<img width="473" height="315" src="/img/lenatest3.jpg" border="2">
</p>

Now after understanding what colored images and grayscale images are;
let us move towards the basic algorithm of the application.

First, the application captures images from the laptop’s webcam.
Second, the application computes the average intensity (I) of the
entire camera input. Changes brightness according to the intensity values.
As an extra feature the application also supports Gesture Brightness Control.
Now, the question is how to get the average intensity?

``` cpp
Mat img;
int s = (int)img.at<uchar>(j,i);
```

This is an overloaded C++ template function which returns the pixel
value at the position (j,i). This function is only used for a
grayscale image i.e. ‘img’ should be grayscale image.

To get the average intensity of all pixels in a grayscale image, we
can use a ‘for’ loop.

``` cpp
unsigned long int sum;
for(int i=0; i<img.rows; i++)
{
    for(int j=0 j<img.cols; j++)
    {
        sum += (int)img.at<uchar>(i,j);
    }
}
int intensity = sum/(img.rows*img.cols);
```


Now we have computed the average intensity of the frame. Now our next
step is to change the brightness depending on the value of the
intensity. In linux, the brightness can be changed by editing.


>/(file system) -> sys -> class -> backlight -> acpi_video0 -> brightness

To change the brightness, open the terminal and run this command. (As root)

``` bash
echo 10 > /sys/class/backlight/acpi_video0/brightness
```

If you want to try out the application, download the source tar-ball
from [here](https://sourceforge.net/projects/autobrightness/).

Then extract the archive, run the install.sh script.

```bash
tar -xvf Autobrightness.tar.gz
cd Autobrightness
chmod +x install.sh
./install.sh
```

After installing, close and reopen the terminal and type

``` bash
autobrightness
```
to start the application.

This installation script is only for linux (Ubuntu/mint), but the
source code can be compiled in other Operating Systems like Windows
etc.
