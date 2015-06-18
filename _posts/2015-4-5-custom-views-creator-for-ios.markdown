---
layout: post
title:  "Custom views creation for iOS using a Rails app (Part I)"
permalink: custom-views-creator-for-ios-part-1
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
Let's begin to "objectify" the problem. Since the objective of this post is to build the actual thing to create the views and send them to the device for further rendering, i'll go ahead and expose my approach and i'll be glad to review it later with anyone kind enough to comment :). The views are a collection of sections they will be the base of the hierarchy a simple container of sections, the only relevant thing for me here is that a digital publication, can and almost always will have more than a single view, therefore the only attribute on the views could be the view index, meaning the order where the view will be displayed. So the sections are the base of a more complex hierarchy, because like that i can extract common attributes, as the section axis, height, width etc…, plus i can add more section types later(pretty much object oriented programming all the way), such as hyperlinks and hidden sections. So i have the head of my hierarchy on a "view" object, this object haves sections, and each section haves a type, something like this:

<img src="{{ site.url }}/images/objects.png" alt="search screenshot" style="width:600px;height:380px;">

Each one of the sections will have then his own type, such as an **Image Section**, or a **Text Section** or **whatever section** needs to be defined.

So the sections are like an encapsulation of the actual thing to render, and depending of the type of the section an actual useful object to render will be defined and will live persisted on the database.

