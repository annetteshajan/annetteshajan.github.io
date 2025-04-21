---
title: "Some cool Pythonic Code Constructs"
description: "Useful python techniques to convey more in less lines of code"
publishDate: "15 April 2025"
tags: ["coding","tipsntricks"]
draft: true
---

## Introduction
This post is meant to be something like a means of writing less lines of code in Python. It isn't an introduction to Python and how it works. For that the best source would obviously be the official documentation here: https://docs.python.org/3/tutorial/index.html

I have never really used python in any of my jobs or internship but honestly it is the best language for interview preparation - especially for Leetcode style problems. Once you start solving Leetcode in Python, you will quickly realize how succinct the language is as compared to Java or C++ where you may have to deal with the pain of initializing variables, the various curly brackets and parentheses and the hundred other syntactic requirements. 

Some things I want to cover in this post would be :
1. Comprehensions
2. Packing and unpacking 
3. Unique data structures in Python

## Comprehensions
Python supports comprehensions for lists, dictionaries, sets and generators.
A comprehension is essentially just a shorter way to fill in a data structure.
For example- if you want to fill a list with the squares of numbers 1 to 5, you might initially consider doing it like this.
```python
squares = []
for i in range(6):
    squares.append(i*i)
```

With comprehensions we can do it just with this single line:
```python
squares = [i*i for i in range(6)]
```

You can even add conditions - if you want to only add squares of even numbers:
```python
squares = [i*i for i in range(6) if i%2==0]
```

Using a nested loop we can add different variables in a 2D list.
```python
sample_list = [[x,y] for x in range(5) for y in range(5)]
```
We can also flatten a matrix into a 1D list like:
```python
matrix=[[1,2,3],[4,5,6],[7,8,9]]
single_list= [num for row in matrix for num in row]
```
Dictionaries and Sets work similarly to lists

```python
sample_dictionary={num : num+1 for num in range(5)}
```
```python
sample_set = {num*2 for num in range(5)}
```

A generator as you may know already creates an iterator which will dynamically generate values during runtime. The generator values itself need to be assigned to a data structure to access it.

```python
res = (num*2 for num in range(5))
twice_tuple=tuple(res)
```

> NOTE: Tuples do not support comprehension due to the fact that comprehension itself allows for modifying values which is not allowed by tuples since it is an immutable data structure. However you can typecast a list or a set into a tuple which is created using comprehension.

## Packing and Unpacking Variables
This is a pretty simple topic but useful if you want to write less lines of code. It is essentially a way to group and ungroup variables into a single variable or split it across multiple variables.

Packing looks like this 
```python
num1,num2,num3=1,2,3
myTuple = (num1,num2,num3)
```
You can also use the * operator for grouping parameters. Something like this:
```python
*nums=num1,num2,num3
a, *b = [1,2,3,4] # a=1, b= [2,3,4]
```

The ** operator will take a dictionary of key value pairs.
```python
dict1 = {'a': 1}
dict2 = {'b': 2}
merged = {**dict1, **dict2}
print(merged)  # {'a': 1, 'b': 2}
```

Unpacking is pretty much the opposite. We split a grouped variable into its elements.
```python
pairs=[[1,'a'],[2,'b']]
for num,val in pairs:
    print(num,val)
```

Packing and unpacking comes especially useful in built in functions like `enumerate` and `zip`.
`enumerate` is a function which returns a sequence of iterable tuples with the index and the value of the element like so:
```python
fruits = ['apple', 'banana', 'cherry']
for index, fruit in enumerate(fruits):
    print(index, fruit) # It will print the index and fruit one per line

```

`zip` is a function which combines iterables into tuples. We can again unpack the variables of zip.
```python
names = ['Ann', 'Ben', 'Cara']
scores = [90, 85, 95]

for name, score in zip(names, scores):
    print(name, score)  # It will print the name and score one per line

```


## Unique Data Structures
### Deque
It is a double ended queue in Python. We can push and pop at either end of the list with O(1) time complexity.
It is a very efficient data structure compared to regular stacks and queues.

To add to a deque:
```python
d.append(10)        # Add to right
d.appendleft(5)     # Add to left

```

To remove from a deque:
```python
d.pop()             # Remove from right
d.popleft()         # Remove from left
```

To rotate a deque:
```python
d.rotate(1)   # Right shift by 1
d.rotate(-1)  # Left shift by 1
```

It is most useful in BFS algorithms as well as sliding window problems. 

### Minheap
Minheaps follow the heap property i.e. the parent is always smaller than its children nodes. Python only supports minheap with the `heapq` module. If you want to use a maxheap we need to insert negative values in order to use the minheap as a maxheap.

So in a minheap we have the smallest element as root. 
- It uses O(1) time to pop elements. 
- Pushing takes O(logn) time as it needs to be inserted satisfying the heap property.

