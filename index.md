---
layout: default
---

## Hello 👋

Welcome to my research website. I was a postdoctoral fellow at Umeå University, Sweden from Dec 2022 - Feb 2026. I received my PhD from the University of Newcastle, Australia in August 2022. I did my M.Sc (Hons) Mathematics and B.E (Hons) Mechanical Engineering from BITS Pilani Goa Campus. My interests lie in numerical methods to solve partial differential equations arising in real--life applications. You can read more about my work in my [blog](/blog-page.html).

## My 3 Recent Posts
---

<ul class="post-list">
{%- for post in site.posts limit:3 -%}
  <li>
  {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
  <span class="post-meta">{{ post.date | date: date_format }}</span>
  <h3>
    <a class="post-link" href="{{ post.url | relative_url }}">
      {{ post.title | escape }}
    </a>
  </h3>
  </li>
{%- endfor -%}
</ul>

## My Recent Projects
---
I am currently working on
- **Topology Optimization:**
  Topology optimization of an acoustic black hole. This is joint work with 
    - [Martin Berggren](https://www.umu.se/en/staff/martin-berggren/), Umeå University, Sweden.
    - [Eddie Wadbro](https://www.kau.se/en/researchers/eddie-wadbro), Karlstad University, Sweden, and     
    - [Seyedabbas Mousavi](https://www.kth.se/profile/smousavi?l=en), KTH Royal Institute of Technology, Sweden,    

  I wrote a sample tutorial on automatic differentiation (AD) in density-based topology optimization using Juila [(TopOpt@github)](https://github.com/Balaje/TopOpt-Presentation). I used [Gridap.jl](https://github.com/gridap/Gridap.jl) to handle the finite element part, and formulate the adjoint method as a reverse mode AD rule. This AD interface plays well with simple functions acting on the design, such as the penalty in the Solid Isotropic Material with Penalization technique.
    
- **Multiscale finite element method:**
  High-order multiscale finite element methods to solve boundary value problems with highly oscillatory/random coefficients. This is joint work with 
    - [Siyang Wang](https://www.umu.se/en/staff/siyang-wang/), Umeå University, Sweden,
    - [Felix Krumbiegel](https://www.uni-saarland.de/lehrstuhl/rupp/team/dr-felix-krumbiegel.html), Saarland University, Germany, and 
    - [Roland Maier](https://ianm.math.kit.edu/english/jrg/npde/maier.php), Karlsruhe University, Germany.

  Here is the repository [MsFEM.jl@github](https://github.com/Balaje/MsFEM.jl) containing the code to implement the method discussed in our papers:
  > - *Kalyanaraman, B., Krumbiegel, F., Maier, R., & Wang, S. (2026).* **Enriched higher-order multiscale approaches with applications to wave propagation**. *Submitted* arXiv [Math.NA]. Retrieved from [https://arxiv.org/abs/2605.30118](https://arxiv.org/abs/2605.30118)<br>
  > - *Kalyanaraman, B., Krumbiegel, F., Maier, R., & Wang, S. (2025).* **Optimal higher-order convergence rates for parabolic multiscale problems**. *Submitted* arXiv [Math.NA]. Retrieved from [https://arxiv.org/abs/2510.09514](https://arxiv.org/abs/2510.09514)

## My Past Projects
---
- [PML for Elastic Waves](https://github.com/Balaje/Summation-by-parts): **Perfectly Matched Layers (PML) for Elastic Waves**

  Worked on the analysis and implementation of the perfectly matched layers for the elastic wave equation in a layered media. The work involved theoretically establishing the stability of the PML model and then numerically solve the governing equations using higher order summation-by-parts (SBP) finite difference methods. An example for a curvilinear domain is shown below:

  ![SBP1](https://raw.githubusercontent.com/Balaje/Summation-by-parts/refs/heads/master/Images/2-layer-1.0.png) | ![SBP2](https://raw.githubusercontent.com/Balaje/Summation-by-parts/refs/heads/master/Images/2-layer-2.0.png) | ![SBP3](https://raw.githubusercontent.com/Balaje/Summation-by-parts/refs/heads/master/Images/2-layer-40.0.png) 
  
  This is joint work with 
    - [Siyang Wang](https://www.umu.se/en/staff/siyang-wang/), Umeå University, Sweden,
    - [Kenneth Duru](https://hb2504.utep.edu/Home/Profile?username=kduru), University of Texas at El Paso, USA.
  
  The results are discussed in this paper:

  > *Duru, K., Kalyanaraman, B., & Wang, S. (2025).* **On the stability analysis of the perfectly matched layer for the elastic wave equation in layered media**. Journal of Computational Physics, 114268.

- [GSoC 2021](/gsoc/index.html): **Google Summer of Code 2021 with NumFOCUS**

  Selected to be a part of GSoC 2021 with Gridap.jl and NumFOCUS. I will be working under [Oriol Colomés](http://www.oriolcolomes.com), [Santiago Badia](https://research.monash.edu/en/persons/santiago-badia-rodriguez) and [Eric Neiva](https://github.com/ericneiva) during the program. Read more [here](https://summerofcode.withgoogle.com/projects/#6175012823760896) or click the image!

  | [![GSoC 2021](/img/gsoc.png)](/gsoc/index.html) |

- [iceFEM@github](https://github.com/Balaje/iceFem): A FreeFem based open-source package to simulate a variety of linear hydro-elasticity problems.

  | ![3D Vibration of a boat](/img/boatCav1.png) | ![Vibration of a non-uniform ice--shelf (BEDMAP2)](/img/BM1.png) |

  The algorithms are based on the papers:

  > - *Kalyanaraman, B., Meylan, M. H., Bennetts, L. G., & Lamichhane, B. P. (2020).*
  **A coupled fluid-elasticity model for the wave forcing of an ice-shelf.**
  Journal of Fluids and Structures, 97, 103074.
  > - *Ilyas, M., Meylan, M. H., Lamichhane, B., & Bennetts, L. G. (2018).*
  **Time-domain and modal response of ice shelves to wave forcing using the finite element method.**
   Journal of Fluids and Structures, 80, 113–131.

   This is joint work with my supervisors:

   - [Mike Meylan](https://www.newcastle.edu.au/profile/mike-meylan), University of Newcastle.
   - [Bishnu Lamichhane](https://www.newcastle.edu.au/profile/bishnu-lamichhane), University of Newcastle.
   - [Luke Bennetts](https://luke-bennetts.com/), University of Adelaide.
