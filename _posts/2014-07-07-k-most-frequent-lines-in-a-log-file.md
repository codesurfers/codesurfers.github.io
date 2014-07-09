---
layout: post
author: Samuel Chase <samebchase@gmail.com>
title:  "k-most frequent lines in a log file"
date:   2014-07-07 
categories:
---

[gsathya](http://gsathya.in/) suggested that I try to do this problem
on IRC, when I was looking for small problems to solve to get some
programming practice.

I immediately started wondering about a good way to solve
this. Something I recently read about UNIX tools came to mind. I
conjectured that I could compose a solution using some arcane
permutation of `uniq`, `sort` and `head` to achieve this. It turned
out that I was right: [this](http://superuser.com/a/383730) excellent
answer has all the details of the UNIX approach of solving this
problem.

I thought this may be an educational problem to solve by writing a
(non-shell) program to do the same thing.

As soon as I see the word "frequency", I start thinking whether using a
hash-table is a viable option.

A hash-table like `{ line : freq_of_occurence , ... }`
can be used to contain all the strings of the log file mapped to their
frequency.

```python
for line in file:
    if line exists in hash_table:
        increment its frequency
    else
        set the its frequency to 1

finally return the hash_table
```

We need a way of getting the lines of the log file. The function
`In_channel.read_lines` takes a filepath as an argument and returns a
list of strings.

The idiomatic way of iterating over collections in OCaml is by using
its `iter` function. We can use this to iterate over the list of
strings returned by `In_channel.read_lines`.

Let's inspect the type of `List.iter` in the REPL.

```ocaml
utop[24]> List.iter ;;
- : 'a list -> f:('a -> unit) -> unit = <fun>
(*   ^-- (1)     ^---- (2)        ^--- (3) *)
```
1. a list containing elements of type `'a`. In other words, a list
   containing elements of any type.

2. a function that takes an argument of type `'a`
3. `unit` is represented as `()` in code and is like `void`.
4. `->` means "returns".
5. `f:` is just the label of the second argument.

The above type signature means that `List.iter` is a function that
takes two arguments: arg 1 is a list containing elements of type `'a`
and arg 2 is a function that takes an argument of type `'a` returning
`unit`. The `unit` on the extreme right is the return type of
`List.iter`.

Let us iteratively figure out how to iterate over a list.

```ocaml
utop[30]> List.iter ;;
- : 'a list -> f:('a -> unit) -> unit = <fun>
```

We know what that type signature means. Let's see
what happens when we give it a list as an argument. This function
normally expects a `List` and a function as an argument, but we're
only giving it one.

Supplying a function that takes n arguments with a different number of
arguments is usually an error in other languages. Let's see how OCaml
behaves:

```ocaml
utop[29]> List.iter [1;2;3;4;5] ;;
- : f:(int -> unit) -> unit = <fun>
(*   ^----(1)           ^-- (2) *)
```
1. a function from `int` to `unit`.
2. The final return value.

[Currying](https://en.wikipedia.org/wiki/Currying) is what is
happening here.

Let's write a simple function that from `int` to `unit` that we could
possibly use as the second argument.

```ocaml
utop[34]> let print_integer x = printf "%d\n" x;;
val print_integer : int -> unit = <fun>

utop[36]> print_integer 1337;;
1337
- : unit = ()
```

We can use this function to print out all the elements of the list. We
use `~f:` as the name of the label.

```ocaml
utop[38]> List.iter [1;2;3;4;5] ~f:print_integer ;;
1
2
3
4
5
- : unit = ()
```

It turns out that for one-off use, it's more convenient to use an
[anonymous function](https://en.wikipedia.org/wiki/Anonymous_function).
Anonymous functions (or lambdas) are defined using `fun`.

```ocaml
utop[40]> (fun x -> x * x);;
- : int -> int = <fun>
(* type signature of a function from int to int *)
utop[41]> (fun x -> x * x) 16;;
- : int = 256
(* anonymous function that squares its argument is passed 16 *)
```

Therefore, this:

```ocaml
utop[39]> List.iter [1;2;3;4;5] ~f:(fun x -> printf "%d\n" x) ;;
```
should work just as nicely.

We know know how to generate a list containing the lines of the
log-file, and how to iterate over a list. We need to figure out what
to do with each line of the list. That's what should be in the body of
the function argument to `List.iter`.

In the skeleton below, we create a `Hashtbl`, fill it up and
then return it.

```ocaml
let generate_frequency_table file_path =
  let frequency_table = Hashtbl.Poly.create () in
  List.iter 
    ~f:(fun line ->
            (* code to handle each line *)
       )
    (In_channel.read_lines file_path);
  frequency_table (* this hash-table is returned by the function *)
;;
```

Let's digress and discuss how hash-tables work in a language like
Python.

```python
In [1]: table = {1 : "one", 2 : "two", 3 : "three" }

In [2]: table[1]
Out[2]: 'one'

In [5]: table.get(1)
Out[5]: 'one'

In [4]: table.get(4)
# this returns None, because 4 isn't a key in the hash-table
```

So, for a hash-table `{KeyType : ValueType}`, `get(KeyType)` returns
the `KeyType`'s associated `ValueType` _or_ `None` if it doesn't
exist.

You just can't have a function in OCaml that returns type A in some
cases, and another type B in other cases. How OCaml gets around this
is by using [Option types](https://en.wikipedia.org/wiki/Option_type).
For an enlightening discussion of user defined types please read
[this](http://www2.lib.uchicago.edu/keith/ocaml-class/userdefined.html).

The `option` type in OCaml is predefined like this:.

```ocaml
type 'a option = Some of 'a | None
```

`option` is a polymorphic type, which means that `'a` could be any
type, just like how the list type can hold elements of type `'a`. This
means that a list can contain elements of any type, as long as they
are all of the same type.

We can think of the type `int option` as: "it is _either_ an `int` or
`None`".

We concern ourself with understanding `option` types because a function we use,
`Hashtbl.find` returns an `option` type, as we can see below.

```ocaml
utop[42]> Hashtbl.find ;;
- : ('a, 'b) Hashtbl.t -> 'a -> 'b option = <fun>

(* - : ('KeyType, ValueType) Hashtbl.t -> 'KeyType -> 'ValueType option = fun *)
```

`Hashtbl.find` takes a `Hashtbl.t` and a `'KeyType` and returns a
`'ValueType option`.

```ocaml
utop[75]> let table = Hashtbl.Poly.create () ;;
val table : ('_a, '_b) Hashtbl.t = <abstr>

utop[82]> Hashtbl.add table ~key:"two" ~data:2 ;;
- : [ `Duplicate | `Ok ] = `Ok

utop[83]> Hashtbl.add table ~key:"one" ~data:1 ;;
- : [ `Duplicate | `Ok ] = `Ok

utop[84]> Hashtbl.find table "two" ;;
- : int option = Some 2
```

Now that we understand the basics of `Hashtbl`, please read
[this](https://blogs.janestreet.com/making-something-out-of-nothing-or-why-none-is-better-than-nan-and-null/) 
so that the following code trivial to understand.

```ocaml
let current_count = 
   match Hashtbl.find frequency_table line with
     | None -> 0
     | Some freq -> freq
   in Hashtbl.replace frequency_table ~key:line ~data:(current_count + 1)
```

Whew! After a _lot_ of digressions, we finally arrive at the full
function. I hope that it's decomposition was easy to understand.

```ocaml
let generate_frequency_table file_path =
  let frequency_table = Hashtbl.Poly.create () in
  List.iter 
    ~f:(fun line ->
      let current_count = 
        match Hashtbl.find frequency_table line with
          | None -> 0
          | Some freq -> freq
      in Hashtbl.replace frequency_table ~key:line ~data:(current_count + 1))
    (In_channel.read_lines file_path);
  frequency_table
;;
```

Okay. Now we've got a hash-table containing the lines and the
frequencies. We need to find a way of getting the most frequent lines
from this map. Turns out that maps aren't that great for k-th maximum
calculations.

We have two choices before us:

1. We can load all the `(Line, Freq)` tuples into an array, sort it in
   descending order and then take the first `k` elements. Sorting is
   `O(n log n)`, but it does more work than necessary, as we only need
   `k` most-frequent elements.

2. We can use heaps.

Heaps are better with the assumption that `k` is much smaller than the
number of lines `n` in the log file. This is because the maximum (or
minimum, if we are using a `min-heap`) element is always guaranteed to
be at the top of the queue. Once this maximum is removed, the priority
queue is _readjusted_ so that the next maximum element becomes the top
most element. Readjusting a heap `k` times might be better than having
to sort a possibly very large vector of `(Line, Freq)` tuples.

Normally tuple comparison happens lexicographically, i.e. like a
dictionary.

Lexicographically speaking: `"be" > "abracadabra"`, because we start
comparing by characters in corresponding places. We compare the
corresponding characters in the next position, only if there is a tie.

We need a function that compares the second (integral) element of two
2-tuples.

```ocaml
let tuple_greater_than a b =
  let a_snd = Tuple.T2.get2 a in
  let b_snd = Tuple.T2.get2 b in
  a_snd - b_snd
;;
```

You might be wondering why we don't do `a_snd > b_snd` instead of
finding the difference and returning a numeric value. This is because
the function passed as an argument to order a heap has this type:

```ocaml
utop[62]> Hash_heap.Heap.create ;;
- : ?min_size:int -> cmp:('a -> 'a -> int) -> unit -> 'a Heap.Removable.t = <fun>
(*                      ^--- This is the comparison function *)
```
The function labeled with `cmp` takes two elements of a (polymorphic)
heap and returns an integer. As we are storing 2-tuples in the heap,
`tuple_greater_than` is defined so that it can operate on that type.

A naïve implementation that I first came up with loaded all the
`(Line, Freq)` tuples into a max heap, and then removed the top `k`
elements. This approach is bound to spend up a lot of time in heap
readjusting; a `O(log n)` operation, where `n` is the number of unique
lines.

While discussing this problem, [gsathya](http://gsathya.in/) whipped
up a quick implementation in Python, the distinctive feature of which
is that he uses a _min-heap_ that only holds `k` elements. We can
restrict the size of the min-heap to `k` elements, and then _only_
insert elements into it if the current element is greater than the
minimum.

This min-heap of size `k` will eventually hold the `k` largest
elements, i.e. at the end of the operation it will hold the `kth`
largest element as the top most element and all the other elements
greater it.

Let's see how we generate this `k` sized min-heap. The `iter`
functions defined for the various collection types, iterate over _all_
the elements. We need to find a way to take `k` elements from the
frequency_table and then insert them into the min-heap. Let's discuss
the code we use to achieve this:

```ocaml
  let k_heap = Hash_heap.Heap.create ~cmp:tuple_greater_than () in (* 1 *)
  Hashtbl.keys frequency_table                                     (* 2 *)
  |> Sequence.of_list                                              (* 3 *)
  |> (fun seq -> Sequence.take seq k)                              (* 4 *)
  |> Sequence.iter                                                 (* 5 *)
      ~f:(fun line ->
        let freq = Hashtbl.find_exn frequency_table line in
        Hash_heap.Heap.add k_heap (line, freq);                    (* 6 *)
        Hashtbl.remove frequency_table line);
```

1. Creates a `Heap`.

2. Generates a list of keys

3. Converts the list to a
   [Sequence](https://ocaml.janestreet.com/ocaml-core/111.17.00/doc/core/#Std.Sequence). Using
   sequences should hopefully be better than passing massive lists
   from one function to another.

4. Ideally, we'd be able to write `|> Sequence.take k`, but
   because that function hasn't been defined with labeled arguments,
   we have to use a trivial lambda to explicitly specify the order.

5. We iter over every line in this `k` sized sequence.

6. We know that `Hashtbl.find` returns an `option`, but as we are sure
   that the associated key exists, we use `Hashtbl.find_exn` to find
   and "unwrap" the `option` value returned. `Hashtbl.find_exn` raises
   an exception if the key is not found, but we don't have to worry
   about that here. We add the `(Line, Freq)` tuple to the heap, and
   finally remove the line from the hash-table.

Instead of using pipes, we could rewrite the above chunk of code like
this:

```ocaml
(Sequence.iter
  (Sequence.take
    (Sequence.of_list
      (Hashtbl.keys frequency_table))
    k)
  ~f:(fun line -> (* ... *)))
```

This looks Lispier, but I prefer the version with pipes as that is
more idiomatic OCaml. In the code shown above we have to understand
what's happening from the inside out, i.e. from the innermost function
call. Using the pipe operator we can comprehend the code more
naturally from the outside in, actually following the sequence of the
flow of data.

We have to iterate over the remaining lines in the hash-table and if
we find a line that has a frequency greater than frequency of the top
element in the min-heap, we remove the top element, and add a new tuple.

We now have a min-heap with the k-most frequent lines. However, we
want to print the lines in descending order, so we create a new
_max-heap_ from which we can remove the elements one by one. Notice
that we use `tuple_less_than` instead of `tuple_greater_than`.

```ocaml
let reversed_heap = Hash_heap.Heap.create ~cmp:tuple_less_than ()
in Hash_heap.Heap.iter k_heap
~f:(fun tuple -> Hash_heap.Heap.add reversed_heap tuple);
```

The full code for `generate_heap` is given below:

```ocaml
let generate_heap frequency_table k =
  let k_heap = Hash_heap.Heap.create ~cmp:tuple_greater_than () in

  Hashtbl.keys frequency_table
  |> (fun seq -> Sequence.take seq k)
  |> Sequence.iter
      ~f:(fun line ->
        let freq = Hashtbl.find_exn frequency_table line in
        Hash_heap.Heap.add k_heap (line, freq);
        Hashtbl.remove frequency_table line);

  Hashtbl.iter frequency_table
    ~f:(fun ~key:line ~data:freq ->
      if freq > Tuple2.get2 (Option.value_exn (Hash_heap.Heap.top k_heap))
      then Hash_heap.Heap.add k_heap (line, freq)
      else ());

  let reversed_heap = Hash_heap.Heap.create ~cmp:tuple_less_than ()
  in Hash_heap.Heap.iter k_heap
  ~f:(fun tuple -> Hash_heap.Heap.add reversed_heap tuple);

  reversed_heap
;;
```

The last bit of code to discuss, is the code that deals with removing
the top element from the max heap.

```ocaml
for _i = 1 to num_pops do
  let top = Option.value_exn (Hash_heap.Heap.pop heap) in
  printf "%d : %s\n" (Tuple2.get2 top) (Tuple2.get1 top);
done
```

We remove the top element `num_pops` times and because we are sure
that the element exists we can `Option.value_exn` to unwrap the
return value of `Hash_heap.Heap.pop`.

That's about it, really. You should be able to understand the the
complete implementation of the problem
[here](https://github.com/codesurfers/article-code/blob/master/k-most-frequent-lines/ocaml/log_lines.ml).
There's also an earlier C++
[version](https://github.com/codesurfers/article-code/blob/master/k-most-frequent-lines/c%2B%2B/log-lines.cc)
which makes heavy use of the standard libary.

Thanks for your patience in reading this rather long walkthrough. I
hope you were able to gain something from it.