To use a heap, we just convert a list to heap using heapify method. It converts the list into a heap such that the first element will always be the minimum value.
```python
l= [10,3,2,4,7]
heapq.heapify(l) #rearranges l in place to become a heap
print(l[0]) #prints the minimum value

heapq.heappush(l,5) # adds an element to the heap
item= heapq.heappop(l) # removes the minimum element
```

We can also use heap methods to find the nsmallest or nlargest numbers in the heap.
```python
import heapq

nums = [5, 2, 1, 9, 7]
print(heapq.nsmallest(2, nums))  # [1, 2]
print(heapq.nlargest(2, nums))  # [9, 7]

```

Some use cases include: priority queues, djikstra's algorithm, merging sorted lists.


### Set

A set or hashset is a collection of unique elements which are unordered and mutable.
It is one of the fastest data structures with adding, removing and membership checks in O(1) time.

|Method | Description|
|--------|--------------------|
s.add(x) | Add x to the set
s.remove(x) | Remove x; raises error if not present
s.discard(x) | Remove x; no error if not present
s.pop() | Removes and returns an arbitrary element
s.clear() | Remove all elements
len(s) | Number of elements
x in s | Membership test
x not in s | Negation of membership test

We can also do comparisons between sets such as
```python
a.issubset(b)       # a ⊆ b
a.issuperset(b)     # a ⊇ b
a.isdisjoint(b)     # a ∩ b == ∅

```

Sets compulsorily only contain hashable types of data and hence will not store lists or dictionaries.
Sets are also unordered so we cannot access them by index.

It is ideal when we want to do fast membership checks and the order is not important.

### Dictionary
This is a hashmap in other languages. It has pairs of keys and values. It is unordered, mutable and all the keys need to be unique. 
The keys cannot be altered and hence are by itself immutable. So we cannot have a list or a dictionary as the key. But we can have tuples, strings and integers.

They also provide fast lookup times of O(1).

Common operations with dictionaries are as follows:
|Operation | Code | Notes|
|----------|--------|--------|
Access value | d["key"] | Raises KeyError if not present
Safe access | d.get("key", default) | Returns None if not found
Add/update | d["new_key"] = value | Adds or updates
Delete key | del d["key"] | Raises error if not found
Safe delete | d.pop("key", default) | Removes and returns value
Clear dict | d.clear() | Empties it
Keys | d.keys() | Returns dict_keys view
Values | d.values() | Returns dict_values view
Items | d.items() | Returns key-value pairs

We can iterate through a dictionary either directly or through the methods: `keys()`,`values()` or `items()`.

The most common use case is when you want to map something to another

### Counter
A counter is basically a frequency hashmap of a string. It comes from the `collections` module.

It supports set like operations and is super handy when you want to deal with frequency of string operations and the like.

1. Creating a counter is as easy as:
```python
from collections import Counter
c=Counter("banana")
print(c)    # Counter({'a':3,'b':1,'n':2})
```

2. You can directly access elements of the Counter like a dictionary with its key (the letter). Here `c['a']` will return `3`.

3. You can update the Counter values using `update` and `subtract` functions. Update adds counts from the new value to Counter and subtract will remove counts of the new value from the Counter
```python
c=Counter("banana")
c.update("band") # adding counts from "band"
print(c)    # Counter({'a':4,'b':2,'n':3,'d':1})

c.subtract("bat")   #removes counts from "bat"
print(c)    # Counter({'a':3,'b':1,'n':3,'d':1,'t':-1})
```

4. Arithmetic and set like operations: Counters behave similar to sets. They can perform addition, subtraction, union and intersections.
    - Adding Counters:
    ```python
    >> Counter("aab")+ Counter("ab")
    # Counter({'a':3, 'b':2})
    ```
    - Subtracting Counters: This will remove Counters till values become zero
    ```python
    >> Counter("aab")- Counter("ab")
    # Counter ({'a':1})
    >> Counter("ab")- Counter("aab")
    # Counter ()
    ```
    - Intersection of Counters: It takes the minimum of counts of the common keys
    ```python
    >> Counter("aabbcc") & Counter("bcde")
    # Counter({'b':1, 'c':1})
    ```
    - Union of Counters: It takes the maximum of counts of all the keys
    ```python
    >> Counter("aabbcc") | Counter("bcde")
    # Counter({'a' : 2, 'b':2, 'c':2, 'd':1, 'e':1})
    ```
5. Removing negative and zero counts can be done by adding a Counter() object on top of a Counter.
```python
c = Counter({'a': 2, 'b': -1, 'c': 0})
c += Counter()  # removes non-positive counts
print(c)  # Counter({'a': 2})
```

6. To return the most frequent characters in the Counter we have a function called `most_common()`. It will return a list of tuples of the character along with its frequency
```python
>> Counter("banana").most_common(2)
# [('a',3),('n',2)]