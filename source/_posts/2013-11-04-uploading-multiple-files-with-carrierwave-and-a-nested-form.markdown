---
layout: post
title: "Uploading multiple files with carrierwave using a nested form"
date: 2013-11-04 17:00
comments: false
categories: Rails
---

<!-- more -->


I recently needed to create a form for uploading multiple files to a given object. I implemented this with [carrierwave](https://github.com/carrierwaveuploader/carrierwave) and [jQuery File Upload](https://github.com/tors/jquery-fileupload-rails) gems.

Before we start, we need to make sure models and migrations are setup.
For images, we have the following migration:

    class CreateImages < ActiveRecord::Migration
      def change
        create_table :images do |t|
        t.string :image
        t.integer :fraud_id

        t.timestamps
      end
    end

I have the following classes:

    class Fraud < ActiveRecord::Base
      has_many :images
      accepts_nested_attributes_for :images
    end

    class Image < ActiveRecord::Base
      belongs_to :fraud
    end


Added gems to Gemfile:

    gem 'carrierwave'
    gem 'jquery-fileupload-rails'


Run ```bundle install``` to install gems and dependencies.


Then we create a carrierwave uploader with ```rails generate uploader image``` and mount the uploader:

    class Image < ActiveRecord::Base
      belongs_to :fraud

      mount_uploader :image, ImageUploader
    end

The carrierwave uploader helps by doing all the heavy lifting with regard to uploading the files.

Now we are ready to work on the view, where the form is modified in the following way:

    = render 'shared/errors', object: @fraud

    = form_for @fraud, html: { class: "form-horizontal", autocomplete: "off", multipart: true } do |f|
      %fieldset
        .control-group
          = f.label :title, class: 'control-label'
          .controls
            = f.text_field :title, class: 'span3'
        .control-group
          = f.label :description, class: 'control-label'
          .controls
            = f.text_area :description, rows: 6, class: 'span6'
        .control-group
          = f.label :fraud_date, class: 'control-label'
          .controls
            = f.text_field :fraud_date, class: 'span3 fraud_date'
        .control-group
          = f.fields_for :images, Image.new do |ff|
            = ff.label :image, "Upload Evidence", class: 'control-label'
            .controls
              = ff.file_field :image, multiple: true, class: 'btn btn-file', id: 'upload-image', name: "fraud[images_attributes][][image]"
      %fieldset.actions.control-group
        .controls
          = f.submit class: 'btn', id: 'submit-data'


The form setup is a typical form with a few details:
```= f.fields_for :images, Image.new do |ff|``` creates a nested form. ```accepts_nested_attributes_for``` allows the creation of a related object along with the main object. There is good documentation of this [here](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html#method-i-accepts_nested_attributes_for). So in this example, we are creating an instance of ```Fraud``` class while simultaneously creating an instance of an ```Image``` class.

Since by default, most browsers do not allow selection of multiple files, we use ```multiple: true``` to change this. Also, we have to change the ```name``` attribute of our nested form to make it work with carrierwave. The attribute finally looks like this:

    name: "fraud[images_attributes][][image]"

Lastly, we need our images to be uploaded on submit. jQuery File Upload gem by default tries to upload files immediately after its attached, we need to change this.

 We grab the id's of the file field and the submit button and use coffeescript to change this default behaviour to fit our needs:

    $ ->
      $("#upload-image").fileupload
      add: (e, data) ->
        data.context = $("#submit-data")
        data.submit()

That's all folks! With this, on submit, Rails knows to create an instance of Fraud and instances of Image. There is a natural association with Fraud and Image.