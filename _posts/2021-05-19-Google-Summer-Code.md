---
layout: post
mathjax: true
tags: [gridap, gsoc]
title: Google Summer of Code with Gridap.jl
---

I am excited to share that I will participate in the 2021 edition of
the Google Summer of Code (GSoC) with
[Gridap](https://github.com/gridap) under the NumFOCUS organization. I
will be working with [Eric Neiva](https://github.com/ericneiva),
[Oriol Colomés](http://www.oriolcolomes.com) and [Santiago
Badia](https://research.monash.edu/en/persons/santiago-badia-rodriguez)
during the program. The topic of my project is [A fast finite element
interpolator in
Gridap.jl](https://github.com/gridap/GSoC/issues/5). In this short
blog post, I will share my experiences writing a proposal for the
GSoC project. Feel free to click on the links to read the
conversation.

## Deciding the topic

Gridap offered a list of project titles through their
official GSoC repository located here. They required the applicants to
open an issue in their repo to discuss the topic in detail. I
will briefly explain the process and the discussions I
had with my mentors to finalize the topic for GSoC. I opened
an [issue](https://github.com/gridap/GSoC/issues/5) on March 19,
approximately a month before the student application closed. I
started with one of the projects listed on the official repository and
inquired whether an applicant could propose a different project.

> - [Deciding a Topic](https://github.com/gridap/GSoC/issues/5#issuecomment-802593378)

I was interested in working on a topic close to my research interest
for GSoC 2021 and proposed a project around Virtual Element
Methods. If you're wondering what virtual elements are, you could take
a look at these posts in my blog:

> - [A note on Gradient Recovery for the Virtual Element Method](/2019/11/02/A-Note-on-Gradient-Recovery.html)
> - [Virtual elements, biorthogonal projections and the peculiarity of second-order VEM](/2019/11/26/VEM-Biorth.html)

We then played around with the VEM idea a little bit while I worked
on learning Gridap. It turned out that VEM needed more tools
in Gridap to be implemented in time for a GSoC project. So we
decided to leave it for later.

> - [VEM Projectors](https://github.com/gridap/GSoC/issues/5#issuecomment-805324118)
> - [Going through Gridap](https://github.com/gridap/GSoC/issues/5#issuecomment-806252705)

While I did have good experience with numerical methods and other
automated finite element solvers like FreeFem, I did not have much
experience with Julia or Gridap. I installed the packages and tried
programming an [ice-shelf](2021/01/09/iceFEM.html) example with
Gridap. I did this for uniform geometries and compared it with some exact
solutions (the code for this is available
[here](https://github.com/Balaje/Gridap-Ice)). I then observed a few
things while solving this problem. FreeFem offers a fast finite
element interpolator of $$O(N\log N)$$ complexity to
interpolate functions between different meshes and finite element
spaces.

- [FreeFem Interpolation
matrix](https://doc.freefem.org/documentation/finite-element.html#interpolation-matrix)
- [A Fast Finite Element Interpolator in FreeFem](https://doc.freefem.org/documentation/finite-element.html#a-fast-finite-element-interpolator)

I wanted this feature in Gridap to solve more general ice-shelf problems, but
Gridap did not have it yet. It gave me an idea for
a GSoC project to implement this feature, and it felt doable too. So
along with this, I proposed a few more doable topics for my GSoC proposal.

> - [Possible Topics for GSoC](https://github.com/gridap/GSoC/issues/5#issuecomment-808703235)

I had video calls with all my mentors and subsequently chose to go with
the first idea, i.e., implementing a fast finite element interpolator
on Gridap.

> - [Deciding the topic](https://github.com/gridap/GSoC/issues/5#issuecomment-816498994)

## Writing a proposal

Writing the proposal to GSoC was a bit of work, coming up with a
suitable plan. I sent the proposal over to my mentors for
suggestions, and they made sure that I highlight all my previous
experience with open-source projects and my research. Also, having
a research website is useful! The next step is to wait for
the results. In the meantime, I learned more Julia
programming and went briefly through the Gridap API. I have listed some resources below.

> - [Zero-to-Hero Julia workshop by George Datseris](https://github.com/Datseris/Zero2Hero-JuliaWorkshop)
> - [Gridap Tutorials](https://gridap.github.io/Tutorials/dev/)
> - [Gridap Tutorial on low-level API](https://gridap.github.io/Tutorials/dev/pages/t012_poisson_dev_fe/#A-low-level-definition-of-the-cell-map-1)

## Results
Then on May 18, I received the results and was excited beyond
bounds! You can find the title and abstract on the official page
located
[here](https://summerofcode.withgoogle.com/projects/#6175012823760896).
I'll keep updating my blog as I work through the summer. I am looking
forward to an exciting summer with Gridap and NumFOCUS!

Stay tuned ;)
