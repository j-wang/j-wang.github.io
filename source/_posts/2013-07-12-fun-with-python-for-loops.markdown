---
layout: post
title: "Fun with Python For Loops"
date: 2013-07-12 13:07
comments: true
categories: [python, programming, hacker school]
---

As mentioned in my [last post](/blog/2013/07/10/lambda-calculus-interpreter-and-benefits-of-ugliness/), while working on a lambda calculus interpreter, I encountered an issue where I wanted to skip ahead in a _for_ loop, but found that I couldn't. Although I initially put it aside, when I was getting a code review on the interpreter from Tom, I remembered the issue again and did some further investigation on the topic.<!-- more -->

## Reassigning the Index Variable

It turns out that unlike _for_ loops in Java or C++, the index variable (e.g. `i` in `for (int i = 0; i < 10; i++)` is drawn directly from the object following the `in` keyword each time the _for_ loop goes around. As such, changing what the index `i` references in the equivalent Python loop `for i in range(10)` won't affect the next round of the _for_ loop (though it will affect anything following the reassignment in the block).

{% codeblock lang:python %}
for i in range(5):
    print i
    i = 20
{% endcodeblock %}

The above code results in:

{% codeblock %}
0
1
2
3
4
{% endcodeblock %}

## Reassigning the Iterable Variable

Additionally, after you provide Python with an initial reference to an object with an iterable method in a _for_ loop, Python accesses the object directly without referring back to the reference variable passed into the _for_ loop. So, what does this mean? Well, it means that you can't reassign the _for_ loop's iterable object in the middle of a _for_ loop.

{% codeblock lang:python %}
iter_this = range(5)
for i in iter_this:
    print i
    iter_this = range(3000)
{% endcodeblock %}

The above code, despite reassignment of `iter_this` results in:

{% codeblock %}
0
1
2
3
4
{% endcodeblock %}

So, does this mean that you can't do anything to modify what a _for_ loop ranges over once you've started it? Nope. Although Python accesses the iterable object directly without regards to what your reference subsequently points to, something else can also access that iterable object -- specifically, the reference that you originally used to direct Python to that object.

## Mutating the Iterable Object

Since you have a reference to the Python object itself, you can actually change the object that is being iterated over by the _for_ loop from right under it. Essentially instead of reassigning the object's reference, you can change the object itself, i.e. mutate it.

We know what the below code does:

{% codeblock lang:python %}
iter_this = range(5)
for i in iter_this:
    print i
{% endcodeblock %}
{% codeblock %}
0
1
2
3
4
{% endcodeblock %}

However, this does something different:

{% codeblock lang:python %}
iter_this = range(5)
for i in iter_this:
    print i
    iter_this.pop()
{% endcodeblock %}
{% codeblock %}
0
1
2
{% endcodeblock %}

So what happened? Well, let's take a closer look:

{% codeblock lang:python %}
iter_this = range(5)
for i in iter_this:
    print "iter: " + str(i)
    print "pop!: " + str(iter_this.pop())
    
print "iter_this: " + str(iter_this)
{% endcodeblock %}

{% codeblock %}
iter: 0
pop!: 4
iter: 1
pop!: 3
iter: 2
pop!: 2
iter_this: [0, 1]
{% endcodeblock %}

As the _for_ loop iterates through the list created by the `range` function, we mutate the list by popping off its last element at the end of each _for_ loop round. Hence, the list that the _for_ loop is drawing from is getting shorter as the _for_ loop lists, resulting in the _for_ loop ending early.

This means that if you need to skip elements in a _for_ loop, you can certainly mutate the iterable object. However, just because you can doesn't mean you necessarily should. Mutation will often cause the _for_ loop to become a lot less straightforward to reason about and may end up creating strange and confusing bugs.

## Other Ways to Skip Around a Loop

One way of skipping elements in a loop that isn't quite as opaque is by explicitly creating another variable that can be modified and `continue`ing when the elements in the iterable are less than (or greater than, or whatever) than the modifiable variable.

This method is what I ultimately settled on for parts of my lambda calculus interpreter that needed to skip elements. It made sense since my _for_ loop skipping would always be in the forward direction (I would never need to backtrack in my _for_ loop) and this makes it crystal clear what I'm doing.

{% codeblock lang:python %}
iter_this = range(10)
skip_while_less = 0

for i in iter_this:
    if i < skip_while_less:
        continue
    else:
        print i
        skip_while_less += 3
{% endcodeblock %}
{% codeblock %}
0
3
6
9
{% endcodeblock %}

On the other hand, if you need to skip around a loop (backwards as well as forwards), it is probably better (and clearer) to just use a while loop.

{% codeblock lang:python %}
i = 2
end = 12

while i < end:
    print i
    if i % 2 == 0:
        i -= 1
    else:
        i *= 4
{% endcodeblock %}

{% codeblock %}
2
1
4
3
12
11
{% endcodeblock %}
