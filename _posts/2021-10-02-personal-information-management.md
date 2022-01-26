---
layout: post
author: Samuel Chase <samebchase@gmail.com>
title:  "Personal Information Management"
date:   2021-10-02
categories:
---

When I was a greenhorn employee, I was once asked by a colleague:
"What do you use as a to-do list?" and my naïve reply of
`<FULL-FEATURED-ENTERPRISE-TICKETING-SYSTEM>` caused him and another
engineer to burst out laughing at my uninitiation.

At that point, I recognised the lack of a "real" system of managing my
tasks and local information. Since then, I have tried various tools
and approaches, and will mention what I have currently settled on, and
what has proven to work over an extended period of time.


## Requirements

Should be in a non-proprietary format.

Syncing is an anti-feature. I'd rather not have them reside on an
external system with the attendant security risks.

All information should be stored locally on the work laptop.

I don't require it to be accessible from my personal phone either.

Should be blazing fast.

Should NOT require an internet connection.


## Pen and Paper

After all these years and advancements, it is likely we may never come
up with a better system of taking notes, in a similar way that the
only time-tested writing material is vellum (parchment).

Writing on real paper is the most enjoyable form of writing, and the
gold standard that all other methods aspire to.

You just can't doodle in an IDE. Sometimes in a boring meeting,
doodling is what might help you get through it. Having a good pen and
paper nearby is essential for those moments. It would be foolish to
dismiss Pen and Paper.


## A place for digital PostIt™ Notes

