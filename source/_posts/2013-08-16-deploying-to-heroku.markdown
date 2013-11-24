---
layout: post
title: "Deploying to Heroku"
date: 2013-08-16 18:16
comments: false
categories: [Heroku, Git]
---

<!-- more -->

[Heroku](http://www.heroku.com) is great for rapid prototyping and testing ideas. All you have to do is `git push heroku master` right? Wonderful.

However, sometimes deploying can be somewhat daunting, with so many errors messages to deal with. Below is my own Heroku deployment checklist:

Steps
------

1.  Create an App on Heroku dashboard eg. `boboobo` and select appropriate region.


2.  Copy Git URL in this case `git@heroku.com:boboobo.git` and run `git remote add heroku git@heroku.com:boboobo.git` in the App directory.


3.  After making changes in local directory, stage files, commit and then deploy code via `git push heroku master` or `git push heroku mod1:master` (when deploying from branch mod1)

After generating migrations, do the following to avoid any nasty errors

1.  `heroku run rake db:migrate`

2.  `heroku run rake db:reset` and `heroku run rake db:setup`

3.  `heroku restart`

4. Also do `heroku logs -t` and `heroku run console` to check server logs and db in console respectively.

After successfully deploying this app you'll end up with a nice url like http://boboobo.herokuapp.com
