---
layout: post
title:  "Welcome to codesurfers!"
date:   2014-03-26 
categories:
---

Welcome to codesurfers.

We hope to publish possibly useful and/or interesting content here soon.

This is some sample code for the FizzBuzz problem.

```lisp

;;; Fizzbuzz in Common Lisp using the Optima pattern matching library

(ql:quickload 'optima)

(defpackage :fizzbuzz
  (:use :cl :optima)
  (:export :fizzbuzz))

(in-package :fizzbuzz)

(defun fizzbuzz ()
  (flet ((fizzbuzz-decider (number)
           (match (cons (rem number 3) (rem number 5))
             ((cons 0 0) "FizzBuzz")
             ((cons 0 _) "Fizz")
             ((cons _ 0) "Buzz")
             ((cons _ _) ""))))
    (loop
       for i from 1 to 100 do
         (format t "~a: ~a~%" i (fizzbuzz-decider i)))))
```