I had tried out and used [void](https://github.com/void-rs/void) as a
terminal task list and as a place to quickly store short snippets of
text, but I started experienced the limitations and frustrations of
terminal-only UI. Reluctantly, I moved off it. In any case, it's worth
checking out for sure.

Later on, I came across [TreeSheets](https://strlen.com/treesheets/)
on the Orange Site. You can think of it as the lovechild of
[Org](https://orgmode.org/) and Spreadsheet software.

TreeSheets is used as a digital whiteboard and as a graphical
outlining tool. It is remarkably powerful, and if there is one tool
out of all the ones that I mention in this article that you should
take a look at, it is TreeSheets.

When I take interviews, I have the entire interview outline in a
TreeSheets document, that I keep referring to structure the interview,
and also take notes as required.

What I particularly like about TreeSheets, is that the author clearly
uses it has his daily driver, and it shows with the smooth UI,
performance and easy to learn keyboard shortcuts.

While it does use a binary format, it is
[documented](https://github.com/aardappel/treesheets/blob/master/TS/docs/file_format_spec.txt)
and the author assures us that it should not be too difficult to write
a parser for it. A small matter of programming.

In terms of tackling writer's block, I find TreeSheets helps me put
down the words faster than Org. A quick outline and mindmap can be
exported as plaintext and fleshed out further in Org.

### Future Steps

TreeSheets has a good exporting feature to various formats, but as an
Org user, I would very much like to see Org export and import which
would open up game-changing functionality where I can have the option
to prototype a document either as an Org document in Emacs, or as a
TreeSheets document, and seamlessly move back and forth as necessary.


## A knowledge base and a cheatsheet

Early on, I used to store short code and shell snippets useful
information in an Org file for easy copy/pasting. For whatever reason,
I don't like searching for stuff across multiple Org files, or in
Emacs in general, so was looking for something that I can read in a
web browser.

After reading an
[article](https://wiki.nikitavoloboev.xyz/sharing/everything-i-know)
by Nikita Voloboev, I decided that I would like to start using wiki
software in a similar manner.

There are many lightweight wikis out there, but for now I have settled
on [Fossil
Wiki](https://fossil-scm.org/home/doc/trunk/www/wikitheory.wiki), and
I've been using it consistently for close to two years.

You may know Fossil Wiki as the wiki that comes packaged with the
[Fossil](https://fossil-scm.org/) Version Control System written by
the same author of the all-conquering SQLite database.

[DRH](https://en.wikipedia.org/wiki/D._Richard_Hipp) wrote Fossil VCS
to manage the development of SQLite, and Fossil itself uses SQLite
under the hood as its data format, and—you know what I'm going to say
next—Fossil VCS is **also** used to manage _its_ own development which is
a delightful example of bootstrapping and dogfooding.

### The Bad

I want to get the bad things out of the way first. I'm pleased to say
there is very little I find bad about Fossil Wiki. However, I believe
others may find its spartan UI, and seemingly simplistic array of
features to be underwhelming compared to modern note taking web
applications.

After using editors like Emacs and Vim, no text input box on the web
is going to be satisfactory. The Fossil Wiki editor is barebones in
both a good way and a bad way. Ideally, I would like to edit markdown
Fossil Wiki pages in Emacs, but unfortunately no one has written that
integration as yet. 😉

Another thing that could be explored in this area is to integrate
[CodeMirror](https://codemirror.net/) (or
[ProseMirror](https://prosemirror.net/) for that matter) into it, so
that the editing experience is more full featured.

There is no syntax highlighting for code blocks, but I have gotten used
to the plain code formatting, so this is a non-issue for me. The main
problem I find with syntax highlighting on the browser, is that many
times it does not go well with the colour scheme of the page, so this
is one annoyance that is eliminated altogether.

The only reason I tolerate the underwhelming editing experience, is
because I spend much more time reading my Fossil Wiki'd notes than
writing pages.

### The Good

Other than the minor issues highlighted above, everything else is
razor sharp and butter smooth. This is one of my favourite pieces of
software in terms of how performant, lightweight, and the pure
pleasure of using it.

Over a period of almost two years, I have created a few dozen pages in
my local Fossil Wiki that I frequently refer to like a cheatsheet of
sorts.

For example, I have a page called MongoDB, where I write sample
queries. For any tricky query or operation, I have sample code that I
refer when I need to. It is written in such a way that it _can_ be
read hundreds of times (and by now, I may well have). It's almost a
lightweight exo brain.

Another thing was that I wanted to be able to easily copy/paste
snippets and sections and send it to my co-workers, for them to use in
the moment, or as skeleton markdown documents that they can insert
into their own note-taking system and extend as they wish.

Fossil Wiki has more functionality than that a single user
requires. For example, it has integrated chat and forum functionality
which I could use to talk to myself, but it is impossible to
unironically do so.

#### Notable mention: pikchr

A hidden gem of Fossil Wiki is its own diagramming tool called
[pikchr](https://pikchr.org/). It takes some getting used to, but once
the basics are mastered, you can make no-nonsense technical
diagrams. The language is remarkably full featured, ergonomic and well
thought through; this is usually the case with any DRH-ware.

When required, pikchr can be used to quickly create diagrams without
having to fiddle around with proprietary tools.

As a new parent, I am willing to do something that requires me to
concentrate intensely for a short period of time, rather than spend an
extended amount of time fiddling around with something that doesn't
require me to concentrate. pikchr hits the sweet spot, between
difficulty and effectiveness.

I have made many diagrams in my wiki pages. These diagrams help me
absorb and remember the material better.

## Task management, and general notetaking

Now, we come to the venerable [Org Mode](https://orgmode.org/). Org is
so good, that you could use
[Emacs](https://www.gnu.org/software/emacs/) for just that
functionality alone. I have been using Org for a long time now, but
until recently never Org'd all-the-things.

### My frustrations with Org Mode

While I heavily use Org mode for my workflows, but I frequently get
the feeling that while it may be the best place to /write/ notes in,
it may not be the best to read notes in. I don't like reading lots of
plain text in a text editor. Of course, I enjoy using Emacs, but
sometimes, I want to read information in a web browser instead of
having to always open up Emacs, and then switch to a buffer and then
copy/paste (I have bound this to a easy shortcut using the Super key
(s-c, s-v), but still, I dislike copy/paste in Emacs and can't seem to
shake off the clunky feeling.)

### Task Tracking

Emacs and Org are similar in the sense that you're only going to get
your money's worth if you buy into it fully. They are both meant to be
invested in deeply, and it is only until that is done where you unlock
certain superpowers which makes you feel like a wizard, and others
tremble/recoil in awe/disgust.

After using a lot of task tracking software, I have finally settled on
Org. I add tasks to various Org files, and schedule them so that the
tasks show up in my Agenda view. Originally, I was resistant to the
idea of clocking in and clocking out to track the time, but I reached
a point in my quest for productivity, that if I want to improve on
something, I need to measure it.

This is working great as a daily TODO list, and I have a rough
overview of my day, and week in this manner using the Agenda
view. Having many Org files is not a problem, as the Agenda view
allows me to easily jump to the task by simply hitting TAB.

### Extended Note Taking

All my project and task notes are stored in various Org files. For
some projects, I've been gradually adding to its notes file for
years. This allows me to glance through the entire history of the
project and its evolution across multiple versions.

While writing code or performing some debugging or maintenance task,
we need a rough working area where we can save snippets of code, data,
configuration and general brain-dump notes so that we can pick up
where we left off after we've context-switched back from the latest
interrupt.

Org Babel is particularly suited to this task of storing code snippets
in a multitude of languages. I haven't yet explored the literate
programming capabilities in which Org can be a lightweight Jupyter
Notebook. Python seems to be well supported, but my favourite
languages are not, unfortunately.

### What about advanced [Zettelkasten](https://zettelkasten.de/) exo-brain software?

For now, I don't need advanced functionality. The information I am
saving is not that complicated, so I'll stick with the existing setup
until I hit its limits.
	

## Bibliography management, Bookmarking

Whenever, I come across a good resource like an web article, academic
paper, text book, I store it in
[Zotero](https://www.zotero.org/). After more than two years of doing
this semi-regularly, I've academically
[tsundoku'd](https://en.wikipedia.org/wiki/Tsundoku) myself. I shake
off the mild disappointment, every time I share relevant literature
for anyone looking for it from my curated collection. I guess, it is
useful, for _that_ at least.

I used to hit "Add to Bookmarks" in the browser, but the browser
search bars are more interested in taking the data you type and giving
it to a search engine rather than providing even the most rudimentary
local search functionality based on your visited webpages and
bookmarks. This is why I felt that I cannot rely on browser bookmarks,
and went with heavyweight Bibliography Management software.


## Clipboard Management

In more than 10 years of professional computer usage, I am embarrassed
to say, that I have not used a Clipboard manager until only about a
year ago. 

Find a Clipboard manager that you like and use it. It's supercharges
your copy/paste skills.

At work, I use [Maccy](https://maccy.app/) which works well enough. I
haven't found a portable Clipboard manager that works across all my
OSes (OpenBSD Desktop and NixOS Laptop), so would appreciate any
recommendations in this area.


## Summary

My information is managed with a combination of Org, Fossil Wiki,
TreeSheets, Zotero, Clipboard Manager and the occasional scribble on
chequered paper.

This complicated "system" is working well enough, and it allows me to
be entirely local and self-sufficient with no external dependencies.

Thanks for reading. I would like to hear what PIM system works for
you. 

Send me an email or toot at me on
[@samebchase@mastodon.social](https://mastodon.social/@samebchase/)!
