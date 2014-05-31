---
title: Rails Girls Berlin
layout: default
---

# App Tutorial

{% include common_start.md %}

# 2. Create Idea scaffold

We’re going to use Rails’ scaffold functionality to generate a starting point that allows us to list, add, remove, edit, and view things; in our case ideas.

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: What is Rails scaffolding? (Explain the command, the model name and related database table, naming conventions, attributes and types, etc.) What are migrations and why do you need them?</span>

    rails generate scaffold idea name:string description:text picture:string
    rake db:migrate
    rails server

Open `http://localhost:3000/ideas` in your browser. Click around and test what you got by running these few command-line commands.

Hit `CTRL-C` to quit the server again when you’ve clicked around a little.

# 3. Design

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: Talk about the relationship between HTML and Rails.
What part of views is HTML and what is Embedded Ruby (ERB)? What is MVC and how does this relate to it? 
(Models and controllers are responsible for generating the HTML views.)</span>

The app doesn’t look very nice yet. Let’s do something about that. We’ll use the Twitter Bootstrap project to give us nicer styling really easily.

Open `app/views/layouts/application.html.erb` in your text editor and above the line

    <%= stylesheet_link_tag "application", media: "all", "data-turbolinks-track" => true %>

add

    <link rel="stylesheet" href="http://railsgirls.com/assets/bootstrap.css">

and replace

    <%= yield %>

with

    <div class="container">
      <%= yield %>
    </div>

Let’s also add a navigation bar and footer to the layout. In the same file, under `<body>` add

    <div class="navbar navbar-fixed-top">
      <div class="navbar-inner">
        <div class="container">
          <a class="brand" href="/">The Idea app</a>
          <ul class="nav">
            <li class="active"><a href="/ideas">Ideas</a></li>
          </ul>
        </div>
      </div>
    </div>

and before `</body>` add

    <footer>
      <div class="container">
        Rails Girls 2013
      </div>
    </footer>

Now let’s also change the styling of the ideas table. Open `app/assets/stylesheets/application.css` and at the bottom add

    body { padding-top: 100px; }
    footer { margin-top: 100px; }
    table, td, th { vertical-align: middle !important; border: none !important; }
    th { border-bottom: 1px solid #DDD !important; }

Now make sure you saved your files and refresh the browser to see what was changed. You can also change the HTML & CSS further.

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: Talk a little about CSS and layouts.</span>

# 4. Adding picture uploads

We need to install a piece of software to let us upload files in Rails.

Open `Gemfile` in the project directory using your text editor and under the line

    gem 'sqlite3'

add

    gem 'carrierwave'

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: Explain what libraries are and why they are useful. Describe what open source software is.</span>

In the terminal run:

    bundle

Now we can generate the code for handling uploads. In the terminal run:

    rails generate uploader Picture

At this point you need to restart the Rails server process in the terminal.

Hit **CTRL-C** in the terminal to quit the server. Once it has stopped, you can press the up arrow to get to the last command entered, then hit enter to start the server again.

This is needed for the app to load the added library.

Open `app/models/idea.rb` and under the line

    class Idea < ActiveRecord::Base

add

    mount_uploader :picture, PictureUploader

Open `app/views/ideas/_form.html.erb` and change

    <%= f.text_field :picture %>

to

    <%= f.file_field :picture %>

In your browser, add new idea with a picture. When you upload a picture it doesn’t look nice because it only shows a path to the file, so let’s fix that.

Open `app/views/ideas/show.html.erb` and change

    <%= @idea.picture %>

to

    <%= image_tag(@idea.picture_url, :width => 600) if @idea.picture.present? %>

Now refresh your browser to see what changed.

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: Talk a little about HTML.</span>

# 5. Finetune the routes

If you try to open `http://localhost:3000` it still shows the “Welcome aboard” page. Let’s make it redirect to the ideas page.

Open config/routes.rb and after the first line add

    root :to => redirect('/ideas')

Delete the file `index.html` from the `public/` folder.

Test the change by opening the root path (that is, http://localhost:3000/) in your browser.

<span class="lead coach"><i class="icon-comment-alt"> Coach</i>: Talk about routes, 
and include details on the order of routes and their relation to static files.</span>

# What next?

- Add design using HTML &amp; CSS
- Add ratings
- Use CoffeeScript (or JavaScript) to add interaction
- Add picture resizing to make loading the pictures faster

