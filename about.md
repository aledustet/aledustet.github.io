---
layout: page
permalink: /about/index.html
title: Alejandro Dustet Mederos
tags: [aledustet, about]
---
<figure>
  <img src="{{ site.url }}/images/aledustet.png" alt="Alejandro Dustet Mederos">
  <figcaption>Alejandro Dustet Mederos</figcaption>
</figure>

{% assign total_words = 0 %}
{% assign total_readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% assign readtime = post_words | append: '.0' | divided_by:200 %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign total_readtime = total_readtime | plus: readtime %}
    {% if post.featured %}
    {% assign featuredcount = featuredcount | plus: 1 %}
    {% endif %}
{% endfor %}


My name is Alejandro Dustet Mederos, i am a developer.