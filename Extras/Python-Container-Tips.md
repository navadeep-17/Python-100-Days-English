## Tips for Using Python Container Types

Python provides a very rich set of container data types. The most commonly known ones include `list`, `tuple`, `set`, `dict`, and more. Below are some tips for using these types, which we hope will help you write more Pythonic code.

#### 1. Finding the Maximum in a Dictionary

Assume the dictionary object is stored in a variable named `my_dict`.

- Get the maximum value

    ```Python
    max(my_dict.values())
    ```

- Get the key with the maximum value

    ```Python
    max(my_dict, key=my_dict.get)
    ```

- Get both the key and the maximum value

    ```python
     max(my_dict.items(), key=lambda x: x[1])
    ```

    or

    ```Python
    import operator

    max(my_dict.items(), key=operator.itemgetter(1))
    ```

    > **Note**: The code above uses the `itemgetter` function from the `operator` module. This function works as shown below. In the code above, `itemgetter` helps us retrieve the second element from a two-element tuple.
    >
    > ```Python
    > def itemgetter(*items):
    >     if len(items) == 1:
    >         item = items[0]
    >         def g(obj):
    >             return obj[item]
    >     else:
    >         def g(obj):
    >             return tuple(obj[item] for item in items)
    >     return g
    > ```

#### 2. Counting Element Occurrences in a List

Assume the list object is stored in a variable named `my_list`.

```Python
{x: my_list.count(x) for x in set(my_list)}
```

or

```Python
from itertools import groupby

{key: len(list(group)) for key, group in groupby(sorted(my_list))}
```

> **Note**: The `groupby` function groups adjacent identical elements together, so we first use the `sorted` function to sort the list in order to place identical elements next to each other.

or

```Python
from collections import Counter

dict(Counter(my_list))
```

#### 3. Truncating a List

Assume the list object is stored in a variable named `my_list`. The usual approach to truncate a list is:
```Python
my_list = my_list[:i]
my_list = my_list[j:]
```

However, a better approach is to use the following operations. Think carefully about why this is better.

```Python
del my_list[i:]
del my_list[:j]
```

#### 4. Implementing Zip by the Longest List

Python's built-in `zip` function produces a generator object that combines elements from two or more iterables together, as shown below.

```Python
list(zip('abc', [1, 2, 3, 4]))
```

Executing the code above produces the following list. As you may have noticed, the number of elements in the list is determined by the shortest iterable passed to `zip`, so the list below only has 3 elements.

```Python
[('a', 1), ('b', 2), ('c', 3)]
```

If you want the number of elements in the final output to be determined by the longest iterable passed to `zip`, try the `zip_longest` function from the `itertools` module, used as follows.

```Python
from itertools import zip_longest

list(zip_longest('abc', [1, 2, 3, 4]))
```

The list created by the code above looks like this.

```Python
[('a', 1), ('b', 2), ('c', 3), (None, 4)]
```

#### 5. Quickly Copying a List

If you want to quickly copy a list object, you can do so through slicing. However, slicing only performs a shallow copy, meaning it creates a new list object, but the elements in the new list are shared with the original list. If you want a deep copy, use the `deepcopy` function from the `copy` module.

- Shallow copy

    ```Python
    thy_list = my_list[:]
    ```

    or

    ```Python
    import copy

    thy_list = copy.copy(my_list)
    ```

- Deep copy

    ```Python
    import copy

    thy_list = copy.deepcopy(my_list)
    ```

#### 6. Operating on Corresponding Elements of Two or More Lists

Python's built-in `map` function can perform a "mapping" operation on elements of an iterable, which is very useful for batch data processing. However, many people don't know that this function can also work with multiple iterables, processing corresponding elements from multiple iterables through the provided function, as shown below.

```Python
my_list = [11, 13, 15, 17]
thy_list = [2, 4, 6, 8, 10]
list(map(lambda x, y: x + y, my_list, thy_list))
```

The operation above produces the following list.

```Python
[13, 17, 21, 25]
```

Of course, the same operation can also be accomplished using `zip` combined with a list comprehension.

```Python
my_list = [11, 13, 15, 17]
thy_list = [2, 4, 6, 8, 10]
[x + y for x, y in zip(my_list, thy_list)]
```

#### 7. Handling None and Zero Values in a List

Assume the list object is stored in a variable named `my_list`. If the list contains `None` values and zero values, we can remove them using the following approach.

```Python
list(filter(bool, my_list))
```

The corresponding list comprehension syntax is shown below.

```Python
[x for x in my_list if x]
```

#### 8. Extracting a Specific Column from a Nested List

Assume `my_list` is a nested list as shown below. This nested list can represent a mathematical matrix. If we want to extract the elements of the first column into a list, we can write it like this.

```Python
my_list = [
    [1, 1, 2, 2],
    [5, 6, 7, 8],
    [3, 3, 4, 4],
]
col1, *_ = zip(*my_list)
list(col1)
```

This gives us the following list, which is exactly the first column of the matrix.

```Python
[1, 5, 3]
```

Similarly, to extract the second column of the matrix into a list, use the following approach.

```Python
_, col2, *_ = zip(*my_list)
list(col2)
```

Following this logic, to perform a matrix transpose, we can write the following code.

```Python
[list(x) for x in zip(*my_list)]
```

After the operation above, we get the following list.

```Python
[[1, 5, 3],
 [1, 6, 3],
 [2, 7, 4],
 [2, 8, 4]]
```
