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

It'll be more fun to follow along if you have a Clojure REPL to play
with.

Steps:

1. Install Leiningen

2. `lein new <project-name>`
3. `cd <project-name>`
4. `lein repl :headless :port 4005

5. `Install CIDER, Paredit and clojure-mode` using `M-x
   package-install` and configure them.

   You can then do `M-x cider-connect` and then enter "127.0.0.1" for   
   the host and the same port number as above.

6. `<project-name>/src/<project-name>/core.clj` is where you can save
   your code.

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

```clojure
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

What's happening here is we are taking a region of storage called
`count` (i.e. the variable `count`) and repeatedly overwriting its
value. Mutation is discouraged in functional languages. Notice that in
the first two Clojure versions, we are not mutating anything. However,
this _change_ of values of `count` can be simulated by the additional
function argument trick. In the above Python code, the variable count
holds various values at different points in time, but in the
functional version, there are multiple invocations of the function
with various argument values. The data is repeatedly being passed from
one function call to another, but never modified in place.

We have to think about data flowing through abstractions that
transform it (sometimes, gradually) into the solution. Contrast
this with the imperative version where we use the same storage area
and repeatedly keep overwriting it's value.

## FP Basics

### map

Whenever we have a collection of data and we want to transform it to
another collection of the same size, we use `map`. Here the word "map"
is used in the mathematical sense of "mapping" one value to
another. Say we have a collection of the first ten natural numbers. We
now map a squaring function over this collection to get a collection
of the squares of the first ten natural numbers. Every number in the
first collection has been mapped to it's corresponding square in the
second collection.

```clojure
explosure.core> (defn square [x]
                  (* x x))
#'explosure.core/square

explosure.core> (range 1 11)
(1 2 3 4 5 6 7 8 9 10)

explosure.core> (map square (range 1 11))
(1 4 9 16 25 36 49 64 81 100)
```

Normally we'd achieve this by looping over the collection, applying
the squaring function on each element and then appending this value to
a result collection.

```python
result = []

for elt in range(1, 11):
    result.append(square(elt))
```

Conceptually, we think of elt taking the value of each of the elements
from `1` to `10` in each iteration and then the operation being
performed on it. This is a common pattern - iterating over some
sequence of values.

It is simpler to think of all the iteration values as one collection
and it is this collection that is mapped to another one by a
function. For example, if you have a collection of word strings that
need to be capitalised, don't think of creating some iterative loop
where a capitalisation function is called on each of these words, and
the capitalised word appened to some accumulating result
collection. Rather, think that you have a collection of words with you
and you'll create a mapping from this collection of words to another
collection of capitalised words.

So, "perform X operation on every element in the collection" becomes
"transform collection by X" or alternatively, "map X over the
collection".

# lambdas

Lambdas are anonymous functions, and they are extensively used in
Clojure code. Quite simply, we use lambdas where we need to use a
function, and we can't be bothered creating a named function. You can
think of it as the body of the function definiton without the name.

```clojure
;; Two ways of creating a lambda that squares its input

explosure.core> (map (fn [x] (* x x)) (range 1 11))
(1 4 9 16 25 36 49 64 81 100)

explosure.core> (map #(* % %) (range 1 11))
(1 4 9 16 25 36 49 64 81 100)

explosure.core> (macroexpand-1 '#(* % %))
(fn* [p1__1264#] (* p1__1264# p1__1264#))

;; p1__1264# is created by gensym, so the above piece of code isn't
;; too dissimilar from
(fn [x] (* x x))
```

The `#` is a reader macro that transforms "(* % %)" into a valid
Clojure form similar to the explicit (fn [x] (* x x)) form that we
just saw. A simple way to remember its syntax is to take the body
of the lambda we need to write, in our case `(* x x)`, and in place
of the variables, use `%`. For functions with multiple arguments,
we can distinguish between them by adding a number like "%1" or
"%2" where the number designates the position of the argument.

```clojure
explosure.core> ((fn [a b] (+ a b)) 3 4)
7
explosure.core> (#(+ %1 %2) 3 4)
7
```

The syntactic shortcut for creating such anonymous functions can help
with readability when you don't want to draw too much attention to the
function, but sometimes it is also possible for it to hinder
readability when the function is too complex. Discernment and good
taste are essential to use it well.

One aspect of Clojure's lambdas that I like is that they can be named
as well. That's right! An anonymous function that has a name! Let's
see how this could be used.

Let's use the pedagogically most popular function to illustrate
recursion - the factorial!.

```clojure
(defn factorial [n]
  (if (= n 0)
    1
    (* n (factorial (dec n)))))
```

```clojure
(fn [n]
  (if (= n 0)
    1
    (* n (??? (dec n)))))
;;         ^-- we don't have a name to recursively call
```

The very essence of recursion is the self-referential
definition. Let's say we wanted to define factorial only using
lambdas. If we don't have a name to recursively call, how do we
recurse?

There is an easy, convenient answer and it is by using named lambdas.

```clojure
explosure.core> (fn factorial [n]
                  (if (= n 0)
                    1
                    (* n (factorial (dec n)))))
#<core$eval1370$factorial__1371 explosure.core$eval1370$factorial__1371@7fe85830>

explosure.core> ((fn factorial [n]
                   (if (= n 0)
                     1
                     (* n (factorial (dec n))))) 10)
3628800
```

