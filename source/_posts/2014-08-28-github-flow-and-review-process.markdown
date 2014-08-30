---
layout: post
title: "Github Flow and Review Process"
date: 2014-08-28 09:33
comments: true
categories:
---

<!-- more -->

[Git](http://git-scm.com/) is an awesome version control system. For the uninitiated it might seem daunting at first, but with time it becomes indispensable. When you're working on a project by yourself, it's easy to keep track of things on [Github](https://github.com/). However, with time issues start cropping up when your project grows and you have a team of developers. Teams in such situations need an efficient way to work together. This aticle on [Github Flow](http://scottchacon.com/2011/08/31/github-flow.html) is a great read on the topic. It describes, in my opinion, an organized workflow that allows for systematic collaboration and code review on projects. Here are my notes from the article:

### 1. Anything on `master` can be deployed

If anything is on the `master` branch, it's ready to be deployed. This means anything on `master` branch should be thoroughly tested and is ready to go to production.

### 2. Create descriptively named local branch off `master`.

When working on a new feature, one has to create a descriptively named local branch off `master`. People usually refer to this branch as a feature branch. Say you're working on a new feature for authentication, you'll typically do something like so:

```bash
# make sure you're on your master branch
git checkout -b authentication
```

The above is the same as running:

```bash
git branch authentication
git checkout authentication
```

Using the `-b` option with `git checkout` creates the local branch and switches to it at the same time.

### 3. Push code up

You'll work on this new feature branch, commit to the branch locally and regularly push to the same remote branch on your server. When you're ready to push up to your remote server, you'll enter this command on your terminal:

```bash
git push origin authentication
```

This will create a new remote branch named `authentication` on the remote server.


### 4. Create a pull request

When you need feedback or assitance, or believe you work on the new feature is complete, open a [pull request](https://help.github.com/articles/creating-a-pull-request). You'll need to create this `pull request` from your feature branch to your `master` branch. When you're done, collaborators can review your commits.


### 5. Code review and merge

After all reviews have been made and changes effected, you can merge it into your `master` branch by going to your `pull request` and clicking the `merge` buton to `merge` it back to `master` branch.

Note that, while the `pull request` is open, you can still `commit` and `push` code up to your remote feature branch. This means any correction can be made while the `pull request` is open. For example:

```bash
git add your_filename.rb
git commit -m "added extra validations"
git push origin authentication
```

Continue until the process is complete.


### 6. Pull changes

Now on your terminal, you'll switch to your `master` branch:

```bash
git checkout master
```

Next, you `pull` the merged code on [Github](https://github.com/) to your local `master` branch:

```bash
git pull origin master
```

### 7. Deploy
Once it's merged to `master`, you can deploy.
