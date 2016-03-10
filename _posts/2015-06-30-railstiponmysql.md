---
layout: post
title: "Rails Tip On MySql in production"
permalink: blog/rails-tip-mysql-in-production
description:
headline:
modified: 2015-06-30 17:00:14 -0400
category: personal
tags: [rails, tip, mysql]
imagefeature:
mathjax:
chart:
comments: true
featured: false
---


Sometimes the difference between your app in production and your app in development is huge, there is a hole lot of information around on how to attack this, i am not doing another tutorial on it but i'll give away a simple tip about a problem on **decimal** data types on migrations and its repercusions on a **mysql** database.

When using an attribute on any of your rails models whose data type is a float number, make sure that the decimal points are specified right on the migration, issues on rounding the number are almost for sure unless specified. So if you are putting a price and your production database is mysql make sure you add it to the migration.

{% highlight ruby %}
  def change
    change_column :magazines, :price, :decimal, :precision => 8, :scale => 2
  end
{% endhighlight %}


Happy coding!!!
