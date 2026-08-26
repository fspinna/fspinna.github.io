---
layout: page
permalink: /teaching/
title: teaching
description: Course Materials
nav: true
nav_order: 6
---

<!-- pages/teaching.md -->
<div class="projects">
{% assign years = site.courses | map: "academic_year" | uniq | sort | reverse %}
{% for year in years %}
  <a id="{{ year | slugify }}" href=".#{{ year | slugify }}">
    <h2 class="category">{{ year }}</h2>
  </a>
  {% assign courses = site.courses | where: "academic_year", year | sort: "title" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for course in courses %}
      {% assign project = course %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
{% endfor %}
</div>
