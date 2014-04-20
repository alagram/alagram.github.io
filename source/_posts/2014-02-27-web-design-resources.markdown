---
layout: post
title: "Web Design Resources"
date: 2014-02-27 07:51
comments: true
categories: [Web Design, Rails]
---

<!-- more -->


Coming up with a good design for a web app can be a challenging task for back-end developers. What are some resources that can help a designed challenged developer get their first prototype out? In this article, we are going to look at some useful resources. We'll start by looking at using a front end framework, then widget libraries and customized themes, and finally how to hire a designer.

Build Your Prototype with Front End Frameworks
---------------------

Using a front end framework like Bootstrap or Foundation can greatly speed up the prototyping process to give you a clean looking user interface. They are pretty easy to get started, and give you a package of good typography, clean default styles, and some common components that you can easily drop into your app. You still have to understand the basics of front end development to blend them in your app's interface, but it generally is not too difficult.

####1) Bootstrap

If you have ever tried to put together your own design from scratch complete with CSS/Javascript while taking into account responsive design, you'll be amazed at this framework. By far the most popular front-end framwork, [Bootstrap](http://getbootstrap.com), offers little snippets of code that can to put some life into you website. I think the following make Twitter Bootstrap amazing:

-  Responsive design
-  JavaScript
-  Typography
-  Easy integration with most popular frameworks
-  Wide community adoption

Bootstrap is built with the LESS CSS preprocessor, but official Sass support has been added recently. To integrate Bootstrap into your Rails app, just drop the [bootstrap-sass](https://github.com/twbs/bootstrap-sass) gem in your Gemfile, bundle it, and you are ready to go.

####2) Foundation

Foundation is another front-end framework which offers quick application prototyping. It uses Sass as the default CSS preprocessor. It comes packed with buttons, navigation and other awesome plugins. Foundation has its own features including:

- Responsive design with a mobile-first grid system
- Better semantic markup
- Developed and maintained by Zurb, an active design agency

Wondering how to use it in a Rails app? There is a gem for that. There is a Railscast on Foundation [here](http://railscasts.com/episodes/417-foundation).

Although the Bootstrap vs Foundation arguments still linger, recent releases have normalized things somewhat. Overall, I've found that making a choice between these two frameworks is a matter of preference. Bootstrap is a "larger" framework and comes with more readily made components, aiming to help you "bootstrap" a prototype as fast as you can. Foundation, on the other hand, is more to give you a solid "foundation" that you can lay on top of with your own style. Ideally, you should give both frameworks a test drive to see which works for you.

Use a widget library, or customize themes
-----------------------------------------

If you use a front end framework, but don't want your app to look exactly like the other thousands that just stick with the framework's default style, you can consider using a widget library or buying a custom theme. Bootstrap shines here with its larger community - most of the widget libraries and themes you find will be based on Bootstrap.

- [WrapBootstrap](https://wrapbootstrap.com/): WrapBootstrap is a marketplace for premium Bootstrap based themes and templates. Using a template is awesome because it gives you the flexibily of picking from many nice designs. On the flip side, you might have to manually copy the templates assets and make them fit onto your app.

- [Designmodo's Flat UI](http://designmodo.com/flat/): Based on Bootstrap, [Designmodo's Flat UI](http://designmodo.com/flat/) offers all the elements in Bootstrap plus a few more. It offers color swatches and is extremely light and fast. It's also [easy](https://github.com/reflection/designmodo-flatuipro-rails) to integrate onto a Rails app. This is not an open-sourced framework.

- [Bootsnip](http://bootsnipp.com/): Bootsnip offers a library of code snippets for Bootstrap.

- [Divshot](http://www.divshot.com/): Based on Bootstrap, Divshot provides a way to design webpages by using a drag and drop interface. The workspace allows you to drop elements, customize and preview code. You can test on multiple device emulators, export code easily and have your site up quickly.

- [Jetstrap](https://jetstrap.com/): Another WYSIWYG Bootstrap page builder, similar to Divshot.

Hire Designers
------------------------

When you feel that you have validated your prototype and ready to seriously invest in the design and user experience, hiring a designer to work with you closely is an execellent choice. Experienced designers can help you with identifying your targeted audience, brand your product and tell a cohesive story with the combination of typography, colors, layout, logos and interaction.

Finding good designers can be a challenging task, but if you feel your product is ready for some professional touch, here are some places that you can hire designers:

- [99designs](http://99designs.com/): A graphic-design marketplace. Designers that work here are typically earlier in their career, but you do get multiple designers to work on your project by hosting a design contest, so you may be able to find some quality work. If you have a small job like designing a logo or a sales page, this can be the cost effective way of finding a good design fast.

- [oDesk](https://www.odesk.com/): oDesk is a global job marketplace with a massive pool of professionals ready to help you and your project. This is the place you'd go if you want to have someone that you can work with for an extented period of time but doesn't cost a fortune. You will find quite a lot of "designers" here, but be careful because people here could claim themselves as "designers" just because they can use photoshop. Look at their work history, feedback from previous employers and portfolio. If you see someone you like, request an interview to talk to them to make sure they communicate well. Start with a small project to see their work. The ideal candidate here for you would be talented designers who are just starting or in parts of the world with lower wages. If you can find your "diamond in the rough", keep them around and treat them well!

- [Dribbble](http://dribbble.com/): Finally, this is where professional designers hang out. It's more of a place where designers share their work with each other, instead of a "designers for hire" site, but some designers there are open to take clients. Use location (if you want to hire someone local) and specialties to narrow down your choices, and look at their past work to see if you can find a style that you'd like to see in your project. Make sure you contact people only if they are available for hire!

Conclusion
----------

We have discussed quite a few options here for you to consider. Ultimately, you should make your choice based on the phase of your project, your budget and your own skill set. If your project is on the prototype stage, using a front end framework to have a clean looking interface may be all you need. As your project matures and gains traction, that can be the time to start engaging professional designers.
