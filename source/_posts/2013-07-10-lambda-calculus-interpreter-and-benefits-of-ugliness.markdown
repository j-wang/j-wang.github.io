---
layout: post
title: "Lambda Calculus Interpreter and Benefits of Ugliness"
date: 2013-07-10 23:19
comments: true
categories: [programming, python, haskell, lambda calculus]
---

For the past week, I've been working on a lambda calculus interpreter ([Github](https://github.com/j-wang/untyped-lambda-calculus-interpreter)) in order to try to better understand the concepts in lambda calculus. It's been a great experience, especially since I've been coding it in Python. Normally, if I were writing the interpreter in a language like SML or Haskell, I would rely heavily on pattern-matching.

Using pattern-matching is a pretty natural fit for creating an interpreter, given the operations involved in lexing, parsing, and reduction/evaluation -- specifically, handling a particular encountered block depending on the token or keyword/structure encountered.<!-- more --> An example of this idea implemented in a Haskell lexer is below:

{% codeblock lang:haskell %}
lexer :: String -> [String]
lexer [] = []
lexer (x:xs) = 
    case x of
      '(' -> "(" : lexer xs
      ')' -> ")" : lexer xs
      '.' -> "." : lexer xs
      ' ' -> lexer xs
      _   -> let chunkWord [] = []
                 chunkWord (y:ys) = 
                     case y of
                       ' ' -> []
                       ')' -> []
                       '.' -> []
                       _   -> y : chunkWord ys
                 word = chunkWord (x:xs)
             in word : (lexer $ drop (length word) (x:xs))
{% endcodeblock %}

The lexer takes a string and iterates through it character by character. Each character encountered has specified a certain action (even if the actions are simple in this case). A `'('` character produces the action of creating a cons list of the string `"("` and the result of lexer on the rest of the string. An analogous action happens for `')'`, `'.'`, and `' '`. For anything else (which has to then be a keyword or variable in lambda calculus), chunk it together as a string and cons it onto the rest of the the string (sans chunked string). It's not bad, though I imagine I'd probably be able to write this in an even more concise way if I knew Haskell better.

If I were to translate this code literally to Python, I would enter `elif` hell, since Python has neither pattern matching or even case expressions. Although beauty is in the eye of the beholder, a massive block of `elif`s is not what I consider beautiful code. For such a small codeblock, it isn't too bad, but even so, it looks verbose when in Python.

{% codeblock lang:python %}
class Lexer(object):
    @classmethod
    def tokenize(cls, string):
        result = []
        first_letter = True

        for char in string:
            if char == '(':
                result.append(char)
                first_letter = True
            elif char == ')':
                result.append(char)
                first_letter = True
            elif char == '.':
                result.append(char)
            elif char == ' ':
                first_letter = True
            else:
                if first_letter == True:
                    result.append(char)
                    first_letter = False
                else:
                    result[-1] = result[-1] + char

        return result
{% endcodeblock %}

Again, it's not _that_ bad, given how short the set of cases there are here (I'll have to actually find a longer interpreter to give a properly horrifying example). In fact, taking into account the `chunkWord` gymnastics I had to do in Haskell to satisfy the type checker (though that might just be my ignorance in Haskell), it might even look nicer to some (aside from the `first_letter` gymnastics required here because you can't easily skip ahead characters in a Python `for` loop... a topic for another post). However, even here the specific case pattern looks annoyingly verbose and repetitive, at least to my eyes (it takes up two lines at minimum and starts to blur together with many, many `elif`s or, alternatively, spaghetti code where the operations are separated from the call site if you opt for using a dictionary to handle each case... especially if you have to define the operations as functions separate from the dictionary). 

However, this type of ugliness does comes with an advantage. The Haskell version actually has a lot of code repetition which is made tolerable and even pleasant through pattern-matching. Being forced to think more cleverly about how to express one's ideas in a succinct way to counter that ugliness can give one deeper insight into what's actually going on. For example, the below code more explicitly acknowledges the code repetition and handles the `for` skipping-ahead challenge in a cleaner way (to my eyes), by accumulating characters when the current character is not one of the special delimiters and clearing it when it reaches one of those delimiters.

{% codeblock lang:python %}
class Lexer(object):
    @classmethod
    def tokenize(cls, string):
        result = []
        temp = ''

        for char in string:
            if char in ['(', ')', '.', ' ']:
                if temp != '':
                    result.append(temp)
                    temp = ''
                if char != ' ':
                    result.append(char)
            else:
                temp = temp + char

        if temp != '':
            result.append(temp)

        return result
{% endcodeblock %}

Even though this can probably also be improved upon, being forced to think harder and attack the problem with multiple paradigms was helpful. The fact that Python is not great at handling multiple cases in a neat, clean way was actually beneficial for my understanding in this case.
