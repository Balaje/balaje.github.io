---
layout: default
---

# Welcome to my research website.

My name is Balaje Kalyanaraman and I am a PhD student at the
University of Newcastle, Australia. I did my M.Sc (Hons) Mathematics
and B.E (Hons) Mechanical Engineering from BITS Pilani - Goa Campus. My
interests lie in numerical methods to solve partial differential
equations arising in real--life applications. Some of my most recent
works includes the simulation of wave-induced ice--shelf vibrations using the
finite element method. You can read more about my work in my [blog](/blog-page.html).

## My 3 Recent Posts

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

## My current projects

- [iceFEM@github](https://github.com/Balaje/iceFem): A FreeFem based open-source package to simulate a variety of linear hydro-elasticity problems.

  | ![3D Vibration of a boat](/img/boatCav1.png) | ![Vibration of a non-uniform ice--shelf (BEDMAP2)](/img/BM1.png) |

  The algorithms are based on the papers

  > *Kalyanaraman, B., Meylan, M. H., Bennetts, L. G., & Lamichhane, B. P. (2020).*
  **A coupled fluid-elasticity model for the wave forcing of an ice-shelf.**
  Journal of Fluids and Structures, 97, 103074.

  > *Ilyas, M., Meylan, M. H., Lamichhane, B., & Bennetts, L. G. (2018).*
  **Time-domain and modal response of ice shelves to wave forcing using the finite element method.**
   Journal of Fluids and Structures, 80, 113–131.

   This is joint work with my supervisors:

   - [Mike Meylan](https://www.newcastle.edu.au/profile/mike-meylan), University of Newcastle.
   - [Bishnu Lamichhane](https://www.newcastle.edu.au/profile/bishnu-lamichhane), University of Newcastle.
   - [Luke Bennetts](https://luke-bennetts.com/), University of Adelaide.
