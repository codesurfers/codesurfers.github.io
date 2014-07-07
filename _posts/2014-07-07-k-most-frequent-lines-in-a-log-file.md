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
hash-table (or a
[map](http://en.cppreference.com/w/cpp/container/map)) is a viable
option.

To get the simple things out of the way first, let's read a file,
generate the frequency map for each line.
Something like : `{ line : occurring_freq , ... }`.

For a `std::map<Line, Frequency>`, the default behaviour initialises
`Value` to `0` when it is of an integral type. This is convenient
because we can just increment the `Frequency` count when we see a
line.

The function `file_line_freq` returns a `FreqMap` which returns a map
containing all the lines and their associated frequencies.

We just keep 'getting' a new line from the input stream until we can't
anymore, and incrementing the frequency count. There will be no
duplicates because we are using maps.

```c++ 
typedef std::map<string, int> FreqMap;

// I have removed some error checking, because pedagogic code can be
// as unrealistic as you want it to be.

FreqMap file_line_freq(string filename) {
    FreqMap freq_map;
    std::ifstream instream(filename);

    string line;

    while (getline(instream, line)) {
        freq_map[line]++;
    }
    
    return freq_map;
}
```

Okay. Now we've got a frequency map of all the lines of the log
file. We need to find a way of getting the most frequent lines from
this map. Turns out that maps aren't that great for k-th maximum
calculations.

We have two choices before us:

1. We can load all the `Line : Freq` pairs into a
`vector<pair<string, int>>` and then loop over the first `k` elements of
this vector after sorting it. Fairly straightforward.

2. We can use priority queues.

Priority queues are better with the assumption that `k` is much
smaller than the number of lines in the log file. This is because the
maximum (or minimum, depending on the kind of priority queue) element
is always guaranteed to be at the top of the queue. Once this maximum
is removed, the priority queue is _readjusted_ so that the next
maximum element becomes the top most element. I believe that readjusting a
priority queue `k` times might be better than having to sort a possibly
very large vector of `Line : Freq` pairs.

Normally pair comparison happens lexicographically, i.e. like a
dictionary. We need to specify our own comparison function that
compares based on frequency.

[std::pairs](http://en.cppreference.com/w/cpp/utility/pair) have
`first` and `second` member objects that can be accessed, so the
following code is trivial.

```c++

bool pair_less_than(pair<string, int> a, pair<string, int> b) {
    return a.second < b.second;
}
```

We can now move on to core function of this whole operation:
`print_most_frequent_lines`

Let's take a look at the declaration of the priority queue we are
going to use:

```c++
std::priority_queue<pair<string, int>,                       // A
                    std::vector<pair<string, int>>,          // B
                    std::function<decltype(pair_less_than)>> // C
    freq_queue(pair_less_than);                              // D
```

* `A:` is the type of the elements of the priority queue. We are using
  `pair<string, int>`.

* `B:` is the underlying container.

* `C:` is the declaration of the type of the comparison function we
  use. We use
  [decltype](http://en.cppreference.com/w/cpp/language/decltype) to
  give us the declaration of the function `pair_less_than`.

* `D:` is a priority queue called `freq_queue` is created with its
  internal comparison function
  [instantiated](http://stackoverflow.com/questions/20826078/priority-queue-comparison)
  with `pair_less_than`.

We iterate over all the pairs in the frequency map, and load them into
a priority queue.

```c++
for (FreqMap::iterator iter = freq_map.begin();
     iter != freq_map.end(); ++iter) {
    freq_queue.push(*iter);
}
```  

We pop `n` elements off the priority queue if they are available like this:

```c++
pair<string, int> elt;

for (int i = 0; i < n && !freq_queue.empty(); ++i, freq_queue.pop()) {
    elt = freq_queue.top();
    cout << elt.second << " : " << elt.first << endl;
}
```

The full code of the `print_most_frequent_lines` is given below:
  
```c++
void print_most_frequent_lines(FreqMap freq_map, int n) {

    std::priority_queue<pair<string, int>,
                        std::vector<pair<string, int>>,
                        std::function<decltype(pair_less_than)>>
        freq_queue(pair_less_than);

    for (FreqMap::iterator iter = freq_map.begin();
         iter != freq_map.end(); ++iter) {
        freq_queue.push(*iter);
    }

    pair<string, int> elt;

    for (int i = 0; i < n && !freq_queue.empty(); ++i, freq_queue.pop()) {
        elt = freq_queue.top();
        cout << elt.second << " : " << elt.first << endl;
    }
}
```

That's it! The complete implementation of the problem is available
[here](https://github.com/codesurfers/article-code/blob/master/k-most-frequent-lines/log-lines.cc).

