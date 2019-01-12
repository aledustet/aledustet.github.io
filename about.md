---
layout: page
permalink: /about/index.html
title:
tags: [aledustet, about]
---

{% assign total_words = 0 %}
{% assign total_readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% assign readtime = post_words | append: '.0' | divided_by:200 %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign total_readtime = total_readtime | plus: readtime %}
{% endfor %}


My name is **Alejandro Dustet Mederos**, and this is my website. I am lucky enough to have found what I like doing early on in my career. Software makes for an essential part in my life, and I would love to share what I unravel about it more often through this medium. Always looking to learn, and trying to put out there all my experience so I can help others just like so many others helped me. I do not have strong opinions, but I do have my ideas, and I believe it is essential to listen to each person point of view. I do not think there's a perfect solution, but I do believe in finding the best one given the requirements, also for me the best validation of an idea is having someone external, but looking for a solution to the problem in question poking around. So what can be a better way to validate your ideas than putting them in the open.

The blog currently has {{ site.posts | size }} posts in {{ site.categories | size }} categories which combinedly have {{ total_words }} words, which will take an average reader ({{ site.wpm }} WPM) approximately <span class="time">{{ total_readtime }}</span> minutes to read. The most recent post is {% for post in site.posts limit:1 %}{% if post.description %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}">"{{ post.title }}"</a>{% else %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}" title="Read more about {{ post.title }}">"{{ post.title }}"</a>{% endif %}{% endfor %} which was published on {% for post in site.posts limit:1 %}{% assign modifiedtime = post.modified | date: "%Y%m%d" %}{% assign posttime = post.date | date: "%Y%m%d" %}<time datetime="{{ post.date | date_to_xmlschema }}" class="post-time">{{ post.date | date: "%d %b %Y" }}</time>{% if post.modified %}{% if modifiedtime != posttime %} and last modified on <time datetime="{{ post.modified | date: "%Y-%m-%d" }}" itemprop="dateModified">{{ post.modified | date: "%d %b %Y" }}</time>{% endif %}{% endif %}{% endfor %}.

I want to thank [Hossain Mohd. Faysal](https://twitter.com/hmfaysal) for creating an awesome [template](http://hmfaysal.github.io/Notepad/) for jekyll.
