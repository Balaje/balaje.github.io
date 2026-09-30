---
layout: post
mathjax: true
title: A New Project
tags: [hydroelasticity,fem,freefem++]
visible: 0
youtubeId: ZD2xiQg1Vn4
---
It is quarantine time and that means, it is time to keep myself busier
than usual. And what better time to write a blog post! Exploring finite elements with FreeFem++ has been a wonderful experience so far and it has helped me out so much throughout my PhD. The grammar of FreeFem++ enables flexibility in programming while still leaving a high degree of control with the user. The video below shows a fluid simulation in which a jet of fluid entering a porous medium (a trapezoid) vertically. Since we have an obstacle, the fluid escapes from the sides of the porous medium. Thef code is a single script which can work with any polynomial degree of approximation ($$P1,P2,P3$$ finite element spaces).

<div class="video-container">
  <iframe
    src="https://www.youtube.com/embed/{{ page.youtubeId }}"
    width="100%"
    height="400"
    frameborder="5"
    allowfullscreen="">
  </iframe>
</div>

But here, I want to share something special that I have begun to work on. I call it **iceFEM++** which is aimed to be an open-source program for modeling ice-shelf vibrations. You can find the package [here on GitHub.](https://github.com/Balaje/iceFem) You can take a look at the `README.md` file in the project for more details on how to use the program. The core computations in the package is handled by FreeFem++ and the visualization is handled by MATLAB. But I want to talk a bit more about the most salient feature of the package.

## BEDMAP2 dataset and FEM

[BEDMAP2 is a dataset](https://www.bas.ac.uk/project/bedmap-2/) describing the surface elevation, ice-thickness and the sea-floor and subglacial bed elevation of the Antarctic continent. To reliably predict vibrations of the ice-shelf due wave-forcing, bathymetry and the thickness data are important. The toolbox is available for MATLAB and can be downloaded [here](https://www.mathworks.com/matlabcentral/fileexchange/42353-bedmap2-toolbox-for-matlab). Install the package using the package manager for MATLAB and run the following commands to obtain the handles for the ice and cavity profiles.

```matlab
%% Generate the map of Antarctica and the ice--shelves
antmap
load coast
patchm(lat,long, [0.5,0.5,0.5]);
bedmap2 patchshelves
[hice,hbed,hwater]=bedmap2_profile();
```

This opens a map of the continent along with the ice-shelves. **Click** two points on the map to define a path and hit **Enter** to create a profile as shown.

<p style='text-align: center;'>
<img width="660" height="515" src="/img/ice-cavity.png" border="2">
</p>

The magenta outlines indicate the profile that is stored in `[hice, hbed]`. Once the data is obtained, we need to construct cubic splines that describes the profile. This can be done using the [`spline` command](https://www.mathworks.com/help/matlab/ref/spline.html)
```matlab
f=spline(x,y);
```
 which returns a structure `f` containing the spline data. The structure contains the Coefficients of the cubic spline in the array `f.coefs` and the interval in `f.breaks`. These can then be used to parametrize the cubic spline in FreeFem++ to define the borders and construct the mesh. I tried generating the meshes for the **Brunt ice-shelf** and this is what I got (open in a new tab to view full image).

<p style='text-align: center;'>
<img width="860" height="515" src="/img/brunt.png" border="2">
</p>

To summarize, I took real-life ice/cavity data from the BEDMAP2 dataset and constructed the finite element meshes using FreeFem++ to analyze the vibrations of the ice-shelf *(Flexible, but with control still with the user!)*. The code to solve the problem will be up soon and in the next few posts, I will be talking about them in detail. Visit the software website [here](https://balaje.github.io/iceFem/) for more features and keep track of what is going on.

Thanks!
