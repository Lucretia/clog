# Common Lisp - The Tutorial Part 1: The Journey

## Introduction

> If CL is ugly, why do I use it and write about it? Because Lisp
> is so powerful that even an ugly Lisp is preferable to using some
> other language.
>
> -- <cite>Apr 30, 2002 - C.L.L - Paul Graham</cite>

## Introduction

Today we are going to embark on a journey together. I, a person with poor skills at writing[^1] and the author of CLOG, the most Awesome GUI and Web Framework on the planet as of 2022[^2], and you, an inquisitive individual looking to learn Common Lisp, CLOG and/or kabbalistic software design[^3]. The goal of this journey is to quickly get you up to speed with enough Common Lisp to use this grotesquely beautiful language[^4] with millions of parentheses[^5] to write awesome software (with CLOG I hope), faster and more powerful than with any other language[^6].

[^1]: If only word processors had compilers, debuggers and profilers.
[^2]:  I know this to be true since CLOG is more awesome than [GNOGA](https://github.com/alire-project/gnoga) my Ada version from 2013 and until CLOG came around nothing was as awesome as it. I can also say this since there are only a few frameworks I know of that are designed from scratch for both web and gui use. Oh, one last proof, this fact has a footnote and is now on the internet so must be true.
[^3]: Which doesn't exist but if it did it would be in a Hebrew version of Lisp.
[^4]: Beauty is in the eye of the beholder, but Jean Ichbiah was an artist. To elaborate further, the author of Lisp was John McCarthy, a Litvach (a Lithuanian Ashkenazi Jew) and Jean Ichbiah was the author of Ada (a Turkish Sefardic Jew). In the world of jews the litvachs are stereotyped as the scientific types and the sefardic the artistic type, so you see this is all a very scientific analysis. I am a mixed breed so I write in Ada and Lisp.
[^5]: In 1987, I wanted to teach myself about AI programming. I was 11 then but I had already written very cool software in Assembly, Basic, Pascal, and lots of other languages but wanted more. So I asked Jack, the computer teacher at Nova University (they still have a k-12 division) who got me started programming with a bribe for the formula for how to draw circles if I could prove I would know what to do with it, what the best language would be, he said "prolog or Lisp?" I took one look at the parentheses and grabbed the prolog floppy.
[^6]: "Lisp is no harder to understand than other languages. So if you have never learned to program, and you want to start, start with Lisp." RMS.

## Rules of the Journey

### 1. Ignore the parentheses and see only the indentations.

The heart of Lisp is the S-expressions (aka the sexp[^7]). A parenthesis followed by an operator followed by arguments and closed with another parenthesis. So ```(+ 1 2)``` results in a ```3``` and in almost every other language is ```1 + 2```. Data is also expressed in the same way e.g.``` (list 1 2 3)``` the list containing four elements list ```1```, ```2``` and ```3```, in memory it is stored as ```1```, ```2``` and ```3```. The expressions nest ```(list 1 2 (list 2 4 (list 1 3) 4) 3)``` etc. This means lots and lots of parenthesis.

However, the human brain can parse them with the help of white space[^8]:

```lisp
(tagbody
  10 (print "Hello")
  20 (go 10))
```

In fact once you start looking at the indentation and forgetting about the parentheses Lisp starts to look like most languages.

```lisp
(defun factorial (x)
  (if (zerop x)
    1
    (* x (factorial (- x 1)))))
```

This is an actual C program from the 1st International Obfuscated C Code Contest (1984)

```c
a[900];       b;c;d=1       ;e=1;f;       g;h;O;        main(k,
l)char*       *l;{g=        atoi(*        ++l);         for(k=
0;k*k<        g;b=k         ++>>1)        ;for(h=       0;h*h<=
g;++h);       --h;c=(       (h+=g>h       *(h+1))       -1)>>1;
while(d       <=g){         ++O;for       (f=0;f<       O&&d<=g
;++f)a[       b<<5|c]       =d++,b+=      e;for(        f=0;f<O
&&d<=g;       ++f)a[b       <<5|c]=       d++,c+=       e;e= -e
;}for(c       =0;c<h;       ++c){         for(b=0       ;b<k;++
b){if(b       <k/2)a[       b<<5|c]       ^=a[(k        -(b+1))
<<5|c]^=      a[b<<5        |c]^=a[       (k-(b+1       ))<<5|c]
;printf(      a[b<<5|c      ]?"%-4d"     :"    "        ,a[b<<5
|c]);}        putchar(      '\n');}}     /*Mike         Laman*/
```

The simple regular syntax of Lisp and its use of parentheses is part of what makes Lisp so powerful, it is called homoiconicity. For now take my word on this, but it is omnipotent power made touchable by parentheses.

There is no place for discrimination! Do not judge a language based on its parentheses!

[^7]: Lisp really does have sexapeal.
[^8]: Yes Lisp is from the 1960's and the classics like  - 10 print "hello" 20 goto 10 was super cool and Lisp  was served on punch cards too. Lisp, a little known fact,  introduced throughout the history of software engineering most cool™ concepts we take for granted like if-then-else (as cond), automatic garbage collection, and was used for the first implementation of JavaScript (which is very Lispy).



### 2. "Clear your mind, use the force Luke!" (Star Wars)

Lisp is a multi paradigm language. If it is a buzzword, Lisp invented it, has it (and did it better), or a few lines of code and will have it (it is the programmable language by design)[^9]. It is not a mistake to write code using any paradigm as long as it is crisp and clean and no caffeine[^10], i.e. readable and fitting the domain[^11], error free and gets the job done[^12].

* Functional - You bet, Lisp and Alonzo Church gave meaning to the letter lambda[^13].
* Object Oriented - Your orientation is accepted here, we are all CLOS
* Procedural - It is our dirty little secret
* Structural - That goto example will haunt me
* Etc etc

[^9]: That is the case in Common Lisp and most Lisps, but not all Lisps are the same. From here on in, when we say Lisp we mean the ugly duckling that made it through years of college in the AI error, standardized as Common Lisp, then 30+ years of real world industrial experi.ence still in production and, because of experimentation with psychedelics, is not all baby skin, but looks awesome even though is the second oldest language in production use after Fortran.
[^10]: 7up, the source of that catchy phrase, is good stuff and not sticky like Sprite. Why are fat people discriminated against and gas stations and best buys only have diet Coke! Only skinny people drink diet cola, we larger folk want flavor, Diet Dr Pepper, Diet Fanta, etc. that is why we are fat!
[^11]: Define abstractions relevant to the problem domain and keep re-using them!
[^12]: I am not a devotee of functional programming. I see nothing bad about side effects and I do not make efforts to avoid them unless there is a practical reason. There is code that is natural to write in a functional way, and code that is more natural with side effects, and I do not campaign about the question." RMS.
[^13]: Some refer to CL as being only functional, yet on some wiki's they run out of disk space with those saying it is not a functional language. Just remember rule #2 clear your mind.


### 3. Stability Matters

Lisp was specified in 1958, its first full compiler in 1962, and evolved into Common Lisp which became ANSI standard in 1994 and has been stable ever since[^14]. The real secret of success[^15] is building a foundation, perfecting it, only when absolutely necessary rewriting it[^16].

The Ada version of CLOG has been around since 2013, CLOG the Common Lisp version is incrementally the same design with much more built on top. Some libraries CLOG sits on are older than most of you.

When you see a github Common Lisp project with a very old date, that just means it is stable and deserves a look.

Experience is fine wine, stability is the tortoise that wins over the hare, always[^17].

[^14]: Common Lisp and Dr. Who have much in common.
[^15]: "The Millionaire Mind" By Thomas J. Stanley inspired this line. Millionaires fix they don't throw.
[^16]: This of course is impossible when the language and libraries change like dirty diapers every year.
[^17]: Jonathan the Seychelles giant tortoise is 190+ years old. 'Nough said.


### 4. Community Matters

I have found in joining the Lisp community fantastic people with tons of experience all willing to contribute their experience and knowledge. They are brutal for their lingo, and rightfully so, beside the only way to communicate succinctly and make sure all on the same page, how we react to rebuke quickly shows who and what we are and if worth spending the time to communicate with.

I frequent ```#commonLisp``` on [Libera](https://libera.chat) thankfully they keep the channel strongly on topic and away from the greatest wastes of time on earth. Governmental politics. I am dbotton there and I have very strong feelings about never being anonymous online and that keeps me from saying things I should regret later[^18].

There is also Lisp discord and [CLiki](https://www.cliki.net) a wiki that will get you to many more resources.

The Open Source movement and Lisp are connected at the hip[^19]. My fellow journeyer I have written insane amounts of free as in freedom lines of code and there is nothing more satisfying than knowing my code contributes to the advancement of us all[^20]. The return with many years of doing this is making a living and loving life (the two rarely go together). Vertical development[^21] is where there is always money to be made, but horizontal development needs to be free as in freedom, and sometimes, but not always, that means doing work for free as in beer.

[^18]: If ever in the Fort Lauderdale area say Hi.
[^19]: [My Lisp Experiences and the Development of GNU Emacs](https://www.gnu.org/gnu/rms-lisp.en.html)
[^20]: Sadly the GPL can be abused by corporations to do great harm, entire languages and communities have been paralized by using the GPL to virus developer's software using their tools to prevent commercial use which harms all developers. I no longer use the GPL (BSD/MIT now)  because of this and still very much respect the idea of keeping the tools free. On the ground though that is not what is being done, GPL was and often is used to poison our tools to prevent Vertical development (see next note).
[^21]: "Vertical" development is for example using CLOG to create a customer website or niche app and "Horizontal" development is general libraries and tools.


### 5. Tools Matter

The success of a language is all about the tools and their open source status. Common Lisp is the most successful of all time with a crazy number of open source compilers (most still maintained!) and commercial compilers each with amazing tooling. Respect your tools. Assuming your code is the cause of the error not the compiler and your compiler will always be your friend.

## Where our journey will take us

As I am trench programmer[^22], I am not the one to write the standard, the manual, the foundation for your development as a programmer, but I can excite you and get you writing Common Lisp with CLOG and that is my focus here.

I hope to cover:

* A minimal set of operators to write business/IT applications.
* Learn basic Lisp idioms
* Get you motivated to look into the depths of Lisp and Software Development in general.
* From early on learn about and how to use parallel computing - yes that is a minimum today!
* And of course learn to be a CLOGer[^23].


[^22]:  I could have a list of self deprecating things about myself and all would be true, but part of my goal with CLOG is attracting people to Common Lisp and to do that with this generation of whippersnappers it means whiz bang graphicy stuff, fresh and new stuff too focus them long enough to see it is worth it to work hard and Lisp will payoff. How You ask? With CLOG. You can already write "Horizontal Apps" faster and in more creative ways than ever before. That means you can turn apps into cash on an individual and corporate level and that means more developers in Common Lisp and that means more of another generation of people to advance the human race.
[^23]: Almost every culture has clogs of one type or another and where there are clogs there are cloggers dancing! A CLOGer loves to program and do cool stuff, for me writing software is my artistic outlet and I want to share my art and inspire others to use it and make their own.

