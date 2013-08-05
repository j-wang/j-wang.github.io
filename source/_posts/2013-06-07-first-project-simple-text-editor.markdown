---
layout: post
title: "First Project: Simple Text Editor"
date: 2013-06-07 15:31
comments: true
categories: [python, programming, GUI, hacker school]
---

I have a lot of project ideas that I want to play around with during Hacker School. However, my primary goal -- the thread that ties all of these disparate ideas together -- is to tackle areas of programming that I have never encountered. If an area of programming is "magical" to me, I want to dispel that magic.

First up on that list of "magical" topics is graphical user interfaces (GUIs). I remember trying to experiment with GUIs in a group project during high school Java using Swing and AWT. I also remember quitting out of disgust after only a brief foray into their APIs, leaving one of my poor teammates the painful task of programming our project's GUI, while I worked on fun game logic. Since that brief attempt, I have never attempted to play around with GUIs ever again, so I have remained mystified as to how I would actually go about creating them. So, after nearly a decade, I've decided to try my hand at it again.<!-- more -->

Given my learning goal, I wanted a project whose primary component was the GUI. After a bit of brainstorming, I settled on a basic text editor. No spell-check, no fonts, and no fancy functionality. I wasn't looking to recreate Word or Emacs. I just wanted something to play the role of mannequin for my GUI.

I also debated a bit which language I should use. Clojure and Python were the primary contenders. I'm pretty curious about Clojure, and its Java interop would give me access to Swing and AWT, but after seeing the state of those frameworks (not much different than when I was in high school) and realizing that I'd spend a lot more time wrestling with Clojure and Java frameworks than learning, I decided to go with Python and potentially revisit Clojure later. Python is the (mainstream) programming language I know best (alas for poor frozen and academic SML) and I've heard good things about Tkinter.

The end result is on my [Github](https://github.com/j-wang/simple-text-editor). A screenshot is below.

{% imgcap /images/posts/python_texteditor.png True to its name, it is quite simple %}

The process of building the GUI was all right. It was less painful than I had expected, probably in part because I'm a more experienced programmer now and Tkinter is simpler than Swing/AWT. However, I probably won't focus much more on GUIs specifically after this project. I'm glad I did it, since I will know how to approach creating GUIs in the future, but it was more an exercise of learning APIs and painstakingly laying out each element in code than it was a "hard problem" that stretched me as a programmer.
