---
layout: post
title: "Seeding Data in Rails"
date: 2014-08-27 16:43
comments: true
categories: Rails
---

<!-- more -->

Imagine you've finished building a feature with tests and want to play with it in the UI. You'll have to find a way to get data into your development database somehow. There are two ways of going about this:

1. Fire up `rails console` and manually create all the data needed. This is fine and works when you want to create one-off data entries. Overtime you'll find that things become cumbersome when you need to add more data. What about when you want to fill up a development database over a network? Or when you accidentally lose all your development data and need to repopulate? Things can get out of hand quickly.

2. Populate your `seeds.rb` with all the data you need. This approach is your sure bet to get default values in your database in less time and is stress-free.

Example seed file:

```ruby
Video.create(title: "South Park", description: "A very funny video", small_cover_url: "/tmp/south_park.jpg", large_cover_url: "/tmp/monk_large.jpg")
Video.create(title: "Futurama", description: "I like this one too...", small_cover_url: "/tmp/futurama.jpg", large_cover_url: "/tmp/monk_large.jpg")
Video.create(title: "Monk", description: "Not bad. I'll watch this again.", small_cover_url: "/tmp/monk.jpg", large_cover_url: "/tmp/monk_large.jpg")
```

After filling in all data you want to be created, you'll simply run the following command in your terminal:

```bash
rake db:seed
```

That its all it takes. When things get out of hand, you can start on a clean slate by running this in your terminal:

```bash
rake db:drop
rake db:create
rake db:migrate
rake db:seed
```
