---
layout: post
title: "Fix Octopress rake new_post problem on Zsh"
date: 2013-08-15 16:35
comments: false
categories: [Octopress, Zsh]
---
<!-- more -->

Creating a post on [Octopress](http://octopress.org) uses a rake command like so:`rake new_post["My New Post"]`

However when you try to create a new post within zsh shell it doesn't work. 

    $ rake new_post["My New Post"]
    zsh: no matches found: new_post[My New Post]

After following worked for me after doing some googling.

##Solution
You can fix this problem by simply running ```rake new_post```

    rake new_post
    Enter a title for your post:


This will then prompt for title of post and then create it. Done!