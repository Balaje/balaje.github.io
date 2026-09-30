---
layout: default
title: Blog
permalink: /blog/
---

{% for post in site.posts %}

<article>

    <h2>
        <a href="{{ post.url | relative_url }}">
            {{ post.title }}
        </a>
    </h2>

    <p class="post-date">
        {{ post.date | date: "%B %-d, %Y" }}
    </p>

    {{ post.excerpt }}

    <hr style="border: none; border-top: 3px dotted #3ABFC0" />

</article>

{% endfor %}