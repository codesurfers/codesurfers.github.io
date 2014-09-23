---
layout: post
author: Samuel Chase <samebchase@gmail.com>
title:  "Initial Explorations: Clojure"
date:   2014-09-24
categories:
---

Clojure is a dialect of Lisp that is worth studying for its impressive
range of features, its design, and the mindset that one that spends
enough time in the community develops. We shall go over several small
snippets of code to introduce and illustrate the language and some of
its features.

Let's start out by trying to write a function called `length` which
gives us the count of the elements of a list. It's the classic
recursive definition of length - "The length of an empty list is 0,
and the length of a non-empty list is one more than the length of its
tail".

```clojure
(defn length [l]
  (if (empty? l)
      0
      (inc (length (rest l)))))
```

Any person who has attempted to read SICP will immediately know at
least one way to improve this code. A list of length `n` will require
`n` recursive calls plus the initial call, so therefore `n + 1` stack
frames will have to be created. This is all very wasteful. We can
rewrite this in a tail recursive manner so that only one stack frame
will be used by keeping track of the count.

```
(defn length [l]
  (letfn [(length* [count l]
            (if (empty? l)
              count
              (recur (inc count) (rest l))))]
    (length* 0 l)))
```

Okay, this looks significantly more complex. Let's rewrite this in
pseudo-Python to see what's happening.

```python
def length(l):
    def length_helper(count, l):
        if l.empty():
           return count
        else
           return length_helper(count + 1, l.rest())

    return length_helper(0, l)
```

This is an approximation of what the Clojure code does. We use `recur`
to take advantage of the Tail Call Optimization (TCO). It is possible
to run out of stack space in some situations if `recur` is not used.

We use the variable `count` to keep track of the state of the
computation between function calls. In other situations, to keep track
of more things, more arguments can be added.

A more familiar way of implementing `length` would be something like
this:

```python
def length(l):
    count = 0
    for elt in l:
        count++
    return count
```

What's happening here is we are taking a region of storage (i.e. a
variable) and repeatedly overwriting it value. Mutation is discouraged
in functional programming languages. Notice that in the first two
Clojure versions, we are not mutating anything. However, this _change_
of values of `count` can be simulated by the additional function
argument trick. In the above Python code, the variable count holds
various values at different points in time, but in the functional
version, there are multiple invocations of the function with various
argument values.