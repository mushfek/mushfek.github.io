---
layout: page
title: Research
permalink: /research/
description: Academic Research & Industry R&D Works
nav: true
---

<div class="post">
  <ul class="post-list">
    {% assign sorted_researches = site.researches | sort: "index" %}
    {% for research in sorted_researches %}
      <li>
        <h5><a class="post-title" style="text-decoration:underline" href="{{ research.url | prepend: site.baseurl }}">{{ research.title }}&nbsp;<i class="fa fa-link" aria-hidden="true"></i></a></h5>
        <p>{{ research.description }}</p>
      </li>
    {% endfor %}
  </ul>
  
  {% include pagination.html %}
</div>
