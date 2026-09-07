## Use Functions or Complex Expressions

*Larry Wall*, the original author of the Perl language, once said that great programmers all have three virtues: laziness, impatience, and hubris. At first glance, none of these three words seem positive, but in the programmer's world, they carry different meanings. First, laziness drives programmers to write labor-saving programs to help themselves or others get work done better, so we never have to do repetitive and tedious tasks. Likewise, if something can be done in 3 lines of code, we will never write 10 lines. Second, impatience drives programmers to proactively complete work you haven't even asked for yet, to optimize their code for better efficiency - if a task can be done in 3 seconds, we can't tolerate waiting 1 minute. Finally, hubris drives programmers to write reliable, error-free code. We don't write code to receive criticism, but to earn admiration from others.

Now there's an interesting problem worth discussing. We need a program that finds the maximum of three input numbers. This program is a piece of cake for anyone who can program, and even someone who can't program could figure it out after 10 minutes of learning. Here is the Python code to solve this problem.

```Python
a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
if a > b:
	the_max = a
else:
	the_max = b
if c > the_max:
	the_max = c
print('The max is:', the_max)
```

But as we just said, programmers are lazy, and many programmers would use the ternary conditional operator to rewrite the code above.

```Python
a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
the_max = a if a > b else b
the_max = c if c > the_max else the_max
print('The max is:', the_max)
```

It should be noted that Python did not have the ternary conditional operator used in lines 4 and 5 of the code above before version 2.5. The reason was that Guido van Rossum (the creator of Python) believed the ternary conditional operator wouldn't help make Python more concise. So programmers accustomed to using the ternary conditional operator in C/C++ or Java (in those languages, the ternary conditional operator is also called the "Elvis operator" because `?:` together looks like the famous rock singer Elvis's pompadour) tried to simulate it using the short-circuit behavior of the `and` and `or` operators. Back in those days, the code above was written like this.

```Python
a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
the_max = a > b and a or b
the_max = c > the_max and c or the_max
print('The max is:', the_max)
```

However, this approach doesn't work in certain scenarios. See the code below.

```Python
a = 0
b = -100
# The code below was expected to output the value of a, but instead outputs the value of b
# Because the value of a (0) is treated as False in logical operations
print(True and a or b)
# print(a if True else b)
```

So after Python 2.5, the ternary conditional operator was introduced to avoid the risk described above (the last commented-out line in the code above). Now the question is: can the code above be made even shorter? The answer is yes.

```Python
a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
print('The max is:', (a if a > b else b) if (a if a > b else b) > c else c)
```

But is this really a good idea? Doesn't such a complex expression make the code much more obscure? We find that in real-world development, many developers like to overuse certain language features or syntactic sugar, turning simple multi-line code into complex single-line expressions. Is this really good practice? I've asked myself this question more than once, and the answer I can give now is the code below - using helper functions.

```Python
def the_max(x, y):
	return x if x > y else y


a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
print('The max is:', the_max(the_max(a, b), c))
```

In the code above, I defined a helper function `the_max` to find the larger of two values passed as parameters. The output statement below can then find the maximum of three numbers by calling `the_max` twice. Now the code is much more readable, isn't it? Using helper functions to replace complex expressions is really a great choice. The key point is that once the comparison logic is moved into this helper function, it can not only be called repeatedly, but also supports cascading operations.

Of course, in many languages, the function to compare values doesn't need to be implemented yourself (it's usually a built-in function), and Python is no exception. Python's built-in `max` function leverages Python's support for variable arguments, allowing you to pass in multiple values or an iterator at once to find the maximum. So the problem discussed above is just a one-liner in Python. However, the thought process from complex expressions to using helper functions to simplify them is well worth reflecting on, so I'm sharing it here for discussion.

```Python
a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
print('The max is:', max(a, b, c))
```
