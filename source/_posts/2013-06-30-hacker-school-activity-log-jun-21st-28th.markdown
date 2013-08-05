---
layout: post
title: "Hacker School Activity Log: Jun 21st - 28th"
date: 2013-06-30 23:50
comments: true
categories: [hacker school, programming]
---

I only started this log last week, but figured it would be helpful to start tracking more of what I do at Hacker School, apart from my bigger projects. As a side benefit, it might give some of the people who have asked me what I'm doing at Hacker School some insight into what a typical day might be like -- though do keep in mind I've likely not captured literally every single thing and this particular week was more chaotic and scattered than average.

This is more of an experimental idea right now, but if it works out well, I'll try to post these at the end of each week.

<!-- more -->

21 Jun 2013
--------------------
- Completed TicTacToe AI with real minimax in Python; always wins or ties. I previously created one that used a depth-based weighting system (the fewer moves it takes to reach a win/loss state and get close to a victory, the higher the "score"), but it sometimes did stupid things. To be fair, it didn't do stupid things that often, but considering the game is solvable, I figured I might as well solve it.  [Link to github repo here.](https://github.com/j-wang/tictactoe_ai)
- Set up Haskell for emacs (haskell-mode and scion); installed cabal and hlint. Typical setup for a new language.
- Reviewed Haskell [style guide](http://www.haskell.org/haskellwiki/Programming_guidelines). Style guides are one of the first things I look at in a new language -- it helps me get a flavor of what good code looks like, often informs me about some of the language's quirks, and usually tells me something about what is efficient or not in the language.
- Wrote first simple program in Haskell (along the lines of simple sum functions and "hello world" type of things -- I didn't bother to push it to github).
- I'm trying to wrap head around monads, though am largely failing. Some of the links I perused. [Haskell wiki page on monads.](http://www.haskell.org/haskellwiki/Monad) ["Do notation considered harmful."](http://www.haskell.org/haskellwiki/Do_notation_considered_harmful) [Monad laws.](http://www.haskell.org/haskellwiki/Monad_laws)
- Talked with Martin. After our conversation and playing around with the concept in SML, I grasp it a lot better. `>>=` is essentially a pipeline of actions, allowing access to wrapped value to execute... largely arbitrary... operations. I imagine a mathematician and a type system research would beg to differ given the monad laws, but it's effectively arbitrary for IO monad operations. Return obviously just provides the wrapped function --  the definition of Maybe (in Haskell) or Option (in SML, though it isn't formally a monad there) is a good example.
- At home now and I read slight more of TAPL (Types and Programming Languages, Benjamin Pierce), trying to cover the chapters leading up to 5 and 6 (untyped lambda calculus). I reviewed 5 and 6 already at Hacker School in order to participate in a study group session that talked about lambda calculus. Thinking about setting a goal around this (getting to know lambda calculus better) to implement some suggestions about concrete goals from Mel Chua during her residency at Hacker School
- Also continued to try to get better grasp of [IO as part of Haskell](http://www.haskell.org/haskellwiki/IO_inside) -- I definitely still need to continue studying this concept.

22 Jun 2013
---------------------
- This morning, I dug a bit into concurrency, started reading about the differences between threads and processes (need to dig into this more) and how to do [multiprocessing in Python](http://docs.python.org/2/library/multiprocessing.html), at least to some extent. I'm probably going to make some project out of this.
- Read about the Python GIL and some of the [reasoning behind it](http://ncoghlan_devs-python-notes.readthedocs.org/en/latest/python3/questions_and_answers.html). Though the post didn't really cover the point, it definitely makes sense that allowing shared mutable state in multiple threads would cause nightmarishly unpredictable results given the language semantics (dynamic, allowing functions/classes, as giant bags of properties/functions, to be modified at any time). At some point, Python is going to have to deal with this though. Even if sequential operation with a GIL is faster now than an implementation that has to juggle locks, eventually computers will have enough cores that sequential speed won't make up for lack of parallelism.
- Read a bit about [STM for PyPy](http://morepypy.blogspot.com/2011/08/we-need-software-transactional-memory.html). I'm starting to get a better grasp of the concept after multiple exposures in different languages. It doesn't seem too much deeper than atomic, logged (and thus retry-able) transactions, but that simplicity in this realm seems to be a good thing.
- Read about stackless Python, pypy, and [greenlets](http://greenlet.readthedocs.org/en/latest/). I never realized that these were all related projects. They embody an interesting approach to concurrency, though like threads it seems cumbersome/very low level and error-prone. Worth trying for myself and validating that though.
- Digressed into [Python performance factors](http://pypy.org/performance.html), which is interesting to think about, especially how local copies of "global" methods/variables speed up execution. I saw the concept appear in Python Cookbook but never really understood how it worked (e.g. why would calling a function twice in a loop vs. assigning a local variable to the _exact same function_ make any difference?). At least to some degree, I imagine this connects to the factors I mentioned about (objects as giant, dynamically modifiable bags), though I never connected the two in my mind before this.
- Read about the theory behind [JIT compilers](http://en.wikipedia.org/wiki/Just-in-time_compilation). It's fascinating stuff. I'm interested in potentially making one to more deeply understand the concept. Alas, I think this is a the general theme of the topics I'm exploring, which creates a bit of an issue with the reality of how much time I have in a day...
- Read about various [open source licenses and why I should include one](http://www.codinghorror.com/blog/2007/04/pick-a-license-any-license.html). I like the MIT license, personally. Short, sweet, gives credit and doesn't inhibit for-profit use (probably relates to my own business-friendly bias and what i like to personally see).
- Read random posts on [Nick Coghlan's blog](http://www.boredomandlaziness.org/); interesting to get a better sense of the Python community
- Hacker School rooftop party -- it was basically all of the coordinators' birthdays at once. Fun times, fun conversations, and I got a lot of charcoal on myself dealing with the "grill" (it was basically a fire-pit). Met a lot of great people who are doing some pretty impressive things (... though we regularly have various software world luminaries come into Hacker School or show up in our internal communications/Humbug anyhow -- fascinating how small the world is and tight knit it is).

23 Jun 2013
----------------------
- Read about Python 3 development and [how to be a core developer](http://docs.python.org/devguide/coredev.html)... not sure about core developer-ship -- I'm definitely not there right now -- but I would like to contribute to the Python language in some way. It'll be interesting to keep track and see if I get to a place where I can help out on any of the [major places where the community needs help](http://ncoghlan_devs-python-notes.readthedocs.org/en/latest/index.html)
- Set up julia-mode for emacs, 
- Read about [CoffeeScript](http://net.tutsplus.com/articles/interviews/should-you-learn-coffeescript/). It's an interesting idea, though I certainly need more practice with base javascript first.
- Diverted to CSS preprocessors (SASS and LESS) at least temporarily. Compass seems to give the advantage (somewhat) to SASS, but it seems unlikely that LESS wouldn't include something like it eventually. Given my dislike of working with CSS's syntax/concept in general though, I might try to stay with Bootstrap and AngularJS -- I don't mind someone else figuring out what the right kerning/sizing/font for various headers are and adjusting from there.
- Julia workshop with Leah! Intro to the Julia programming language.

24 Jun 2013
-----------------------
- [Ninety-Nine Haskell Problems](http://www.haskell.org/haskellwiki/H-99:_Ninety-Nine_Haskell_Problems), did some of these for [Haskell practice](https://github.com/j-wang/99-Haskell-Problems)
- Wrote a hook to allow my tictactoe AI to play online against other tictactoe AIs. In the process, I got to know Python's urllib a lot better. Unfortunately, I still can't figure out the server's exact API -- and I figure it isn't worth any more effort to fine-tune it, since I won't be getting much learning out of the exercise at this point other than what I need to connect to this specific server.
- Code review with Laura on tictactoe -- helpful to hear feedback on code that I did previously, and to see how much I've improved/wouldn't write things the way I did just a few months ago. It'll be useful to go back and do some code revisions/refactoring myself to crystallize some of my improvement over time (which is hard to see otherwise).
- Reviewing [Git Pro](http://git-scm.com/book/en/) in preparation for GitHub training at Jane Street
- Listened to a talk by Carin Meier on controlling robots and drones using Clojure! It was hosted by Etsy, which definitely was an interesting location (with a very quirky office).

25 Jun 2013
----------------------
- Went to Intermediate/Advanced Git tutorial hosted at Jane Street Capital this morning. It started a bit slow, but was really useful in bolstering my understanding of the tool, especially coming from a non-professional (programming) background and never having used a lot of the collaboration or branching functionality in a serious way.
- Reading [Berkeley Bloom paper](http://db.cs.berkeley.edu/papers/cidr11-bloom.pdf) in preparation for Systems discussion group.
- Did a code reading for one of the earliest commits for bottle (commit HEAD~820 / SHA hash: 4f50cece28b8ee3ff1c5bcf3f8a7bd1d3bbf6128). It's really interesting to see how the author put together the pieces of it and the design decisions that went into it. My group also discovered a bad bug in the regex/special URL handling that I imagine got fixed in the later revisions (bottle sequentially loops through a list of regexes to match against a URL -- however, this means a more general regex will always match first; compounding on this, there's a semi-random optimizing process that brings more frequently matched regexes forward in the list... which in the case of a very general match, would ultimately and self-reinforcingly end up in front and block any other matches from ever being reached).
- Coming out of the bottle code reading session, I worked with my group in wrangling with the Python `@property` decorator (and decorator syntax more generally) and `@x.setter` syntax
- Talked with Tom and learned about "magic methods" and how they work (`__get__`, `__set__`, `__setattr__`, `__getattr__`, `__setattribute__`, `__getattribute__`) -- really interesting, and much more magic than I expect most users would need/understand... That being said, to really get to understand/know Python, I'll definitely need to understand how those work and how they fit into its evaluation model.
- More learning about monads in Haskell from [yet another blog post](http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html). I did a fair number of the exercises, which was useful. I haven't uploaded them to github, but maybe I will later (if I finish them).
- For the heck of it, I did learn how [Haskell testing frameworks work](http://lambda.jstolarek.com/2012/10/code-testing-in-haskell/) (test-framework, HUnit, QuickCheck2), through testing my Monads tutorial exercises. This definitely was not the cost-effective project to learn this from (given that I probably didn't need tests at all), but I did need to learn sometime. In general, I should tighten up my testing discipline -- out of the major "best practices" (clean, readable code, clear documentation, etc.), I am currently the most lax/entirely negligent on rigorous and systematic testing. I rely on my "mental interpreter" too much, which while important, shouldn't substitute for good tests. Not being a professional programmer, I just have never needed the discipline.
- In the same vein (best practices and getting to know the Haskell ecosystem at least), I've been learning about how Haskell modules work.
- Reading from Computer Systems: A Programmer's Perspective (Bryant, O'Hallaron); probably going to continue reading this book and try to do questions from it


26 Jun 2013
-----------------------
- Finished reading [Bloom paper](http://www.bloom-lang.net/calm/) and researching [CALM](http://databeta.wordpress.com/2010/10/28/the-calm-conjecture-reasoning-about-consistency/) principles. These weren't actually the papers I was supposed to read though... I'm hoping they give me a better foundational understanding for Dedalus paper, which we are actually discussing.
- Briefly went through [logic programming](https://en.wikipedia.org/wiki/Logic_programming), in order to better understand the Dedalus paper; I certainly barely scratched the surface, but I understand the basics of it now -- hopefully enough for the paper -- after seeing examples like the [Horn clause](https://en.wikipedia.org/wiki/Horn_clause). Formal logic from philosophy in college is starting to come back to me...
- Discussed [Dedalus](http://www.eecs.berkeley.edu/Pubs/TechRpts/2009/EECS-2009-173.pdf) with Systems reading paper group -- I have a better sense of it now after working through it with the group, though I still don't entirely understand how Dedalus gracefully handles asynchronous data/information -- at least not explicitly. I do understand the idea of how the time stamps would work with eventual consistency (rules come in later that help lead to consistency), but not how they literally make this happen. I'll reread the paper later, but that part seemed a bit fuzzy (though there were certainly enough dense sections that I could have missed it).
- More discussion on monads, functors, and their mathematical properties with Nabil, Alex, and Alice
- Listened to a talk about [miniKanren](http://www.youtube.com/watch?v=fHK-uS-Iedc)
- Set up [octopress blog](http://j-wang.net) on github pages with custom domain
- Migrated posts (or at least downloaded them) from my Blogger blog
- Updated blog colors, poked around how octopress/jekyll liquid templates work
- Haskell meetup: I Accidentally the Entire Heap: How I Learned to Love the Profiler. Listened to [this presentation](https://www.bitba.se/i-accidentally-the-entire-heap/). If nothing else, this definitely served as a wake-up call for me to pay a _lot_ more attention to how laziness affects the evaluation of Haskell programs. I imagine my lack of mental model familiarity with it is going to bite me at some point (probably sooner rather than later). Aside from the content of the meetup, I also thought that it was really interesting to see the strong (and seemingly consistent, based on peoples' familiarity with each other) community around the language. I'm going to look forward to seeing what sort of communities there are in the Bay Area.

27 Jun 2013
-----------------------
- Came in early this morning to participate in a Javascript seminar led by Mary on javascript (of course) and higher order functions
- Discussed characteristics of `this` in javascript along with how closures work in javascript with Mary
- Finished pretty basic implementation (after discussing with Mary) for [map and reduce](https://github.com/j-wang/javascript-exercises). My misunderstanding of `this` was definitely holding me back from writing better patterns. However, I don't think I love the particular quirks in javascript or its general evaluation/scoping model (yeah, I know, join the club, right?).
- Worked a bit more on my [blog](http://j-wang.net) (this post will actually be on my blog -- very meta, huh?). Pushed/uploaded posts from my old blog (which is still linking to my blogger site for images... definitely need to fix that) and pushed updated color scheme. Nothing dramatic, but I do like the slight dash of subtle color, vs. the default octopress black on gray, on white.
- It seems I have ended up working a bit longer on my site than I expected. I messed up and did `git checkout source` (which was behind my detached HEAD in git) -- needed to do a bit of recovery with `git reflog` and moving everything up to the most-updated blog state. It seems the git tutorial came in handy very quickly after all.
- While working on my site anyway, I debugged Disqus comments not working on my site... after digging into the liquid template, it looked like I needed to specify the comments = true in the front-matter for each page.
- I started writing my untyped lambda calculus interpreter in Python (which I probably should have started a few days ago). [Here's the link to it](https://github.com/j-wang/untyped-lambda-calculus-interpreter), though it definitely isn't ready for prime time... or even to be run. I'm currently only as far as the lexer.
- I also started it in Haskell (actually flipping between the two for a while...), but I don't think I have enough experience in it yet to do so efficiently, even though it's likely the more natural fit for an interpreter.
- While writing the lexer, I learned a lot about regular expressions (specifically for Python, but also more broadly applicable). Funny how these projects end up creating paths that help me learn things that I never thought I would when I began the project... (though I probably should have anticipated this for an _interpreter_).
- Read some of "The Mythical Man Month." It's a very interesting book. Many of the principles/challenges are similar to what I saw in the investment world (creative, technical field). Essentially, I take away that it's important (as I learned in that field) to manage so that you can accomplish what you need to do with a realistic amount of time and available resources -- to crunch it is just to create a train wreck. You might get halfway to your goal, but you didn't get all the way and left a large mess that you'll need to clean up later. I'm pretty happy that the field in general seems to care deeply about craftsmanship and code beauty though.

28 Jun 2013
-------------------------
- Played D&D with other batch members -- never played before, so it was interesting and fun. I definitely can imagine that one's experience will depend greatly on the group you gather though. Fortunately, we have a good one among Hacker Schoolers!
- Read more of "The Mythical Man Month"
- Slowest coding day since I started Hacker School, but at least there was great bonding time!
