---
layout: post
title: "A Post with Jupyter Notebook"
date: 2025-03-07 13:16:00 +0200
description: "An example of a blog post embedding a Jupyter notebook"
tags: [formatting, jupyter]
categories: [sample-posts]
giscus_comments: true
related_posts: false
---

To include a Jupyter notebook in a post, use:

{::nomarkdown}
{% assign jupyter_path = 'assets/jupyter/blog.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blog.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
  {% jupyter_notebook jupyter_path %}
{% else %}
  <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}