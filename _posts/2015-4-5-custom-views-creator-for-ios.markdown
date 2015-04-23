---
layout: post
title:  "Custom views creation for iOS using a Rails app I"
date:   2015-4-5 16:00:25
categories: rails iOS Javascript
comments: true
---

Sometimes your client sees the views you generated on your iOS app fetching the contentthe client provides for the app, using a template he agreed on, and the data he said was the right one, and he goes "I don't like where the picture is ... it's not on the right spot", "I don't like how the text looks ..." [image-of-troll] and then you start a useless explanation about that it is a template that is data-agnostic and just for one picture that i quote he does not finds on "the spot" the hole approach shouldn't be tossed away. Well, after all that there you go again to figure out how to detect that single case and to make it "on the spot" to the client, i don't know about you but that is a common scenario for me and i would like to answer this: "WELL GO AND DO THE VIEW YOURSELF", and right there, the solution was laying in front of me: "Go and do it by yourself". But a crash course of "Storyboard" and Xcode wasn't gonna quite do the trick, so i went and built a simpler interface where he could create the view using pictures and textboxes, and then i would made it look exactly how he created it. So i went on and made it, but my revenge wasn't complete until i posted the solution [funnny-face].

### The Problem

The need to create custom views for iOS but to serve them as dynamic content to be displayed in the app. So what we need to acomplish here is some sort of logic to create the view and it's components, an easy to use interface to create them and some way to render all that data on the iOS app. We will go trough all of those steps in a series of posts(not sure how long they will take). I will try to be concise, cause i have a tendency to loose interest on huge blog posts(they sometimes are needed and have a point[FIND A LINK TO A LARGE BUT COOOL BLOG POST]).

### The "Solution"
I double quoted solution because i dont feel this like being the ultimate path for attacking this predicament. I will simply present my approach and i hope to get some feedback to improve it :).


### A Checklist
Lets begin with a checklist of all the things i am looking to accomplish in order to build an easy to use webapp for creating views for an iOS device, and of course actually displaying the views on the device.

- Create the logic behind the rails app, to model the problem.
- Create the tool to visually and simply generate the views
- Export the data in a simple format to display it on the device
- Sit back and watch the clients happy faces when they can actually create the views they want

### Well lets define some stuffs
Let's begin to "objectify" the problem. Since the objective of this post is to build the actual thing to create the views and send them to the device for further rendering, i'll go ahead and expose my approach and i'll be glad to review it later with anyone kind enough to comment :). So i see the views as a collection of parts, because like that i can extract common attributes to the part, as its axis, height, width and other stuffs, and i can add more types of view parts later, such as hyperlinks, hidden views, etc. So i have the head of my hierarchy on a "view" object, that haves sections, and each section haves a type, something like this:


[IMAGE OF OBJECT HIERARCHY]

Each one of the sections will have then his own type, such as an **Image Section**, or a Text Section or whatever section you want to define, right now, Image Sections only haves the actual image to render, in my case, i am using **carrierwave** gem for uploading the files and have the actual image for later processing, you can use whatever variant you find suitable for your case. The **Text Sections** are a different case, here we need the font, font-size, font-color and all the things you may need to display text on the actual ios app, like text-alignment and wheter if the text is scrollable or not. An aspect that seems irrelevant but it is not, is when a section will be placed on the view, i mean, the order in wich a view will be displayed, so i went ahead and added a **section_index** property to the sections, so i can order the sections afterwards and gain control over when the sections are rendered.

### Well lets actually do something
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