Each of these objects will have different characteristics therefore each one needs a different definition,  meaning a different model in **rails** talk, for example **Image Sections** only haves the actual image to render, in my case, i will use [**carrierwave**](https://github.com/carrierwaveuploader/carrierwave) gem for uploading the actual image for later processing, any other tool for uploading in rails is just as [good](https://www.ruby-toolbox.com/categories/rails_file_uploads). The **Text Sections** are a different case, in this case at the very least the font, font-size and a color for the fonts are mandatory. An aspect that seems irrelevant but it is not, is when a section will be placed on the view, i mean, the order in wich a view will be displayed, so i went ahead and added a **section_index** property to the sections, so i can order the sections afterwards and gain control over when the sections are rendered.
The backend project needs to have all these definitions and a simple to use interface for creating those, with all this concepts in place it is possible to create the views, the interface will be an index of the views, and the classic **CRUD** operations to perform on the resources after that the place where the *magic* will happen, i mean where we will design our views needs to have some heavy javascript to *“draw”* the views on a canvas. The first part of the *backend* application we are going to finish it of on this post the first of three in total.

#### Doing something
Now the good part!!. We can create the rails project now, the full code can be found [here](repo), there you will find all the necessary migrations, and of course(this is my curse) some tests to add in the mix, i have change my tests methodology since i read [*dhh’s* article](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html), i will go through the process right now.

First thing first, the migrations:

1. All we need on the **views** table is actually the view_index attribute.
2. On the **sections** table we need all the common attributes to the sections, the order of creation of all the sections inside the views and of course the necessary reference to the parent view.
3. The **ImageSections** will of course have the reference to its section and since i am using carrier-wave for file uploading i am gonna need two fields, one for carrier-wave named **image** and other for the iOS App to load the image named **image_name**
4. The **TextSections** table will hold the font, the font-size, the content, the alignment will be an integer according to the text alignment property of text-boxes and labels on iOS.

So the migration looks like this:

{% highlight ruby %}
create_table :views do |t|
  t.integer :view_index

  t.timestamps null: false
end

create_table :sections do |t|
  t.belongs_to :view, index:true

  t.integer :section_index
  t.integer :x
  t.integer :y
  t.integer :width
  t.integer :height
  t.integer :section_type

  t.timestamps null: false
end

create_table :image_sections do |t|
  t.belongs_to :section, index: true

  t.string :image_name
  t.string :image

  t.timestamps null: false
end

create_table :text_sections do |t|
  t.belongs_to :section, index: true

  t.text :content
  t.string :font
  t.integer :font_size
  t.integer :red
  t.integer :green
  t.integer :blue
  t.integer :alignment
  t.boolean :scrollable

  t.timestamps null: false
end
{% endhighlight %}

So now that all the definitions are on the right place, usually i begin realizing that all the validations and tests are missing  i go test first on the models definitions not so much when i think about the system itself i go from feature, to better feature to test and make it even better. There is nothing special to it, just some plain old tests, i will go through the suit of tests.

###### Tests
First of all i wanna point out that i am musing **mini_test** and **fixtures** right from the rails stack. I will go through the reasoning of my test suite and the models definitions, i will add some code samples, but the complete code is on the [github project](repo).

With the views the need was simple i just needed to access the view’s sections and make sure that the index of the section is a positive number. The fixture is simpler, just add a random positive number. To satisfy this tests the model will reflect a **has_many** relationship with the **sections** models and will validate numerically that the **view_index** field must be greater than or equal to 0.

On the sections, to make sure that the data was sanitized, tests for the presence of mandatory attributes are present, and for the minimum values of axis, width, height and the other numeric values. And of course access to the **sections** real underlying object definition, in the case of an **image_section** an object on the **image_sections** table pointing at his section parent, so it can be retrieved with the **has_one** relation on the **section** model definition. Since a section is pointless without a *minion* object the fixtures reflects it:


{% highlight xml %}
section_for_image:
  section_index: <%= rand(1..10) %>
  x: <%= rand(0..320) %>
  y: <%= rand(0..480) %>
  width: <%= rand(0..320) %>
  height: <%= rand(0..480) %>
  section_type: 0
  view: :view

section_for_text:
  section_index: <%= rand(1..10) %>
  x: <%= rand(0..320) %>
  y: <%= rand(0..480) %>
  width: <%= rand(0..320) %>
  height: <%= rand(0..480) %>
  section_type: 1
  view: :view
{% endhighlight %}

On the tests all the validations are tested and also the relations:

{% highlight ruby %}
#TESTS

def setup
  @section_image = sections(:section_for_image)
  @section_text = sections(:section_for_text)
end

test "haves a valid factory" do
  assert @section_text.valid?
  assert @section_image.valid?
end
{…}
test "a minion object exists on the right table" do
  text_section = TextSection.find_by_section_id(@section_text.id)
  image_section = ImageSection.find_by_section_id(@section_text.id)
  assert (!text_section.nil? and image_section.nil?)
end

#model

class Section < ActiveRecord::Base
  has_many :image_sections
  has_many :text_sections
  belongs_to :view
  validates :x, :y, :width, :height, :section_type, :view, presence: true
  validates :x, :y, :section_index, :section_type, numericality: { only_integer: true, greater_than_or_equal_to: 0 }
  validates :width, :height, numericality: { only_integer: true, greater_than: 0 }
end
{% endhighlight %}

The **ImageSection** model only needs the string field for **carrierwave**, the name of the image for the iOS and the reference for the parent object. The tests are for the presence of this fields. The **TextSection** model haves a few more validations, since there are more fields to look after, for example the max and min values of the three fields for the rgb colors **red**, **green** and **blue**, also the text-alignment and the font-size and no duplication on the parent **section** object. Also a high number of times the default font color remains the same also the size and the alignment so an init method was added on the model for easy creation:

{% highlight ruby %}
#TESTS

def setup
  @text_section = text_sections(:text_section)
end

{...}

test "correctly validates colors" do
  prev = @text_section.section
  @text_section.red = 256
  assert_not @text_section.valid?
  @text_section.section = prev
  prev = @text_section.content
  @text_section.blue = -1
  assert_not @text_section.valid?
end

test "correctly validates font_size" do
  @text_section.font_size = 0
  assert_not @text_section.valid?
end

test "correctly validates alignment" do
  @text_section.alignment = -1
  assert_not @text_section.valid?
end

test "doesn't allow duplicate section values" do
  text_sec = text_sections(:other_text_section)
  text_sec.section = @text_section.section
  assert_not text_sec.valid?
end

test "initializes the omited values with defaults" do
  text_section = text_sections(:uninitialized_text_section)
  assert (text_section.font_size == 12)
end

{…}
test "a minion object exists on the right table" do
  text_section = TextSection.find_by_section_id(@section_text.id)
  image_section = ImageSection.find_by_section_id(@section_text.id)
  assert (!text_section.nil? and image_section.nil?)
end

#model

class TextSection < ActiveRecord::Base
  belongs_to :section
  validates :section, :content, presence: true
  validates :section, uniqueness: true
  validates :red, :green, :blue, numericality: { only_integer: true, greater_than_or_equal_to: 0, less_than_or_equal_to: 255 }
  validates :font_size, numericality: { only_integer: true, greater_than_or_equal_to: 1}
  validates :alignment, numericality: { only_integer: true, greater_than_or_equal_to: 0}

  after_initialize :init

  def init
    self.font  ||= "Helvetica"
    self.font_size ||= 12
    self.red ||= 0
    self.green ||= 0
    self.blue ||= 0
    self.alignment ||= 0
    self.scrollable ||= false
  end
end
{% endhighlight %}

That is pretty much it on the models. The models holds simple but complete definitions for the problem intended to solve.

###### Doing something else
Now the interface part is left, on the fronted side of the application the heavy-lifting will be done so it will be left for the second part of this series, where an **easy to use interface to create the views** will be made, yeah of course, easy to use but not that easy to implement. Well the easiest thing for us to draw around and update things on a webpage is of course a canvas, so the trick will be to simulate an iOS device screen and on the canvas draw the items, but remains the not so easy task of saving the canvas state, drawing the images and texts and the line changing text(OUCH!!). Hope this posts clarifies the problem, more on this on following articles. Keep coding strong.


[repo]:      https://github.com/aledustet/views-creator-backend
