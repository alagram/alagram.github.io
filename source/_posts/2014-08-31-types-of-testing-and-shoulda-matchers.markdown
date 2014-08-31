---
layout: post
title: "Types of Testing and shoulda-matchers"
date: 2014-08-31 11:11
comments: true
categories: TDD
---

<!-- more -->

## Introduction

When developing software, how do you know something works? Its certain that all software have bugs and the only way to make sure the code you've written is bug-free is by designing test cases to test against your codebase. It is the most objective way to say confidently that a feature works and things did not break. This post looks at types of testing and how to use [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) to test common Rails functionality.

### Types of testing

1) _**Unit Tests**_: Unit tests involve testing different components in isolation. You'll typically focus on one block at a time and make sure all cases are covered. In the Rails context, unit testing includes tests for models, views, helpers and routes in isolation. In sum, you're focusing on one thing at a time. Unit tests are the fastest in terms of speed and offer the best coverage.

2) _**Functional Tests**_: Functional tests involve testing multiple components together. This is another way of making sure different parts of a system work in tandem. In the Rails context, functional testing includes controller tests. Typically the controller is where you pull multiple models and make them work with eachother to generate data before passing them over to the views. You'll also be testing request and response cycles.

3) _**Integration Tests**_: This involves following a business process to make sure that not only do components work together, but can also work together to achieve a business objective. Integration testing involves mimicking the end-user by driving through the browser. You can login, fill a form, submit forms, click links and buttons etc. Integration tests are more realistic because you're following what a user of your system would do.


### shoulda-matcher

In Test Driven Development (TDD) you'll typically want to test your own code. [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) is a library that simplifies testing common Rails functionality like associations and validations. Suppose we have a `video` model with the following code:

```ruby
# video.rb
class Video < ActiveRecord::Base
  belongs_to :category

  validates_presence_of :title, :description
end
```

using `shoulda-matchers` we can write a few lines of code to test the functionality of our code:

```ruby
it { should belong_to :category }
it { should validate_presence_of :title }
it { should validate_presence_of :description }
```

These simple lines can take the place of all the following tests:

```ruby
it "belongs to a category" do
  cat = Category.create(name: "Action")
  vid = Video.create(title: "Yet another fake video", description: "Not sure...", category: cat)
  expect(vid.category).to eq(cat)
end

it "does not save video without a title" do
  video = Video.create(description: "I like this movie")
  expect(Video.count).to eq(0)
end

it "does not save a video without a description" do
  video = Video.create(title: "Robocop")
  expect(Video.count).to eq(0)
end
```
