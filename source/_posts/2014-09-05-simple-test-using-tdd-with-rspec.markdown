---
layout: post
title: "Simple Test using TDD with Rspec"
date: 2014-09-05 19:12
comments: true
categories: [Rspec, Rails, TDD]
---

<!-- more -->

## Introduction

Test Driven Development is great for driving out implementation of features in software development. In this post, I'm going to go through how I write tests using the TDD approach with [Rspec](https://github.com/rspec/rspec-rails) by going through the steps to drive out the implementation of simple search feature. We'll implement a method named `search_by_title`.

## How I Write Tests

  1) I start by thinking about all the test cases I want to cover. That is, thinking on a high level on what the functionality should achieve. This allows me to have more complete coverage of what I want my code to accomplish instead of diving straight into one test, making it pass and then figuring out what the next test should be.

  2) Setup data for tests, perform action and put an assertion on the result of the action.

  3) Run test and let it fail.

  4) Finally, take small steps to make the test pass. This is because in TDD you want to write the simplest code to make a test pass and also make sure that every piece of code you write is going to be the result of a failing test.

### The code

By following step 1, I write out all the test cases I want to cover:

```ruby
require "rails_helper"

RSpec.describe Video, :type => :model do
  it { should belong_to :category }
  it { should validate_presence_of :title }
  it { should validate_presence_of :description }

  describe "search_by_title" do
    it "returns an empty array if there is no match"
    it "returns an array of one video for an exact match"
    it "returns an array of one video for a partial match"
    it "returns an array of all matches ordered by created_at"
    it "returns an empty array for a search with an empty string"
  end
end
```

The video model in `app/models/video.rb` looks like this:

```ruby
class Video < ActiveRecord::Base
  belongs_to :category
  validates_presence_of :title, :description
end
```

Next, I'll set up the data I need for the first test, perform an action and make an assertion on the result; step 2. The code for the first test looks like this:

```ruby
it "returns an empty array if there is no match" do
  futurama = Video.create(title: "Futurama", description: "Space Travel!")
  back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
  expect(Video.search_by_title("hello")).to eq([])
end
```

Now I'll run the test and let it fail; step 3:

```bash
rspec spec/models/video_spec.rb
```

{% img /images/2014-09-05-simle-test-using-tdd-with-rspec/spec_1.png Spec 1 %}

Test is complaining about `NoMethodError: undefined method search_by_title for #<Class:0x007f9ec0ad0d98>`

Next, make that little change in `app/models/video.rb`; step 4:

```ruby
class Video < ActiveRecord::Base
  belongs_to :category

  validates_presence_of :title, :description

  def self.search_by_title
  end
end
```

Run test again:

```bash
rspec spec/models/video_spec.rb
```

{% img /images/2014-09-05-simle-test-using-tdd-with-rspec/spec_2.png Spec 2 %}

This time the failure is `ArgumentError: wrong number of arguments (1 for 0)`. In our test, we pass a parameter to the method and have to replicate that in the real code.

Let's make that change:

```ruby
class Video < ActiveRecord::Base

  ...

  def self.search_by_title(search_term)
  end
end
```

Run the test again:

```bash
rspec spec/models/video_spec.rb
```

{% img /images/2014-09-05-simle-test-using-tdd-with-rspec/spec_3.png Spec 3 %}

There is another error:

```bash
expected: []
got: nil
```

The easiest change to make the test pass is to return an empty array in the `search_by_title` method:

```ruby
class Video < ActiveRecord::Base

  ...

  def self.search_by_title(search_term)
    []
  end
end
```

At this point, if all all method does is to return an empty array this is all we need to satisfy that.

Now when we run the test again, we'll have a passing test:

{% img /images/2014-09-05-simle-test-using-tdd-with-rspec/spec_4.png Spec 4 %}


Fortunately, we have other test cases that we do want to satisfy and by following this simple process for each test, we're forced to drive-out the actually implementation of the method:

Our final code in `app/models/video.rb` will look like this:

```ruby
class Video < ActiveRecord::Base
  belongs_to :category

  validates_presence_of :title, :description

  def self.search_by_title(search_term)
    return [] if search_term.blank?
    where("title LIKE ?", "%#{search_term}%").order("created_at DESC")
  end
end
```

And our test code will be:

```ruby
require "rails_helper"

RSpec.describe Video, :type => :model do
  it { should belong_to :category }
  it { should validate_presence_of :title }
  it { should validate_presence_of :description }

  describe "search_by_title" do
    it "returns an empty array if there is no match" do
      futurama = Video.create(title: "Futurama", description: "Space Travel!")
      back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
      expect(Video.search_by_title("hello")).to eq([])
    end

    it "returns an array of one video for an exact match" do
      futurama = Video.create(title: "Futurama", description: "Space Travel!")
      back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
      expect(Video.search_by_title("Futurama")).to eq([futurama])
    end

    it "returns an array of one video for a partial match" do
      futurama = Video.create(title: "Futurama", description: "Space Travel!")
      back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
      expect(Video.search_by_title("urama")).to eq([futurama])
    end

    it "returns an array of all matches ordered by created_at" do
      futurama = Video.create(title: "Futurama", description: "Space Travel!", created_at: 1.day.ago)
      back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
      expect(Video.search_by_title("Futur")).to eq([back_to_future, futurama])
    end

    it "returns an empty array for a search with an empty string" do
      futurama = Video.create(title: "Futurama", description: "Space Travel!", created_at: 1.day.ago)
      back_to_future = Video.create(title: "Back to Future", description: "Time Travel")
      expect(Video.search_by_title("")).to eq([])
    end
  end
end
```

And all our tests pass:

{% img /images/2014-09-05-simle-test-using-tdd-with-rspec/spec_pass.png Spec Pass %}
