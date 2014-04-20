---
layout: post
title: "Set Up Staging Environment for an App"
date: 2014-04-19 10:21
comments: true
categories: [Rails, Heroku]
---

<!-- more -->

Rails apps usually consist of test, development and production environments. We usually write test code to drive out implementation and make sure things work. However, you generally want to make sure that changes from developemt don't break things in production. For instance, developing on a Windows or Mac machine provides few guarantees that what works on development will work as you expect after deploying to production. This type of requirement calls for another eenvironment called staging. This environment should closely mirrow production. This blog post goes through how to set up a staging environment with a minimum of fuss.

With App Not Deployed to Production
---------------

If your app has not been pushed to production, i.e , it's sitting on your development machine, you can start at the top of this [document](https://devcenter.heroku.com/articles/multiple-environments) and follow the simple steps to set up `staging` and `production` remote environments.

With App Already Deployed to Production
---------------

Alternatively, if your app is already in production, you need to create a copy from the production environment by using the `heroku fork` command. This [command](https://devcenter.heroku.com/articles/fork-app) also copies add-ons, config vars and Heroku Potgres data. Suppose our production app is `googleclone`, we'll run

    heroku fork -a googleclone googleclone-staging

This will create a copy of googleclone called googleclone-clone

    Creating fork googleclone-staging... done
    Copying slug... done
    Adding heroku-postgresql:some-dev... done
    Adding pgbackups:plus to googleclone... done
    Adding pgbackups:plus to googleclone-staging... done
    Transferring database (this can take some time)...  done
    Copying config vars... done
    Fork complete, view it at http://googleclone-staging.herokuapp.com/

With this taken care of, we'll use the [paratropper gem](https://github.com/mattpolito/paratrooper) which provides simple rake tasks and other goodies for Heroku deployment. After the gem is installed, we'll need to create a simple `deploy.rake` file under `lib/tasks`

{% codeblock lang:ruby %}

# lib/tasks/deploy.rake

require 'paratrooper'

namespace :deploy do
 desc 'Deploy app in staging environment'
 task :staging do
   deployment = Paratrooper::Deploy.new("googleclone-staging", tag: 'staging')

   deployment.deploy
 end

 desc 'Deploy app in production environment'
 task :production do
   deployment = Paratrooper::Deploy.new("googleclone") do |deploy|
     deploy.tag              = 'production',
     deploy.match_tag        = 'staging'
   end

   deployment.deploy
 end
end

{% endcodeblock %}

Now we can call  `rake deploy:staging` and `rake deploy:production` to deploy to `staging` and `production` environments respectively.

I hope you found this useful.


