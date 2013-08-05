---
layout: post
title: "Fun with Python For Loops"
date: 2013-07-11 13:07
comments: true
categories: [python, programming]
---

As mentioned in my [last post](/blog/2013/07/10/lambda-calculus-interpreter-and-benefits-of-ugliness/), while working on a lambda calculus interpreter, I encountered an issue where I wanted to skip ahead in a for loop, but found that I couldn't. Although I initially put it aside, when I was getting a code review on the interpreter from Tom, I remembered the issue again and did some further investigation on the topic.<!-- more -->

It turns out that unlike for loops in Java or C++:

1. The index variable (e.g. `i` in `for (int i = 0; i < 10; i++)` is drawn directly from the object following the `in` keyword each time the for loop goes around. As such, changing what the index `i` in the equivalent Python loop `for i in range(10)` references won't affect the next round of the for loop (though it will affect anything following the reassignment).

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

after you provide Python with an initial reference to an object with an `iterable` method in a for loop, Python accesses the object directly without referring back to the _reference_ passed in the for loop.

So, what does this mean?

Well, if I had a block of code where I wanted to skip to next paren

{% codeblock lang:python %}
expr = ['(', 'lambda', 'x', '.', 'x', ')', 't']
length = len(expr)
i_list = range(length)

for i in i_list:
    print expr[i],
    if expr[i] == '(':
        new_i = expr[i:].index(')') + i
        i = new_i
{% endcodeblock %}

{% codeblock %}
( lambda x . x ) t
{% endcodeblock %}


What's going on? Isn't the iterator changing?

{% codeblock lang:python %}
expr = ['(', 'lambda', 'x', '.', 'x', ')', 't']
length = len(expr)
i_list = range(length)

for i in i_list:
    if expr[i] == '(':
        new_i = expr[i:].index(')') + i
        i = new_i
    print i
{% endcodeblock %}

{% codeblock %}
5
1
2
3
4
5
6
{% endcodeblock %}


Must be drawing from i_list -- let's change that instead.

{% codeblock lang:python %}
expr = ['(', 'lambda', 'x', '.', 'x', ')', 't']
length = len(expr)
i_list = range(length)

for i in i_list:
    if expr[i] == '(':
        new_i = expr[i:].index(')') + i
        i_list = i_list[new_i:]
    print i_list
{% endcodeblock %}

{% codeblock %}
[5, 6]
[5, 6]
[5, 6]
[5, 6]
[5, 6]
[5, 6]
[5, 6]
{% endcodeblock %}


Let's look at what these objects really are...

{% codeblock lang:python %}
expr = ['(', 'lambda', 'x', '.', 'x', ')', 't']
length = len(expr)
i_list = range(length)

print id(i_list)

for i in i_list:
    if expr[i] == '(':
        new_i = expr[i:].index(')') + i
        i_list = i_list[new_i:]
    print id(i_list)
{% endcodeblock %}

{% codeblock %}
4494430936
4494431152
4494431152
4494431152
4494431152
4494431152
4494431152
4494431152
{% endcodeblock %}


Mutate the iterator

{% codeblock lang:python %}
i_list = range(10)

for i in i_list:
    print "iter: " + str(i)

print ""

for i in i_list:
    print "pop!: " + str(i_list.pop())
    print "iter: " + str(i)
{% endcodeblock %}

{% codeblock %}
iter: 0
iter: 1
iter: 2
iter: 3
iter: 4
iter: 5
iter: 6
iter: 7
iter: 8
iter: 9

pop!: 9
iter: 0
pop!: 8
iter: 1
pop!: 7
iter: 2
pop!: 6
iter: 3
pop!: 5
iter: 4
{% endcodeblock %}


Other methods of doing this:
Use continue to skip loops...

{% codeblock lang:python %}
i_list = range(10)
skip_while_less = 0

for i in i_list:
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



while:
{% codeblock lang:python %}
i = 0
i_list = range(10)

while i < len(i_list):
    print i
    i += 3
{% endcodeblock %}

{% codeblock %}
0
3
6
9
{% endcodeblock %}


And, of course... mutate!

{% codeblock lang:python %}
i_list = range(10)

for i in i_list:
    print i
    i_list.reverse()
    i_list.pop()
    i_list.pop()
    i_list.reverse()
{% endcodeblock %}

{% codeblock %}
0
3
6
9
{% endcodeblock %}
