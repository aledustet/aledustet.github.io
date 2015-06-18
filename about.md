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
{% endfor %}


My name is **Alejandro Dustet Mederos**, and this is my personal blog. I am a developer, i have a major in computer science. Fell in love with programming, and thus i wanted to program in the right way, so i met [Ruby on Rails](http://rubyonrails.org), and my love grew bigger, [iOs](http://developer.apple.com) came into my life and it has been my other love. I am allways looking to learn, and i want to put out there my experience, to help others just like so many others helped me. This blog is about pretty much everything i do is the pipeline of knowledge that flows from me and i feel the need to put it somewhere, to give back all i gain from the developers community over the years i have been a part of it, to serve as a reflex of who i am as a developer and to do something i love, hence this place. If anyone learns something from this place, then my goal will be met. I hope everyone enjoys it as much as i did.

The blog currently has {{ site.posts | size }} posts in {{ site.categories | size }} categories which combinedly have {{ total_words }} words, which will take an average reader ({{ site.wpm }} WPM) approximately <span class="time">{{ total_readtime }}</span> minutes to read. The most recent post is {% for post in site.posts limit:1 %}{% if post.description %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}">"{{ post.title }}"</a>{% else %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}" title="Read more about {{ post.title }}">"{{ post.title }}"</a>{% endif %}{% endfor %} which was published on {% for post in site.posts limit:1 %}{% assign modifiedtime = post.modified | date: "%Y%m%d" %}{% assign posttime = post.date | date: "%Y%m%d" %}<time datetime="{{ post.date | date_to_xmlschema }}" class="post-time">{{ post.date | date: "%d %b %Y" }}</time>{% if post.modified %}{% if modifiedtime != posttime %} and last modified on <time datetime="{{ post.modified | date: "%Y-%m-%d" }}" itemprop="dateModified">{{ post.modified | date: "%d %b %Y" }}</time>{% endif %}{% endif %}{% endfor %}. The last commit was on {{ site.time | date: "%A, %d %b %Y" }} at {{ site.time | date: "%I:%M %p" }} [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time "Temps Universel Coordonné").

I want to thank [Hossain Mohd. Faysal](https://twitter.com/hmfaysal) for creating an awesome [template](http://hmfaysal.github.io/Notepad/) for jekyll.