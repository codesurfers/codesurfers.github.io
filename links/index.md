---
layout: article
title: links
---

## Programming

### Basic Tutorials

Python is a great choice to start programming in. It is a
well-designed language with an emphasis on readability. There are a
multitude of resources and libraries to achieve almost any task
imaginable.

Later on, you can explore other languages, comparing and contrasting
them with your technical judgement and taste.

[The Python Tutorial](http://docs.python.org/3/tutorial/)

The official tutorial for programming in Python. Having some
programming experience will help you to move through the material
presented here at a faster pace.

### Language Specific Resources

#### JavaScript

[Eloquent JavaScript](http://eloquentjavascript.net/)

A modern introduction to web development using JavaScript.

#### Haskell

[Learn you a Haskell for great good](http://learnyouahaskell.com/chapters)

A nicely illustrated book on functional programming in
Haskell.

[Learning Haskell](http://www.haskell.org/haskellwiki/Learning_Haskell)

[What I Wish I Knew When Learning Haskell 2.0](http://dev.stephendiehl.com/hask/)

[A Haskell Reading List](http://www.stephendiehl.com/posts/essential_haskell.html)

[Typeclassopedia](http://www.haskell.org/haskellwiki/Typeclassopedia)

#### OCaml

[Awesome OCaml](https://github.com/rizo/awesome-ocaml/)

A collection of OCaml resources.

[Real World OCaml](https://realworldocaml.org/)

A book about OCaml, a fast, functional programming language used in
research and industry. A new edition is in the works.

#### Lisp

Aaahhhh, the Lisp family of languages. Features of this family include
expressivity, interactive-development, and macros.

Every Lisp language has its strengths and weaknesses, and the
tradeoffs need to be carefully considered.

##### Modern Lisps

[Janet](https://janet-lang.org/)

A modern Clojure-like Lisp without the JVM baggage designed for easy
embeddability.

[Clojure](https://clojure.org/)

Clojure is a modern Lisp on the JVM (and the browser in the form of
ClojureScript), which incorporates great ideas from multiple
paradigms. Easy interoperability with the large number of available
Java libraries is an added bonus.

It features persistent data structures, a well-designed sequence
abstraction, great support for concurrency, and to round it off, good
performance on the JVM.

The error handling and interactivity may not be as good as Common
Lisp, but it doesn't have all of Common Lisp's warts and historical
baggage.

[Racket](http://racket-lang.org/)

A modern dialect of Scheme, which is gradually becoming the premier
Scheme-ish language. An interesting feature of Racket is that it has
several "sublanguages".

##### Common Lisp

[A Road to Common Lisp](https://stevelosh.com/blog/2018/08/a-road-to-common-lisp/)

Steve Losh's recommendations on how to learn Common Lisp.

[Practical Common Lisp](http://www.gigamonkeys.com/book/)

An introduction to Common Lisp describing the construction
of practical, real world programs.

[Articulate Common Lisp](http://articulate-lisp.com/)

A collection of CL resources.

#### Programming languages

[Rust](http://www.rust-lang.org/)

A systems programming language that seems to keep winning Stack
Overflow's "Most Loved Programming Language" year after year.

[Prolog](https://en.wikipedia.org/wiki/Prolog)

A logic-programming language. You are probably best off starting with
[SWI-Prolog](https://www.swi-prolog.org/).

[Raku](https://raku.org/)

The language formerly known as Perl 6. At some point the Perl
community decided to create a "new" version of Perl i.e. Perl 6. As
the changes became more and more backwards-incompatible, they realised
that they were designing an all-new sister language. In 2020, after
much discussion and Larry Wall's blessing, Perl 6 was renamed to Raku.

Raku is an expressive, gradually typed, multi-paradigm language
drawing from the rich history and
roll-up-your-sleeves-and-get-your-hands-dirty hacker-ethos of Perl
with the explicit goal of being a fun language to program in.

Notable features include first-class support for grammars, modern
concurrency primitives, a MOP, being able to easily define arbitrary
operators, and keeping in line with Perl's legacy, all new regexes.


### Programming Language Theory

[Lambda the Ultimate](http://lambda-the-ultimate.org/)

A blog and community for programming language enthusiasts.

[Oleg Kiselyov's site](http://okmij.org/ftp/)

Pro research.

### Networks

[Beej's Guide to Network Programming Using Internet
  Sockets](http://beej.us/guide/bgnet/output/html/singlepage/bgnet.html)

A humorous guide to get started with network programming in C.

## CS

### Things to learn

[What every computer science major should
know](http://matt.might.net/articles/what-cs-majors-should-know/)

Matt Might's advice on obtaining the essential education any student
of CS ought to know.

## Hiring/Interviews

[Alex Bowe's interview prep advice](http://alexbowe.com/failing-at-google-interviews/)

[Steve Yegge's interview prep advice](http://steve-yegge.blogspot.in/2008/03/get-that-job-at-google.html)

## Tools

### General Programmer Tools

You will have to be familiar with the development tools like
the compiler/interpreter and debugger because you'll be
spending a sizeable amount of your time working with them.

[gdb](https://www.gnu.org/software/gdb/)

The GNU Debugger can help with finding what's going wrong
with your program. It's usually used to debug C/C++
programs.

[ccache](https://ccache.samba.org/)

Building large projects can take a significant amount of
time. ccache can help with reducing the time spent
waiting for the build to finish.

### Text editors

Knowing how to efficiently use a text editor is one of the most useful
secondary skills of any programmer.

Among the free text editors, vim and emacs have (unfortunately) been
the state of the art for quite some time now.

[Vim](http://www.vim.org/others.php)

A modal text editor that has nice key bindings and an
emphasis on speed. It is an important command-line
survival skill. Once installed, run the "vimtutor" command
to start a basic tutorial.

[Emacs](http://www.gnu.org/software/emacs/tour/)

An operating system containing, amongst many other things, a file
manager, calculator, games, package manager and (arguably) a text
editor.

It can be extended in trivial, and non-trivial ways with Emacs Lisp,
which also happens to be the language it is written in.

Many users of Emacs suffer from the dreaded "Emacs Pinky" due to
having to press down the Control key (or the Caps Lock key for that
matter) for almost every command.

This can be alleviated to some extent by adding support of another
modifier key like "Hyper", to reduce dependence on the Control key.

Users who prefer Vim's more ergonomic keybindings have a lot of
options in making Emacs work that way.

[VS Code](https://code.visualstudio.com/)

A modern open-source text editor developed by Microsoft.

[Notepad++](http://www.notepad-plus-plus.org/)

An easy to use, feature-laden text editor.

### Version Control

A Version Control System (VCS) is used to track changes in
files. Even the most trivial projects can quickly become
unmanageable if a VCS is not used.

[Pro Git](http://git-scm.com/book)

A solid introduction to the powerful Git version control system.

[Mercurial](http://hginit.com/01.html)

Mercurial is easier to learn and use when compared to
Git. This tutorial covers the fundamental idea behind version control
and basic usage of Mercurial.

[Commit message guidelines](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

It is important to write good commit messages so that commit history
is readable.

### Command-line

Mastering the command line shell gives you unfathomable power and more
importantly nerd cred. ;-)

[zsh](https://wiki.archlinux.org/index.php/Zsh)

A shell with nice completions and other customisable features.

[tmux](http://tmux.sourceforge.net/)

Switching between multiple terminal emulator instances can
hamper productivity. This terminal multiplexer can be used
with a tiling window manager for a killer combination.

[GNU coreutils](https://en.wikipedia.org/wiki/GNU_Core_Utilities)

The GNU coreutils bundled with GNU/Linux can be used to
perform straighforward tasks, and also arcane
wizardry.

For as long as text terminals are used, it will be worth your while to
be comfortable with using these.

[hh](https://github.com/dvorka/hstr)

Allows you to easily view, navigate, search and manage your command
history.

[fzf](https://github.com/junegunn/fzf)

A command line fuzzy finder.

[ripgrep](https://github.com/BurntSushi/ripgrep)

Similar to other popular search tools like The Silver Searcher, ack
and grep which can be used to search directories recursively.

### Window Managers

#### Tiling

Tiling window managers are useful in a typical programming session
when you have to repeatedly switch between your text-editor, terminal
emulator, web-browser, and any number of other programs you may have
running. Alt-Tab never again!

[xmonad](http://xmonad.org/)

A tiling window manager written in Haskell.

### Graphics

[Inkscape](http://tavmjong.free.fr/INKSCAPE/MANUAL/html/)

The best open source SVG editor there is. Also allows importing from,
exporting to the commonly used graphical formats.

### Miscellaneous

[Pandoc](http://johnmacfarlane.net/pandoc/index.html)

Easily converts between a great variety of markup formats.

## CS Theory

### Algorithms

[Big-O Cheatsheet](http://bigocheatsheet.com/)

A handy cheatsheet of algorithmic complexities associated with
commonly used data structures and algorithms.

## References

Learning to use references is one of the essential skills of a
programmer. References are by necessity large and intimidating, but
with time they become indispensable.

### Programming Language References

#### C++

[C++ Reference](http://en.cppreference.com/w/)

Handy quick reference while writing C++ code. It can be
downloaded for offline use.

[C++ Reference](http://www.cplusplus.com/reference/)

Another reference on C++. It has clear examples to help
you learn how to use the standard library.

[C++ FAQ](http://www.parashift.com/c++-faq/)

Fairly large collection of C++ FAQs and their answers.

[C++ FQA](http://www.yosefk.com/c++fqa/index.html)

C++ Frequently Questioned Answers.

[Official C++ FAQ](http://isocpp.org/wiki/faq)

#### Common Lisp

[Simplified Common Lisp Reference](http://jtra.cz/stuff/lisp/sclr/index.html)

A selection of commonly used symbols.

[Common Lisp HyperSpec™](http://www.lispworks.com/documentation/HyperSpec/Front/index.htm)

The entire ANSI CL standard in HTML. Rather readable.

### Protocols, Specifications

[RFC](http://www.rfc-editor.org/rfc-index2.html)

Freely available specifications for various internet-related protocols.

## Programming Exercises

[Project Euler](http://projecteuler.net/)

A good source of many exercise problems. Many of the
problems here require some mathematical (and programming)
knowledge.

[Programming Praxis](http://programmingpraxis.com/)

Some practical programming problems in addition to the sorts of
problems found at [Project Euler](http://projecteuler.net/).

[Problem of the Day](http://www.problemotd.com/)

Fun exercises.

## Operating Systems

Any operating system that you are productive in, and happy
with, is fine. If that is not the case, however, these are
some of our recommendations.

[Debian](http://www.debian.org/)

[neena](http://neena.at/) says that it is a decent operating system. A
widely used, stable Linux distribution.

[Arch Linux](https://www.archlinux.org/)

A Linux distribution with an emphasis on simplicity, configurability
and having the latest software packages. Rolling releases keep your
system at the bleeding edge.

[FreeBSD](https://www.freebsd.org/)

It is worth checking out some BSDs as well. Features include
first-class support for ZFS, great networking software, excellent
documentation, no dependence on systemd, and the BSD license for those
who prefer it to Linux's GPL.

From what we can gather, it works better on slightly older hardware as
all the latest and greatest drivers may not be well-supported.

BSDs are used in many sites for critical networking infrastructure
such as firewalls.

## Miscellaneous

### Articles

[How to Become a Hacker](http://www.catb.org/esr/faqs/hacker-howto.html)

A good introduction to what it means to be a hacker, and how to get
there.

[What I Tell All New Programmers](http://josephg.com/blog/what-i-tell-all-new-programmers/)

Good advice that new programmers will do well to follow.

[Teach Yourself Programming in Ten Years](http://norvig.com/21-days.html)

Check out his other articles, as well.

[Joe Armstrong's Advice](http://erlang.org/pipermail/erlang-questions/2013-January/071944.html)

Joe Armstrong's recommendations as to what might be worth learning.

[How To Ask Questions The Smart Way](http://www.catb.org/esr/faqs/smart-questions.html)

It's a good idea to read this before asking questions on the internet (and in real life).

[You and Your Research](http://www.cs.virginia.edu/%7Erobins/YouAndYourResearch.html)

Fascinating insights into the life experiences of a
scientist, and his colleagues.

[Programming Language Comparison by Mike Vanier](http://users.cms.caltech.edu/~mvanier/hacking/programming.html)

Slightly dated, but still a good comparison between the various
languages the author has used in his career.

### Books

#### Algorithms

[Programming Algorithms](https://lisp-univ-etc.blogspot.com/2019/07/programming-algorithms-book.html)

A freely available book on Programming Algorithms. Common Lisp is used
as the implementation language, which is a refreshing change.

#### Distributed Systems

[Designing Data-Intensive Applications](https://dataintensive.net/)

Any one working with backend web applications, database infrastructure
will need to have a good understanding of distributed systems, for
which this book is the premier resource for working professionals. An
absolute must-read that will pay dividends many times over in your
career.

#### Introductory programming

[The C Programming Language](https://en.wikipedia.org/wiki/The_C_Programming_Language)

An excellent way to learn C, a widely used, influential programming
language.

[Structure and Interpretation of Computer Programs
  (SICP)](http://mitpress.mit.edu/sicp/full-text/book/book.html)

An enlightening introduction to computation and
programming. [Racket](http://racket-lang.org/) is a good choice for
running the example code and doing the exercises from the book as it
has a sublanguage for that specific dialect of Scheme.

#### Lisp Books

[Paradigms of Artificial Intelligence Programming: Case Studies in Common Lisp](https://github.com/norvig/paip-lisp)

Peter Norvig has kindly made this book freely available which
deals with implementing classical AI algorithms in Common Lisp.

While we are on the subject of Mr. Norvig, his articles on [Solving
Every Sudoku Puzzle](http://www.norvig.com/sudoku.html) and [How to
Write a Spelling Corrector](http://www.norvig.com/spell-correct.html)
are legendary. It is manifest how powerful and elegant programming can
be when performed by a master. Any programmer who can write a
technical article of a similar quality to these two, should be pleased
with themselves.
