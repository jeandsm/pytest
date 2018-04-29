# Introduction to Sets in Python

## Learning Objectives

1. Understand what sets are.
2. Learn how to manipulate sets.
3. Understand why sets are useful with the 'friends in common' example.


## What is a set?

A set is another useful collection item. It differs from the data structures we have seen previously in several ways:
* **Unordered**: Sets are unordered collections, unlike arrays or tuples in which elements are sequenced and can be indexed.
* **No duplicates**: Members of a set are unique, they appear in the set exactly 0 or 1 times.
* **Immutable members**: Members of a set are immutable, typically of type int, float or str. A set cannot contain tuples, lists or dictionaries. Technically, we say that sets cannot contain non-hashable items.

Like dictionaries, sets are very fast. Operations on sets typically take a constant amount of time regardless of their size, making them valuable for large scale applications.


## How do we use sets?

### Construction

In Python, a set can be created by passing an iterable to the set function: : `my_set = set([1,2])`, or `my_set = set()` for an empty list.
We can also pass values directly with curly braces `my_set = {1,2}` but it can be tricky for empty sets or single elements since `my_set = {}` and `my_set = {1}` will create dictionaries. If we do initialise with curly braces, we shouldn't forget the comma: `my_set = {1,}`.

```python
>>> my_set = set([1,2,3,4,5])
# {1,2,3,4,5}
>>> my_set = set([1,1,2,3,4,4,5])
# {1,2,3,4,5}
>>> my_set = set("alabama")
# {a,b,l,m}
>>> my_set = set(range(5))
# {0,1,2,3,4}
```

Once a set exists, we can add elements one by one using the add method or several at a time using update:

```python
>>> my_set = set([1,2])
# {1, 2}
>>> my_set.add(3)
# {1, 2, 3}
>>> my_set.update([1,3,6,9])
# {1, 2, 3, 6, 9}

# Note that in the last call elements are not duplicated
```

You can also remove specific items by using the method remove, or a random element with pop:

```python
>>> my_set = set([1,2,3,4,5])
# {1,2,3,4,5}
>>> my_set.remove(3)
# {1,2,4,5}
>>> my_set.pop()
2
# {1,4,5}

# Note that there is no way to know for sure which element will be removed by pop
```

### Manipulation

The most common operation on sets is **membership testing** to check whether an item exists in a set:

```python
 >>> my_set = set("alabama")
 # {'l','a','b','m'}
 >>> 'b' in my_set
 # True
 >>> 'c' in my_set
 # False
 ```

It is also possible to iterate through a list but there is now way to know in which order it will be performed:
```python
>>> my_set = set("alabama")
>>> for i in my_set:
...    print i
a b l m
```

There are also several operations that can be performed on two or more sets. For example, **issubset** will check whether a set is a subset of another, **intersection** will return the common elements from all sets, union will return all non-duplicate elements from all sets, while **difference** will return the items from the first set that are not found in the second:

```python
>>> set1, set2= {1,2}, {2,3}
>>> set1.union(set2)
{1,2,3}
# Union can also be called with a vertical bar, aka pipe character: set1 | set2
>>> set1.intersection(set2)
{2}
# Intersection can also be called with an ampersand symbol: set1 & set2
>>> set1.difference(set2)
{1}
# Difference can also be called with a minus symbol: set1 - set2
>>> set3 = {2,4}
>>> set1 | set2 | set3
{1,2,3,4}
>>> set1 & set2 & set3
{2}
# Union and intersection work on more than two sets
>>> set4 = {1,2,3,4}
>>> set1.issubset(s4)
True
# All elements in set1 exist in set4, so set1 is a subset of set4
```


## Why are sets useful?

A great reason to use sets is that they allow for **blazing fast operations**, or more precisely a constant speed of operation that does not depend on their size. For instance if we want to check whether an item is a member of an array, doubling the size of the array will double the time to return the information, while with a set it will always take the same amount of time.

### The 'friends in common' example

Imagine we run a social network with billions of members and we need to provide an efficient way to show anyone the list of friends they have with someone else.
We could store everyone's friend list in an array, and then for two given members we could look up the common elements of both arrays. Or we could use sets and look up their intersection.

Let's try both methods and time them with the timeit module, which allows us to run a computation several times to get an idea of its performance:

```python
>>> import timeit

>>> array1, array2 = range(1000), range(100,1000,2)
>>> %timeit test = [i for i in array1 if i in array2]
100 loops, best of 3: 5.04 ms per loop

>>> set1, set2 = set(range(1000)), set(range(100,1000,2))
>>> %timeit test = set1.intersection(set2)
100000 loops, best of 3: 18.1 µs per loop
```

Here we can see that this simple computation, which finds common elements between two collections (one of size 1000, one of size 450), gives very different results depending on the choice of data structure. **Arrays give a computation time of ~5ms where sets compute in ~18µs, about 277 times faster!**
