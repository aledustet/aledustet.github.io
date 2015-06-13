---
layout: post
title:  "Custom views creation for iOS using a Rails app (Part I)"
date:   2015-4-5 16:00:25
category: Development
tags: [rails, iOS, Javascript]
comments: true
imagefeature: computer_stare.png
---

In the case you need an app that displays dynamic content and when the team “reviews” the “views” you generated on your iOS app fetching the approved content for the app, using the agreed template and the right data, and then "the picture is ...", "the text looks …”, “is not where it is supposed to …”. Well i wish everyone could arrange it the right way, so we don’t have to reship the hole project after every review but a crash course of "Storyboard" and “Xcode” wont do any good, so i went and built an easy to use interface where we all could create the views using pictures and text boxes, and then i could make it look exactly how it looked. So i went on and made it, here is a bit of my thoughts and code.

## The Problem

I needed to create custom views for an iOS app and serve them as dynamic content to be displayed.
So what we need to accomplish here is some sort of logic to create the view and it's components, an easy to use interface to create them and some way to render all that data on the iOS app. We will go trough all of those steps in a series of posts(not sure how long they will take). I will try to be concise, cause i have a tendency to loose interest on huge blog posts (they sometimes are needed and have a point [Design Patterns in Swift](http://www.raywenderlich.com/86477/introducing-ios-design-patterns-in-swift-part-1) there is a good example).

## The "Solution"
I double quoted solution because i don’t feel this like being the ultimate path for attacking this predicament. I will simply present my approach and i hope to get some feedback to improve it :).


#### A Checklist
Lets begin with a checklist of all the things i’m looking to accomplish in order to build an easy to use webapp for creating views for an iOS device, and of course actually displaying the views on the device.

- Create the logic behind the rails app, to model the problem.
- Create an easy to use tool to visually create the views.
- Export the data in a simple format to display it on the device.
- Sit back and watch the happy faces when we can actually make the content view how it looked.

#### Defining some stuffs
Let's begin to "objectify" the problem. Since the objective of this post is to build the actual thing to create the views and send them to the device for further rendering, i'll go ahead and expose my approach and i'll be glad to review it later with anyone kind enough to comment :). So i see the views as a collection of parts, because like that i can extract common attributes, as the views axis, height, width etc…. Plus i can add more view types later(pretty much object oriented programming all the way), such as hyperlinks and hidden views. So i have the head of my hierarchy on a "view" object, this object haves sections, and each section haves a type, something like this:

<img src="{{ site.url }}/images/objects.png" alt="search screenshot" style="width:600px;height:380px;">

Each one of the sections will have then his own type, such as an **Image Section**, or a **Text Section** or **whatever section** needs to be defined, right now, Image Sections only haves the actual image to render, in my case, i am using [**carrierwave**](https://github.com/carrierwaveuploader/carrierwave) gem for uploading the actual image for later processing, any other tool for uploading in rails is [fine](https://www.ruby-toolbox.com/categories/rails_file_uploads). The **Text Sections** are a different case, in this case at the very least the font, font-size and font-color are mandatory. An aspect that seems irrelevant but it is not, is when a section will be placed on the view, i mean, the order in wich a view will be displayed, so i went ahead and added a **section_index** property to the sections, so i can order the sections afterwards and gain control over when the sections are rendered.

#### Doing something
Since we will be using a rails project you can go ahead and create it and add all that virtual and non concise definitions that i just throwed at your faces or you can download the nutshell of the project [here][repo], there you will find all the necessary migrations:



{% highlight ruby %}
#views objects migrations
def func
  puts "PILL"
end
#other migrations code

{% endhighlight %}

So now that all the foundations are on the right place, the part when we make an **easy to use interface to create the views**, yeah of course, easy to use but not that easy to implement. Well the easiest thing for us to draw around and update things on a webpage if of course a canvas, so the trick will be to simulate an iOS device screen and on the canvas we can draw the items , but remains the not so easy task of saving the canvas state, drawing the rif


[repo]:      https://github.com/aledustet/views-creator-backend
