# Common Lisp - The Tutorial Part 2: Not in Kansas anymore - Symbols

> A hero ventures forth from the world of common day into a region of supernatural wonder: fabulous forces are there encountered and a decisive victory is won: the hero comes back from this mysterious adventure with the power to bestow boons on his fellow man.
>
> -- <cite>The Hero with a Thousand Faces, John Campbell</cite>

## Introduction

I am glad I have not scared you off, we are going to train hard and fast and get you on your feet ready to make awesome happen. I am making some assumptions[^1] that you have some experience with computers and programming ideas[^2] and have emacs, slime, and sbcl installed[^3]. There are many editors for Common Lisp but for this journey we are going to use emacs and slime, it is like learning to box by picking up chickens (Rocky II[^4]).

[^1]: I know to ass-u-me is a bad idea, but I really love Common Lisp and think you will too.
[^2]: [Lisp Book](https://www.cs.cmu.edu/~dst/LispBook/book.pdf) is a free book starting at the ground level and [Practical Common Lisp](https://gigamonkeys.com/book) is a good intro that is more practical. Another good source of sources [Steve Losh's Blog](https://stevelosh.com/blog/2018/08/a-road-to-common-lisp).
[^3]: If on windows grab [Portacle](https://portacle.github.io) otherwise [Getting Started](https://lisp-lang.org/learn/getting-started).
[^4]: [Rocky II scene](https://www.youtube.com/watch?v=q7cDQY9wVF8)


## Toto, I’ve a feeling we’re not in Kansas anymore

Common Lisp is not like most languages you may have used[^5]:

1. edit,
2. run compiler,
3. run executable,
4. start again.

Instead your editor (emacs in our case) is plugged in via slime (sort of like an umbilical cord) to a living breathing Lisp image containing your tools (sbcl the compiler, CLOG Builder, etc) and your code as you grow it. It is an organic process. When it comes time to deliver an executable (your baby) you ```save-lisp-and-die```[^6].

This system of development is far faster and certainly a lot more fun. You get to see results immediately and experiment on the spot. You can enter your code in one of two places to inject the imageith, either in the REPL or in text files that you read into the REPL.

The REPL (read–eval–print loop) is in many ways similar to an operating system's shell, so much so that there are Lisp alternatives for the shell[^7]. In Lisp it is possible to write code that directly affects how code is read, how it is evaluated, and how the results are returned to you. All of that is beyond our journey.

Let's start our Lisp image and start talking to it. Using emacs ```M-x slime``` starts slime and in most cases starts the new image. Once ready the REPL prompt is returned:


```lisp
; SLIME 2.26.1
CL-USER>
```

For most of us the Lisp image resides on our laptop or the like, but it could be anywhere including a lunar lander on mars[^8].

Type a simple command:

```lisp
CL-USER> (print "hello")

"hello"
"hello"
```

The REPL returns the results of the function ```(print "hello")``` which is "hello" and the side effect (it is called a side effect because it doesn't affect our Lisp image but our world) of running the print function which is "hello" output to the *standard-output* stream which in this case is directed to our slime interface attached to our Lisp image.

Unless poking and prodding at our code already in the Lisp image most of the time we want to keep our code around in files.

In emacs execute ```C-x C-f``` and type a file name say like hello.lisp. The .lisp extension is most often used. Now we can enter our program in to our file:

```lisp
(print "hello")
```

Save the file ```C-x C-s```

In the REPL we can now _Read_ the contents of the file, which then will be _Evaluated_ and _Print_ the results.

```lisp
CL-USER> (load "~/common-lisp/hello.lisp")

"hello"
T
```

Our side effect "hello" is output and T (the Lisp symbol for true, although any value that is not nil is considered true) is returned for the load operator.

Unlike other languages, files do not provide structure in a program[^9]. Whatever you put in a file just gets pumped into the Lisp image when you load it as if you were typing it directly into the REPL.

The structure of a Lisp program is organized by "packages" of definitions for symbols. Symbols are associated in the reader with functions, macros, data, and more and give them a human readable name. There is only one Lisp image and everything lives in the same place, the symbols just refer to where things are in that image and packages group symbols together.

The REPL tells us which package is the default package of symbols we are using.


```lisp
CL-USER>
```

In this case it is "CL-USER", which is just a name, but any standard implementation will already have that package defined.

The symbol name for print (with its home package) is - common-lisp:print. The cl-user package "uses" the common-lisp package of symbols and since that is our default package we don't have to write the package name part of the symbol name (common-lisp:) just the symbol print. Later we will discuss how to define our own packages[^10].

I know my fellow journeyer this is all strange and different, a language that is alive, a pool of primordial ooze with sections of the goop identified by packages of symbols. It is important though to understand the nature of Lisp from now to not fall into the trap of thinking it is like other languages.

[^5]: Lisp was discovered not invented - [Roots of Lisp](http://www.paulgraham.com/rootsoflisp.html)
[^6]: That really is how it is done… ```(sb-ext:save-lisp-and-die "hello.exe" :toplevel #'main :executable t)```
[^7]: [SHCL](https://github.com/SquircleSpace/shcl
[^8]: [Lisping at JPL](https://flownet.com/gat/jpl-lisp.html) - Having a read-eval-print loop running on the spacecraft proved invaluable.
[^9]: In ASDF systems files are part of their structure and will talk about them when we are ready to start building software.
[^10]: The package name is itself a symbol and *package* is a symbol that is associated with the symbol of the current default package the reader is using at the moment.


## Symbols

Let's create some symbols and talk about them. You can create them in the REPL directly or store them in a file and load the file again in the future. There is an important set of keys to remember ```M-C-x``` (or ```C-x C-e```). Using that emacs command allows you to just send the code you are working on directly to the Lisp image instead of rereading the entire file. For example in our hello.lisp file while near the ```print``` code hit ```M-C-x``` and "hello" will be output in our slime-repl window. The code was read and evaluated and the side effect was displayed from the ```*standard-output*``` stream.

The first symbol we will create ```main``` will be associated with a function that we are familiar with already:

```lisp
(defun main ()
  (print "hello"))
```

Now let's create a symbol ```*cool*``` that names a global dynamic variable with the value ```123```.

```lisp
(defvar *cool* 123)
```

If I want to change the variable's value we use ```setf```, Lisps general assignment operator.

```lisp
(setf *cool* nil)
```

Nil is the null value and/or false. The symbol t is for true (more accurately anything not nil is true).

We can even change the value to our function from before.

```lisp
(setf *cool* #'main)
```

Main is just a label pasted on a ball of goop in the list image. That the ball of goop is like any other data. ```#'``` is a _reader macro_ that returns the function that a symbol names.

Symbols in Lisp besides being used to name balls of goop in a list image are a "type" in Lisp. When we use symbols for themselves, i.e. as a type we place an apostrophe before the symbol name:

```lisp
'a-symbol
```

So we can say

```lisp
(setf *cool* 'a-symbol)
```

You as the programmer decide what 'a-symbol means. It is not a string of characters, it is not a number, it is a symbol. Maybe one day you write a function when passed as an argument of the symbol 'love it paints flowers and when passed 'hate it paints skulls, passing "love" or "hate" would be an error as they are just talk, i.e. strings, not real 'love or real 'hate.

Symbols are case sensitive but the _reader_ part of the REPL turns all symbols to uppercase before evaluating them. So typing in to the REPL:

```lisp
(equal 'a-symbol 'A-symbol)
```

Will return ```T```.

You can though still create cased symbols by placing the symbol between pipes:

```lisp
(equal '|a-symbol| '|A-symbol|)
```

Will return ```NIL```.

```lisp
(equal 'a-symbol '|A-SYMBOL|)
```

Will return ```T```.

## Technical Terms

The standard unit of interaction with a Common Lisp implementation is the form.

Meaningful forms may be divided into three categories: self-evaluating forms, such as numbers; symbols, which stand for variables; and lists. The lists in turn may be divided into three categories: _special forms_, _macro calls_ and _function calls_[^11].

[^11]: Common Lisp the Language, 2nd Edition Section 5.1

## Conclusion

I know my journey mate that we are going slow and theoretical so far, but seeing things the Lisp way is important for later on. Soon things will go much faster.

## Summary

### From Part 1

1. Lisp uses “LISt Processing”
2. The first element of the list is an operator and the remainder of the list are its arguments.

### From Part 2

1. Lisp has a different development cycle than other languages and a different model of development, programs are grown.
2. The top level of structure in Lisp programs is the package, which organizes symbols, not files.
3. A symbol is coded as package-name:symbol-name but if package-name is the same as the current default package you can refer to the symbol-name alone.
4. Symbols can name packages, functions, variables, and more.
5. Symbols can also be used as a data type as well by placing an apostrophe before the symbol name.


