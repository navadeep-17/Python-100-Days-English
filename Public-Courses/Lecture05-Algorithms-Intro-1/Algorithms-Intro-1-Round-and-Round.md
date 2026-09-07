## Introduction to Algorithms Series 1 - Round and Round Again

### Algorithm Overview

1. What is an algorithm?

   The correct method and specific steps to solve a problem.

   Example 1: How to run a cable between two rooms in two buildings that are 50m apart (both rooms have windows)?

   - Raise a bird (such as a pigeon) to carry the cable across
   - Use a very long pole to pass the cable across
   - Use a drone (remote-controlled aircraft) to carry the cable across

   How to evaluate these methods? **Spend less money, with less effort**!

   Example 2: A large classroom has hundreds of students attending a class together. How to quickly count the number of students?

   Example 3: Insert 100,000 elements into a list container in **reverse** order.

   - Method 1:

     ```Python
     nums = []
     for i in range(100000):
         nums.append(i)
     nums.reverse()
     ```

   - Method 2:

     ```Python
     nums = []
     for i in range(100000):
         nums.insert(0, i)
     ```

   Example 3: Generate a Fibonacci sequence (the first 100 Fibonacci numbers).

   - Method 1 - Iteration:

     ```Python
     a, b = 0, 1
     for num in range(1, 101):
         a, b = b, a + b
         print(f'{num}: {a}')
     ```

   - Method 2 - Recursion:

     ```Python
     def fib(num):
         if num in (1, 2):
             return 1
         return fib(num - 1) + fib(num - 2)


     for num in range(1, 101):
         print(f'{num}: {fib(num)}')
     ```

   - Method 3 - Improved recursion:

     ```Python
     def fib(num, temp={}):
         if num in (1, 2):
             return 1
         elif num not in temp:
             temp[num] = fib(num - 1) + fib(num - 2)
         return temp[num]
     ```

   - Method 4 - Improved recursion:

     ```Python
     from functools import lru_cache


     @lru_cache()
     def fib(num):
         if num in (1, 2):
             return 1
         return fib(num - 1) + fib(num - 2)
     ```

2. How to evaluate an algorithm?

   [Asymptotic time complexity](<https://zh.wikipedia.org/wiki/%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6>) and asymptotic space complexity.

3. What does Big ***O*** notation mean?

   It represents the growth rate of a function relative to the input size, also known as the order of the function.

   | Big *O* Notation  | Description                       | Examples                                                    |
   | ----------------- | --------------------------------- | ----------------------------------------------------------- |
   | $$O(c)$$          | Constant time complexity          | Bloom filter / Hash storage                                 |
   | $$O(log_2n)$$     | Logarithmic time complexity       | Binary search                                               |
   | $$O(n)$$          | Linear time complexity            | Sequential search / Bucket sort                             |
   | $$O(n*log_2n)$$   | Log-linear time complexity        | Advanced sorting algorithms (merge sort, quicksort)         |
   | $$O(n^2)$$        | Quadratic time complexity         | Simple sorting algorithms (selection sort, insertion sort, bubble sort) |
   | $$O(n^3)$$        | Cubic time complexity             | Floyd's algorithm / Matrix multiplication                   |
   | $$O(2^n)$$        | Exponential time complexity       | Tower of Hanoi                                              |
   | $$O(n!)$$         | Factorial time complexity         | Travelling salesman problem                                 |

### Exhaustive Search

In computer science, **exhaustive search** or **brute-force search** is a very straightforward problem-solving method. This method works by enumerating all possible candidates for the solution and checking whether each candidate satisfies the problem description, ultimately arriving at the solution.

Although brute-force search is easy to implement and will always find a solution if one exists, its cost is proportional to the number of candidate solutions. Because of this, in many practical problems, the cost grows rapidly as the problem size increases. Therefore, brute-force search can be used when the problem size is limited or when there are techniques available to reduce the set of candidate solutions to a manageable size. Additionally, this method can be considered when simplicity of implementation is more important than speed.

### Classic Examples

1. **Hundred Coins, Hundred Chickens** problem: A rooster costs 5 coins each, a hen costs 3 coins each, and chicks cost 1 coin for three. Using 100 coins to buy exactly 100 chickens, how many roosters, hens, and chicks are there?

   ```Python
   for x in range(21):
       for y in range(34):
           z = 100 - x - y
           if z % 3 == 0 and 5 * x + 3 * y + z // 3 == 100:
               print(x, y, z)
   ```

2. **Five People Dividing Fish** problem: Five people A, B, C, D, and E went fishing together one night and eventually fell asleep exhausted. The next day, A woke up first, divided the fish into 5 piles, threw away the extra 1 fish, and took his share; B woke up second, also divided the fish into 5 piles, threw away the extra 1 fish, and took his share; then C, D, and E woke up in turn, each dividing the fish in the same way. What is the minimum number of fish they caught?

   ```Python
   fish = 6
   while True:
       total = fish
       enough = True
       for _ in range(5):
           if (total - 1) % 5 == 0:
               total = (total - 1) // 5 * 4
           else:
               enough = False
               break
       if enough:
           print(fish)
           break
       fish += 5
   ```

3. **Brute-force password cracking**:

   ```Python
   import re

   import PyPDF2

   with open('Python_Tricks_encrypted.pdf', 'rb') as pdf_file_stream:
       reader = PyPDF2.PdfFileReader(pdf_file_stream)
       with open('dictionary.txt', 'r') as txt_file_stream:
           file_iter = iter(lambda: txt_file_stream.readline(), '')
           for word in file_iter:
               word = re.sub(r'\s', '', word)
               if reader.decrypt(word):
                   print(word)
                   break
   ```