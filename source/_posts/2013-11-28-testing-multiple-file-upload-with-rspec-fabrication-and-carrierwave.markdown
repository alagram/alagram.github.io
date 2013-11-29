---
layout: post
title: "Testing multiple file uploads with Rspec, Fabrication and carrierwave"
date: 2013-11-28 20:26
comments: false
categories: [Fabrication, Rspec, carrierwave]
---

<!-- more -->

I ran into some problems when testing controller code with Rpsec while generating objects with fabrication. Most tests failed after I set validation on an Image object. 

My setup:

{% codeblock lang:ruby %}
class Expense < ActiveRecord::Base
  has_many :images
  accepts_nested_attributes_for :images, :reject_if => proc { |attr| attr['image'].blank? }

  validate :must_have_image

  private

  def must_have_image
    errors[:base] << "Please upload image" if images_empty?
  end

  def images_empty?
    images.empty?
  end
end

{% endcodeblock %}

{% codeblock lang:ruby %}
class Image < ActiveRecord::Base
  belongs_to :expense

  mount_uploader :image, ImageUploader
end
{% endcodeblock %}

With this, create an Image object first as shown [here](https://github.com/carrierwaveuploader/carrierwave/wiki/How-to%3A-Use-test-fixtures):

{% codeblock lang:ruby %}
Fabricator(:image) do
  file {
    ActionDispatch::Http::UploadedFile.new(
      :tempfile => File.new(Rails.root.join("spec/support/uploads/file.jpg")),
      :type => 'image/jpg',
      :filename => File.basename(File.new(Rails.root.join("spec/support/uploads/file.jpg")))
    )
  }
end
{% endcodeblock %}

Then comes the tricky part of creating an ```Expense``` object. Adding this line ```images_attributes { [Fabricate.attributes_for(:image)] }``` to the Fabricator did the magic. Here's my final code:
{% codeblock lang:ruby %}
Fabricator(:expense) do
  title { Faker::Lorem.words(2).join }
  description { Faker::Lorem.paragraph(2) }
  exp_date Time.now.strftime("%Y-%m-%d")
  images_attributes { [Fabricate.attributes_for(:image)] }
end
{% endcodeblock %}

There is another way to do this through the controller spec without adding the last line in the Fabricator above:

{% codeblock lang:ruby %}
describe "POST create" do
  it "creates a new expense" do
    exp_params = Fabricate.attributes_for(:expense)
    img_params = Fabricate.attributes_for(:image)
    post :create, expense: exp_params.merge(images_attributes: [img_params])
    expect(Expense.count).to eq(1)
  end
end
{% endcodeblock %}