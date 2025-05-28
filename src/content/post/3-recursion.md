---
title: "Some Recursion Hacks"
description: "How to identify whether a problem can be solved using Recursive techniques

"
publishDate: "22 April 2025"
tags: ["coding","tipsntricks"]
---

## Introduction
I spend my days grinding Leetcode so why not put it to good use and make a theoretical cheatsheet of how when and why to use recursion. It always ends up being the smartest solution that impresses interviewers. So recursion is mostly always a great idea. 

It is only not a good idea if the depth of recursion is a lot or if an iterative approach is more time efficient. Also note that recursion is usually a slower operation due to clogging up the call stack and writing to memory.

Recursion requires you to basically look at a big problem in terms of smaller versions of the same problem. We solve the smallest instance of the problem and extend it to the whole problem. Two things we need for recursion are:
1. A base case or a stopping condition
2. A recursive step which has modified parameters with each call

## Functional Programming
FP is basically a way of programming in which we consider the basic units of the language as data and functions, and a composition of them. You can't modify the value of any variable, you can simply just use functions on them to transform them. A *pure function* is basically a function which does not modify any variables and it also provides the same output for the same input every time. An example is a mathematical function like `f(x)=x+2`. 

In a functional language, we only reuse functions and there is no iterative concept like for and while loops. Hence recursion is the only way we execute an iteration. The function will repeatedly call itself until it reaches the base condition. 
### Call Stack
Function calls by default takes up the processor's call stack i.e. a memory which stores which point to return to after the function returns. In case of recursion, the call stack will keep building up until it reaches the limit of the call stack and hits a stack overflow exception.
```python
def fn1(num):
    if num==1:
        return 1
    return num*fn1(num-1)
```
In this example, if we call `fn1(5)`, the call stack would look like:
- fn1(1)
- fn1(2)
- fn1(3)
- fn1(4)
- fn1(5)

It starts popping from the top and returning the value to the caller and in the end computes the factorial of 5.
The pushing and popping at each step are quite costly compared to regular instructions. It involves writing and reading from the stack added with the risk of overflowing the stack. Note here that it is important to store each call since we are actually adding data with every recursive call. For example in the first call we calculate fn1(4) and then multiply 5 on top of it.

Due to the disadvantages it brings, some functional languages like Scala and Haskell have implemented tail recursion optimization. 
### Tail Recursion Optimization
Tail recursion is a way of performing recursion in such a way that the recursive call is only a call to itself with the sufficient parameters. This way we keep overwriting the stack call and the base case will return the required value.
If we consider the same example of computing the factorial and implement tail recursion on it, it would look like this:
```python
def fn1(num, fact=1):
    if num==1:
        return fact
    return fn1(num-1,fact*num)
```

In this example we see that there is only a single call of fn1 in the end, and when it hits the base case it would return the actual answer. We would not need to restore the stack value on every iteration. If we go over this function for fn1(5), the stack initially looks like
- fn1(5,1)

Then instead of appending the stack, it will only update the stack parameters with:
- fn1(4,5)

This keeps repeating till the stack parameters become
- fn1(1,120)

It finally returns `120` immediately when `num=1`. Here we can see that an operation using space complexity _O(N)_ has now effectively been reduced to _O(1)_. There is no possibility of a stack overflow to occur here.
It basically means that with this optimization, recursion can happen at the same speed as their iterative counterpart. The only operation the function does is jumps to itself and it only has to save a single call at any given point reusing the single stack frame.

### Why Python doesn't care for recursion
Python's interpreter does not perform tail recursion due to its own design principles and the logic with which python was built. It generally prefers iteration over recursion.
- *Traceback for debugging*: Python will maintain the entire call stack so that we can observe the stack order when an error occurs for debugging
- *Implementation Complexity*: Tail recursion optimization in Python would add significant complexities in the performance of other operations of the Python interpreter.
- *Preference for Iteration*: Recursion limit in python is usually set to about 1000. Iteration is usually more clear and performs better and hence Python prefers it.

The workaround for this is using third party libraries or decorators which basically convert the recursion into an iteration, and using exceptions to avoid growing the call stack. Chris Penner has written well about how we can use decorators to implement tail recursion in Python in his blog [here](https://chrispenner.ca/posts/python-tail-recursion)

## Applications
Now lets get to where we use recursion. How do we know where to use recursion? The main indicator if we can use recursion in a problem is if we can solve the problem using a subproblem i.e. the same problem but with a smaller range.
In the end, we only need to solve the simplest and most basic case of the problem and simply extend it to the bigger problem. Like in the factorial problem above we:
1. Calculate the factorial of the most simple case i.e. factorial of 1 is 1
2. If the number is not 1 we multiply the current number into the subproblem of the number 1 less than it.
### Sorting
1. **Merge Sort**: We split the list into two and sort each of them and then merge them. The subproblems here are the two sub arrays.
2. **Quick Sort**: We identify a pivot and then solve the subproblem of arranging the subarrays lesser than the pivot and greater than pivot.
### Trees
Trees are a classic problem where we use a lot of recursion. If we consider traversal of a tree, it is a combination of subproblems of its children. If we consider inorder traversal we need to solve the left child subproblem then the root and then the right child subproblem.
```python
def inorderTraversal(root):
    if root is None:
        return 
    inorderTraversal(root.left)
    print(root.val)
    inorderTraversal(root.right)
```

Other recursive manipulation of trees can include finding height of a tree, mirroring a tree and many more problems.

### Dynamic Programming
Many problems in DP are best with a combination of memoisation and recursion. We calculate the solution of a smaller problem and store it so we don't recompute them again. For example if we consider calculating fibonacci number, there are multiple subproblems which we repeatedly solve.
```python
def fib(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)
    return memo[n]
```
This is basically storing all the subproblems in memo. This makes it a fast implementation with minimal code

Another place where we often see DP with recursion is in grid related problems. The solution ends up being a bottom up solution where we start with the base case at the last position and build it up to the top position.


## Pitfalls
Here are common pitfalls that many of us face while writing recursive solutions:
1. Incorrect base case: Its important to correctly identify the smallest subproblem in recursion. It needs to be a scenario which every iteration of the problem needs to solve in order to solve its own problem. It could be `n=0 or n=1` or `root is None` or `node in visited` . 
2. Overlapping and not reusing subproblems: This may not cause errors but we are not being efficient in our solution. Especially in dynamic programming, we store the subproblem solution in a data structure and reuse it when we need it again.
3. Misunderstanding recursion depth limits: This is quite crucial since, if we incorrectly configure the calculation, it can lead to infinite recursive calls. This comes into picture when we are calculating the sub problem of the problem. We need to know if the subproblem will eventually lead to the base case. If not - it can lead to stack overflow.




Hope this helps in writing recursive code and understanding how it works! Thanks for reading :)