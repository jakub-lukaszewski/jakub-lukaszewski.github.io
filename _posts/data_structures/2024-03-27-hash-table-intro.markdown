---
layout: post
title: "Hash Table"
subtitle: "Loose Considerations"
author: "Kuba"
header-style: text
mathjax: true
tags:
  - Hash table
  - Python
  - Data structures
---

Hash table is a data structure consisting of keys and values arranged in pairs. Moreover, it defines operations on those elements, like:

1.  delete a key-value pair,
2.  add a key-value pair,
3.  modify a value given a key,
4.  lookup a value given a key.

An examples resembling this abstract data type is a phone book where keys are names and values are numbers and a billingual dictionary with keys as foreign words and values as their definitions in other language.

> Dictionaries are also called maps or associative arrays because we map keys to values or to put it another way, we associate values with keys.

We can think of a dictionary as a math function assigning a value to possibly more than one argument. Since we might interpret it this way, you probably guess there are certain properties about it:

- **only key-value pairs:** argument of a function determines a value and inversely, a value is assigned an argument <br><center> $ \forall y \in Y_f \text{ } \exists x \in X_f : f(x) = y $,

- **unique keys:** there don't exist two distinct values associated with one argument <br><center> $ \nexists _{y_0 \neq y_1} [f(x)=y_0 \land f(x)=y_1] $, where $ y_i\in Y_f,x\in X_f $,
- **non-unique values:** there might be an argument for which there is more than one value,
- **arbitrary pairs:** keys and values might be of various types like pictures, strings or numbers, though they have to be uniform within a given set (*),
- **unordered pairs:** no order by default, it is implementation-specific.

> (*) Most programming languages don't allow hash tables to have keys of different data type. However, there are exceptions and in some languages it is possible if a data type is hashable. One of such languages is Python. Hashable data types in Python are immutable types like strings, numbers and tuples.

Now, how do we implement such a data type? We don't! (unless we have a reason... we will cover it later) Modern languages come with dictionaries in the form of primitives or classes in their standard libraries.

In Python, `dict` type is the implementation and the underlying inner-workings being wrapped by it are written in C.

*Code*
```python
spanish_english_dict = {"casa": "house", "manzana": "apple", "platano": "banana"}
print(f"Dict type: {type(spanish_english_dict)}")
```
*Output*
```
`Dict` type: <class 'dict'>
```

How does mapping actually work? Why is it so fast (is it?)?

### Hash Table as an Array with a Hash Function

Have you ever wondered why a sequence look up is so blazingly fast in Python? Let's take a look:

```python
import string
from time import perf_counter

example_sequence = "starcraft" * 100_000_000 + string.ascii_lowercase * 50_000_000

start_time = perf_counter()
print(example_sequence[:100])
end_time = perf_counter()

lookup_time = end_time - start_time
print(f"Lookup took {lookup_time*1_000:.2f} ms")
```

This sentence is 2.2B chars long and I was able to get a desired first 100 chars of a sentence in approx. 0.06 ms! Better still, an almost instantaneous lookup is true for all sequence types in Python: tuples, string, lists. It's because of an array implementation , sequences are backed by that and it has the following properties:
1.  an array occupies a contiguous blocks of memory,
2.  fixed size is required for every array element, known upfront.

To get a specific array element, a machine needs its memory address, and it can be calculated given an element's index, element's size and an array memory address (also known as offset): <br><center> $ element\_address = offset + element\_size * element\_idx $.

But hey! Lists can store heterogeneous elements. They might be of a different size, which breaks the above formula! To avoid that, Python introduces another indirection where it actually doesn't store elements directly in an array but it stores their addresses under which the according element is located at (Fig. 1).

![img](/img/in-post/hash-table/post-hash-table-memory-addresses.jpg)
***Fig. 1.** An array with memory addresses pointing to respective element locations.*

The same idea is used in hash tables. They are named this way because of a hashing mechanism that lets them translate keys into integer values acting as array indices and without a significant performance loss it allows for element retrieval by using an arbitrary key. It's important to note that not every key can be hashed like e.g. mutable lists, dictionaries and sets.

There's going to be a part about hash functions in detail in a near future, so stay tuned!
