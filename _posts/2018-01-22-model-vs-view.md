---
layout: post
title: Model vs View
date: 2018-01-22 15:34:42.000000000 +00:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '13901538434'
  timeline_notification: '1516635283'
  _thumbnail_id: '351'
  _g_feedback_shortcode_64e4f70a2e37d7faa9201e2253d7d943961e32f3: '[contact-field
    label="Name" type="name" required="1" /][contact-field label="Email" type="email"
    required="1" /][contact-field label="Comment" type="textarea" required="1" /]'
  _g_feedback_shortcode_atts_64e4f70a2e37d7faa9201e2253d7d943961e32f3: a:9:{s:2:"to";s:30:"acf+wordpress@alancfrancis.com";s:7:"subject";s:28:"[Alan
    Francis] Model vs View";s:12:"show_subject";s:2:"no";s:6:"widget";i:0;s:2:"id";i:349;s:18:"submit_button_text";s:6:"Submit";s:14:"customThankyou";s:0:"";s:21:"customThankyouMessage";s:30:"Thank
    you for your submission!";s:22:"customThankyouRedirect";s:0:"";}
author: Alan Francis
permalink: "/2018/01/22/model-vs-view/"
---
I started my post-college career as a Microsoft Windows developer, building a visitor centre application for Scottish Nuclear's "Come and See" program in 1995.  Microsoft's Visual C++ and the MFC framework meant it was relatively easy to make an app "look and feel" like Windows.  You really had to go out of your way to make a really bad UI, but obviously that didn't stop everyone.

My code back then wasn't great.  Wasn't the cleanest.  Didn't have much attention lavished on its internal structure.

By 2000 I was a Java developer and developing client side apps for Windows was an exercise in frustration, so I welcomed the switch to serverside - servlets and JSP.  On that team we had front end people and designers to build the HTML and CSS, so I was able to concentrate on building a good model and controller layer.  This was the era of TDD and patterns, refactoring.  Complete focus on the structure of the code.  Beautiful on the inside was all I cared about.

Around 2006 I switched to Ruby and Rails, but still I had people to worry about the UI.  I could keep my head clear for refactoring, code smells, classes and methods and structure and not be worrying really about how the app looked.  This was immensely satisfying in the dayjobs, where the front end team could be relied upon to take care of stuff.  It was, however, immensely frustrating in personal side projects. Sure I could build a basic little app to store comics, or run a conference, but I lacked any of the skills to make it look good enough to release to the public.  I quickly gave up.

Then, in 2008, the iPhone.

A return to my days of Windows.  A standard platform look and feel.  An IDE that allowed one to make HIG compliant, clean UIs.,  Again you had to work hard to make something horrible, it was easy to make something look decent.  Satisfying.  My little home projects looked reasonable.  I didn't feel like I needed a designer to do the front end stuff.

Now, looking at these three phases - Windows, web apps, iOS it strikes me that in each case I'm saying I don't have to worry too much about the UI.  Either because the tools make it easy for me, or because someone else is worrying about it.  In all three cases I should be free to focus on the internal quality of my code.  To do the things I love, looking for places where code belongs elsewhere, or theres an abstraction that will clear up a ton of code (I'm much happier refactoring code than I am writing it :).

What I notice though is that even though the UI is "easy" (_very simple little apps remember, I'm not saying all UI/UX work is easy, just that its easy to make something thats not horrible_) having to write those classes at that level means I take my eye off the ball.  I sacrifice a lot of model-level awareness and attention because I'm building the app one ViewController at a time instead of thinking of a domain model.

Maybe I need to get back to server side development, or build command line apps.  🤔
