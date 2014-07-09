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

As soon as I see the word frequency, I start thinking whether using a
hash-table is a viable option.

We want to create a hash-table of the sort : `{ line : freq_of_occurence , ... }`
which contains all the strings of the log file mapped to their frequency.

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
its `iter` function.

```ocaml
utop[24]> List.iter ;;
- : 'a list -> f:('a -> unit) -> unit = <fun>
(*   ^-- (1)     ^---- (2)        ^--- (3) *)
```
1. a list containing elements of type `'a`
2. a function that takes an argument of type `'a`
3. `unit` is represented `()` in code and can be thought of as something
   like `void`.
4. `->` means "returns".
5. `f:` is just the label of the second argument.

We shall use labeled arguments soon. Using labeled arguments we can
change the order in which arguments are passed to the function.

The above type signature means that `List.iter` is a function that
takes a list containing elements of type `'a` and a function that takes
an argument of type `'a` returning `unit`, and finally `List.iter`
returns `unit`.

Let us 'incrementally' figure out how to iterate over a list.

```ocaml
utop[30]> List.iter ;;
- : 'a list -> f:('a -> unit) -> unit = <fun>
```

By now, it should be clear what that type signature means. Let's see
what happens when we give it a list as an argument. Normally,
supplying a function that takes n arguments with a different number of
arguments is an error in many languages. However, let's try it anyway.

```ocaml
utop[29]> List.iter [1;2;3;4;5] ;;
- : f:(int -> unit) -> unit = <fun>
(*   ^----(1)           ^-- (2) *)
```
1. a function that from `int` to `unit`.
2. The final return value.

Let's write a simple function that is from `int` to `unit`.

```ocaml
utop[34]> let print_integer x = printf "%d\n" x;;
val print_integer : int -> unit = <fun>

utop[36]> print_integer 1337;;
1337
- : unit = ()
```

Now, we'll use this function to print out all the elements of the
list.

```ocaml
utop[38]> List.iter [1;2;3;4;5] ~f:print_integer ;;
1
2
3
4
5
- : unit = ()
```

It turns out that for one-off use, it's better to use an
[anonymous function](https://en.wikipedia.org/wiki/Anonymous_function).
Anonymous functions (or lambdas) are defined using `fun`.

```ocaml
utop[40]> (fun x -> x * x);;
- : int -> int = <fun>
(* type signature of a function from int to int *)
utop[41]> (fun x -> x * x) 16;;
- : int = 256
(* anonymous function called with 16 as an argument *)
```

Therefore, this:

```ocaml
utop[39]> List.iter [1;2;3;4;5] ~f:(fun x -> printf "%d\n" x) ;;
```
should work just as nicely.

We know know how to generate a list containing the lines of the
log-file, and how to iterate over a list. All we need to do is to
figure out what to do with each line of the list. That's what should
be in the body of the function argument to `List.iter`.

In the below skeleton, we are creating a `Hashtbl`, filling it up and
then returning it.

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

Let's discuss how hash-tables work in a language like Python.

```python
In [1]: table = {1 : "one", 2 : "two", 3 : "three" }

In [2]: table[1]
Out[2]: 'one'

In [5]: table.get(1)
Out[5]: 'one'

In [4]: table.get(4)
# this returns None, because 4 isn't a key in the hash-table
```

So, we can say that for a hash-table `{KeyType : ValueType}`,
`get(KeyType)` returns the `KeyType`'s associated `ValueType` _or_
`None` if it doesn't exist.

You just can't have a function in OCaml that returns type A in some
cases, and another type B in other cases. How OCaml gets around this
is by using [Option types](https://en.wikipedia.org/wiki/Option_type).
There is a enlightening discussion of user defined types
[here](http://www2.lib.uchicago.edu/keith/ocaml-class/userdefined.html).

The `option` type in OCaml is predefined like this:.

```ocaml
type 'a option = Some of 'a | None
```
`option` is a polymorphic type, which means that `'a` could be any
type, just like how the list type can hold elements of type `'a`. This
pretty much means a list can hold any type of element. We can think of
the type `int option` as: "it is _either_ an `int` or `None`". 

We concern ourself with `option` because we have to use `Hashtbl.find`
to get the frequencies from the hash-table.

```ocaml
utop[42]> Hashtbl.find ;;
- : ('a, 'b) Hashtbl.t -> 'a -> 'b option = <fun>

(* - : ('KeyType, ValueType) Hashtbl.t -> 'KeyType -> 'ValueType option = fun *)
```

`Hashtbl.find` takes a `Hashtbl.t` and a `'KeyType` and returns a
`'ValueType option`.

[This](https://blogs.janestreet.com/making-something-out-of-nothing-or-why-none-is-better-than-nan-and-null/)
should make the following code trivial to understand.

```ocaml
let current_count = 
   match Hashtbl.find frequency_table line with
     | None -> 0
     | Some freq -> freq
   in Hashtbl.replace frequency_table ~key:line ~data:(current_count + 1)
```

Whew! After a _lot_ of digressions, we finally arrive at the full
function. It is easy to understand what it is doing.

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

1. We can load all the `(Line, Freq)` tuples into an array, sort it and
then take the first `k` elements. Sorting is `O(n log n)`, but it does
more work than necessary, as we only need `k` most-frequent elements.

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
readjusting; a `O(log n)` operation.

While discussing this problem, [gsathya](http://gsathya.in/) whipped
up a quick implementation in Python, the distinctive feature of which
is that he uses a _min-heap_ that only holds `k` elements. We can
restrict the size of the min-heap to `k` elements, and then only
insert elements into it if the current element is greater than the
minimum. Eventually this min-heap of size `k` will hold the `k`
largest elements, i.e. at the end of the operation it will hold the
`kth` largest element as the top most element and all the other
elements greater it.

Let's see how we generate this `k` sized min-heap. The `iter`
functions defined for the various collection types, iterate over _all_
the element. This is not what we want, at least for the first part of
generate the heap. We need to find a way to take `k` elements from
the frequency_table and then insert them into the min-heap.

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

3. Converts the list to a [Sequence](https://ocaml.janestreet.com/ocaml-core/111.17.00/doc/core/#Std.Sequence).

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

```ocaml
(Sequence.iter
  (Sequence.take
    (Sequence.of_list
      (Hashtbl.keys frequency_table))
    k)
  ~f:(fun line -> (* ... *)))
```

The above code looks Lispier, but I prefer the version with pipes as
that is more idiomatic OCaml. For the code above we have to understand
what's happening from the inside out, i.e. from the innermost function
call. Using the pipe operator we can comprehend the code more
naturally from the outside in.

We now have iterate over the remaining lines in the hash-table and if
we find a line that has a frequency greater than frequency of the top
element in the min-heap, we remove the top element, and add a new tuple.

We now have a min-heap with the k-most frequent lines. However, we
want to print the lines in descending order, so we create a new
_max-heap_ from which we can remove the elements one by one. Notice
now we use `tuple_less_than` instead of `tuple_greater_than`.

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

That's about it, really. Have a look at the complete implementation of the problem
[here](https://github.com/codesurfers/article-code/blob/master/k-most-frequent-lines/ocaml/log_lines.ml).
There's an earlier
[C++ version](https://github.com/codesurfers/article-code/blob/master/k-most-frequent-lines/c%2B%2B/log-lines.cc)
as well.