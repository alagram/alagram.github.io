---
layout: post
title: "Full-Text Search on Hstore Column"
date: 2013-12-09 19:27
comments: true
categories: [Postgres, Hstore, Rails]
---

<!-- more -->

I've come to realise that PostgreSQL is awesome and comes with amazing in-built full-text search features. Another wonderful feature of Postgres is its Hstore schema less key value store. What this does is basically allow us to store data like hashes into a column.

With this knowledge in place, we can query data based on keys and values as shown [here](http://schneems.com/post/19298469372/you-got-nosql-in-my-postgres-using-hstore-in-rails). This is fine and great but how does one search by value regardless of key? Using the examples in the above link won't cut it. Lets assume we're working on a Product model with hstore properties column. After much frustration and searching, I found that to do full text search on hstore I could cast the properties column to text, but in my case I had to cast the values to text. Ended up with one kickasss search method:

{% codeblock lang:sql %}
def self.search(query)
  conditons = <<-EOS
    to_tsvector('english', CAST(avals(properties) AS text)) @@ plainto_tsquery('english', #{sanitize(query)})
  EOS

  where(conditons, query).order("created_at DESC")
end
{% endcodeblock %}

### Reference
*  http://terryrfinn.com/posts/1