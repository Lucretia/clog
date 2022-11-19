# Common Lisp - The Tutorial Part 3: Evolution - Packages and Systems

> As we live a life of ease (a life of ease)</br>
> Every one of us (every one of us)</br>
> Has all we need (has all we need)</br>
> Sky of blue (sky of blue)</br>
> And sea of green (sea of green)</br>
> In our yellow (in our yellow)</br>
> Submarine (submarine, aha)
>
> –- <cite>The Beatles</cite>

## Introduction

Common Lisp evolved by cross breeding dialects of Lisp that were being used to write production real world software[^1]. That makes Common Lisp a 30+ year stable hybrid with no need nor want of anyone to update to its 1984 standard and a powerful language to fight "fake tech" programmed with buzz words like web 3.0. _**Real**_ real world software requires the ability to create modular code and to do so you need support in the language, this is achieved with packages in Common Lisp, and outside of the language, this is achieved with ASDF/QuickLisp systems.

[^1]: "Nevertheless this process has eventually produced both an industrial strength programming language, messy but powerful, and a technically pure dialect, small but powerful, that is suitable for use by programming-language theoreticians."   [The Evolution of Lisp](https://dreamsongs.com/Files/Hopl2.pdf)

## Packages

As previously mentioned, the structure of a Lisp program is organized by _packages_ of definitions for symbols. With those words we are ready now to begin growing our software and our **practical** knowledge of Lisp at the same time[^2].

1. Open emacs
2. M-x slime to run slime
3. At the slime prompt ```CL-USER>``` we are going to type the command:
```lisp
(ql:quickload :clog)
```
This will load CLOG into our lisp image and all the needed dependencies. We will learn more about quicklisp and its quickload in a bit. We will be using CLOG because it is no fun in 2022 to make believe we are using black and white CRTs[^3] when we can use a **C**ommon **L**isp **O**mificient **G**ui from the start.

[^2]: As these are tutorials, they are to practically get you coding and playing with Common Lisp and CLOG. It is an unbelievably rich language with many brilliant ideas. Invest the time into learning Lisp properly with one of the many free books and resources available.
[^3]: I was 8 when my father got me my first computer, a TRS-80 model 4. It had just come on the market and I was so excited, 64k memory!! The was double the ram of the computers in the lab at school. My father got me the deluxe model that came with 2 dual density 5 ¼" floppys 360K each! I spent every waking hour not at school glued to that machine and listening to my father scream how he could have gotten a Cadillac for the same price. My eyes were so messed from the CRT, between dry eyes, twitches, etc. Thankfully it seems to have left no long term effects on me… at least in the eyes.

## Navigating your Lisp Image

At this point our Lisp Image is alive and kicking with many cool packages (we are going to start exploring today). To change the default package we use ```IN-PACKAGE``` (by convention we copy the default behavior of the reader from our REPL and capitalize symbols when referring to them).

E.g. To change from ```CL-USER``` to ```CLOG-USER``` we use:

```lisp
(in-package "CLOG-USER")
```

And now our default package is as indicated by our REPL:

```lisp
CLOG-USER>
```

By convention we generally define our packages in all upper case (we will discuss defining packages soon). There is a practical reason for this which we shall learn soon.

There is a special package called ```KEYWORD```. Any symbol starting with a colon ```:``` is treated as a symbol from the ```KEYWORD``` package and is local to all packages. Like all symbols by default the reader upcases them.

```lisp
CLOG-USER> :a_symbol
:A_SYMBOL
```

You can use a keyword symbol with ```IN-PACKAGE``` instead of a string and it will be turned into a string.

```lisp
CLOG-USER> (in-package :clog-user)
#<PACKAGE "CLOG-USER">
```

Another alternative that can be used in ```IN-PACKAGE``` that you will see frequently is an _uninterned_ symbol, i.e. a symbol that has no home package and is written ```#:package``` so the following is also valid:

```lisp
(in-package #:clog-user)
#<PACKAGE "CLOG-USER">
```

I tend to use keywords[^4], but to each their own. Your Lisp image is your little creation.

It is a good practice though to fully qualify IN-PACKAGE with it's home package CL. Let see why:

```lisp
CLOG-USER> (in-package :keyword)
#<COMMON-LISP:PACKAGE "KEYWORD">
KEYWORD> (in-package :clog-user)
; Evaluation aborted on #<UNDEFINED-FUNCTION IN-PACKAGE {1005227763}>.
```

The package KEYWORD does not use the CL package (which is a nickname for the package COMMON-LISP) that contains the symbols with the Common Lisp language. The way out then is:

```lisp
KEYWORD> (cl:in-package :clog-user)
#<PACKAGE "CLOG-USER">
CLOG-USER>
```


[^4]: Using symbols to represent package names (which are strings in reality) keeps your code from "SCREAMING" at you and keywords are not affected by the current package. Using uninterned symbols works as well for this but I never got in the habit and see no need to.

## Creating Packages

Now that we can navigate our Lisp image, let's create our first package.

```lisp
CL-USER> (defpackage :hello-package  ; create the package
           (:nicknames :hello-pkg)   ; alternate name
           (:use :cl :clog)          ; other packages to make local
           (:export :hello-world))   ; symbols exposed to the world
#<PACKAGE "HELLO-PACKAGE">
CL-USER> (in-package :hello-package) ; make our package the current
#<PACKAGE "HELLO-PACKAGE">           ; default
HELLO-PKG>
```

The comments give us a play-by-play.

Export is our "interface" or "protocol" to our package, ie what symbols will be available publicly in the lisp image and so accessible using the fully qualified symbol hello-package:hello-world or in any DEFPACKAGE including hello-package in its :use without needing to specify its home package, i.e. hello-package and can use just hello-world.

Keep in mind that an export, exports the symbol. Symbols can mean many things, whatever it is now, it is publicly accessible. In Lisp, there is no strict enforcement of public and private. It is possible to access any symbol in a package by using two colons instead of one. So hello-package::hello-private would give access to the non-exported hello-private symbol.

Let's define two functions for the symbols we have talked about so far.

```lisp
HELLO-PKG> (defun hello-private (body)
             "Create a div on new pages containing - hello world"
             (create-div body :content "hello world"))
HELLO-PRIVATE
HELLO-PKG> (defun hello-world ()
             "Initialize CLOG and open a browser"
             (initialize 'hello-private)
             (open-browser))
HELLO-WORLD
```

Even though we haven't yet explained the details of these functions, it is clear that violating the protocol and calling the function hello-private would be a mistake, as hello-world is where the initialization is taking place. So with great power comes great responsibility.

## ASDF Systems and QuickLisp

Packages allow us to develop the internals of our "system", i.e. our Lisp image. ASDF and QuickLisp provide the means to make that system reproducible from outside the Lisp image.

ASDF allows us to define where our code is located in the real world and how to reconstruct the Lisp image from scratch. QuickLisp sits on top of ASDF and retrieves if needed over the internet any dependencies of your system and any dependencies of those dependencies. There are other options to ASDF and QuickLisp but for most needs they do an excellent job.

Let's turn the bit of code we created before into a full system.

1. We need to create a directory for our system in a location ASDF knows about. The directory ```~/common-lisp``` is built in and what we will use. (If using portacle portacle\projects). The directory ```~/.quicklisp/local-projects``` is also configured on many systems and you can add any directory by running in the Lisp image as well``` (push #P"path/to/dir/of/projects" ql:*local-project-directories*)```.
2. Create the directory hello-sys in ```~/common-lisp```.
3. Next we need to copy our code we typed before into a file. ```C-x C-f``` and create the file ```~/common-lisp/helllo-sys/hello.lisp``` and copy and paste to create:

```lisp
(defpackage :hello-package
  (:nicknames :hello-pkg)
  (:use :cl :clog)
  (:export :hello-world))

(in-package :hello-package)

(defun hello-private (body)
  "Create a div on new pages containing - hello world"
  (create-div body :content "hello world"))

(defun hello-world ()
  "Initialize CLOG and open a browser"
  (initialize 'hello-private)
  (open-browser))
```

4. Next we need to create our .asd file that tells ASDF what files to load into a list system and what the dependencies are. It needs to have the same name as our directory which is the name of the system, so the file name is ```~/common-lisp/hello-sys/hello-sys.asd``` and the contest is:

```lisp
(asdf:defsystem #:hello-sys
  :description "Common Lisp - The Tutorial Part 3"

  :author "david@botton.com"
  :license  "BSD"
  :version "0.0.0"
  :serial t
  :depends-on (#:clog)
  :components ((:file "hello"))) ; <- notice no .lisp used
```


5. Let's let ASDF recalculate the available systems - ```(asdf:clear-source-registry)``` or we can reset our lisp image using ```M-x slime-restart-inferior-lisp```.
6. The we request QuickLisp to load our brand new system - ```(ql:quickload :hello-sys)```.

To load "hello-sys":
  Load 1 ASDF system:
    hello-sys
; Loading "hello-sys"
..................................................
[package hello-package]
(:HELLO-SYS)

7. We can now try out our complete system:

```lisp
CL-USER> (hello-pkg:hello-world)
Hunchentoot server is started.
Listening on 0.0.0.0:8080.
HTTP listening on    : 0.0.0.0:8080
HTML Root            : /home/dbotton/common-lisp/clog/./static-files/
Boot js source       : compiled in
Boot file for path / : /boot.html
NIL
NIL
0
```

A browser should open on most computers. If it does not go to [http://127.0.0.1:8080](http://127.0.0.1:8080) and you will see the famous words ```Hello World```.


## Summary

### From Part 1

1. Lisp uses “LISt Processing.”
2. The first element of the list is an operator and the remainder of the list are its arguments.

### From Part 2

3. Lisp has a different development cycle than other languages and a different model of development, programs are grown.
4. The top level of structure in Lisp programs is the package, which organizes symbols, not files.
5. A fully qualified symbol is coded as ```package-name:symbol-name``` but if ```package-name``` is the same as the current default package you can refer to the symbol-name alone.
6. Symbols can name functions, variables, and more.
7. Symbols can also be used as a data type as well by placing an apostrophe before the symbol name.

### From Part 3

8. We navigate between packages using ```IN-PACKAGE```.
9. We define packages using ```DEFPACKAGE```.
10. A system is defined by a directory named the same as an .asd configuration file that describes what files should be loaded in a lisp image and what are the dependencies.
11. A system is loaded into the Lisp image with ```(ql:quickload :system-name)```.

