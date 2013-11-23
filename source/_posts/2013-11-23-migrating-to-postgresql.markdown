---
layout: post
title: "Migrating to PostgreSQL"
date: 2013-11-23 18:53
comments: true
categories: [Rails, PostgreSQL]
---

<!-- more -->

In my recent project, I had to serialize a Hash into my SQLite database. Then I realised the need to search on that column. Upon doing some research, I found that searching a serialized hash column is very inefficient. There was a better way of going about this: using PostgreSQL Hstore! What's a hacker to do? I quickly started to migrate my SQLite database to PostgreSQL.


Found this [Railscast](http://railscasts.com/episodes/342-migrating-to-postgresql) which really helped. But there are a few nasty suprises which I'd like to put out here for anyone going through this.

The real problem starts during the process of pulling data from SQLite database with [Taps](https://github.com/ricardochimal/taps). This gem provides a ```taps``` command that will help serve the one database and also pull data from it into another. First we have to serve our current SQLite database by passing ```taps``` a path to the database and also set a username and password. Then we can pull the data from this database into our Postgres database.


However after running this command I got an error that read something like this:

    taps cannot load such file -- sqlite3 (LoadError)

Huh? After hours of frustration I found that ```Taps``` depends on ```rack``` version ```1.0.1```.


## Solution

Added the this to ```Gemfile```

{% codeblock Gemfile %}
'rack',’1.0.1’
{% endcodeblock %}

Then on Terminal run this...

{% codeblock Terminal %}
bundle update rack
{% endcodeblock %}

```Taps``` will now successfully pull data from SQLite into Postgres.

