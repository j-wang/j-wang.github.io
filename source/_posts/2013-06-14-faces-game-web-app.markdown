---
layout: post
title: "Faces Game Web App"
date: 2013-06-14 19:09
comments: true
categories: [web, hacker school, python, programming]
---

Last week, while working on my [simple text editor](/blog/2013/06/07/first-project-simple-text-editor/), I chatted with fellow Hacker Schoolers about what was actually being used in the industry nowadays for GUIs. The answer I most commonly heard was that the tech world had largely moved on to web (and mobile) apps. No one who I spoke to actually worked on desktop applications. The insight here is quite fascinating to me.

Developers and users are a chicken-and-egg problem. To get developers to work on a platform, you need users. To have users, you need a strong developer base that creates appealing and useful apps. It's a small sample set, but the lopsided nature of the sentiments suggest to me that either developers have a stronger lean towards web than users or the companies that work in web are much more interesting/appealing to the strong programmers that Hacker School selects for and attracts. Again, it's too small a sample set to draw sweeping conclusions, but it is an interesting data point. Nevertheless, these conversations helped point me to my next project: a web app.<!-- more -->

I've built static websites and played around with content management systems (CMS) like Wordpress in the past. In doing so, I've played around with HTML and CSS. However, those have mainly been exercises of graphical tweaking. I've never built a web app (dynamic content) before, which requires more understanding of how web sites serve content (requests and responses), knowledge of server/user data management (persistent storage and sessions/cookies), and at least a basic understanding of concurrency -- though web frameworks largely abstract away the latter.

At the beginning of the week, I spent many productive hours working through tutorials and many less productive hours thinking about what web app to create and what language to write it in. I ended up defaulting to Python, since like last time my current goal is to focus on this area topic and not languages. In terms of web frameworks, a fellow batch-mate, [Erik](http://skien.cc/), suggested that I look at [Flask](http://flask.pocoo.org/), whose lightweight nature immediately appealed to me. For what I needed it for, it has easy-to-understand and extremely simple routing and templating functionality. There's still some magic in routing, but I'm not quite prepared to write my own web framework and create HTML request-handling/responses myself quite yet.

Finally, I decided to create a "Faces Game," based on a project from a prior batch. There was some demand for an updated version including our batch, and it had all of the elements I wanted: dynamic content, (minor) data management, login handling (since I wanted to restrict it to Hacker Schoolers), scraping (I wanted to dynamically retrieve information from the Hacker School website for posterity), and processing/handling scraped data. The code is on [Github](https://github.com/j-wang/faces-game) and the app is live on [Heroku](https://enigmatic-hollows-9840.herokuapp.com/). There are definitely many improvements I can make to it: multiple difficulty levels, free-form responses, caching, OAuth vs. mechanize login, etc. but it served its learning purpose. I learned a lot about what HTML requests/responses contain, HTML GET and POST methods, how form data works, how HTML templates work, and how web servers work (along with, of course, a refresher on writing HTML/CSS). Given that these topics are non-specific to Python and Flask, post-project I found that I could helpfully pair with several other Hacker Schoolers on their web apps (whether they were Python/Flask or not). Although I'm probably done with this web app, given their importance/prevalence I'll probably be revisiting web apps again in the future.

Only Hacker Schoolers can play the game, but screenshots from it are below.

## Login Screen
Takes a username and password, which my scraper (built without preexisting scraper/spider libraries) uses to login into the Hacker School website for the user session.

{% imgcap /images/posts/faces_game_login.png %}

## Batch Selection
These batches are dynamically scraped from the Hacker School website, which means that this will automatically handle future batches.

{% imgcap /images/posts/faces_game_select_batch.png %}

## Guessing Screen
This screen shows random Hacker Schooler, with the correct name somewhere in the list of five other names (drawn randomly from the same batch) below.

{% imgcap /images/posts/faces_game_guess.png %}

## Result Screen
Shows whether or not the user correctly answered, the name of the Hacker Schooler, and his or her listed skills on the Hacker School website.

{% imgcap /images/posts/faces_game_after_guess.png %}
