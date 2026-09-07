## A Simple Example to Help You Fully Understand Coroutines

Coroutines are probably one of the most confusing concepts for Python beginners. They are also an important way to implement concurrent programming in Python. Python can use multithreading and multiprocessing for concurrency, which are relatively well-known approaches. In fact, there is another way to achieve concurrency called asynchronous programming, and coroutines are an essential mechanism for implementing it.

Simply put, coroutines can be understood as multiple sub-programs that cooperate with each other. Within the same thread, when one sub-program is blocked, we can immediately switch from that sub-program to another, thereby preventing the CPU from sitting idle due to blocking. This improves CPU utilization, essentially accelerating program execution through cooperation. So we can say concisely: **coroutines implement cooperative concurrency**.

Let's use a simple example to help you understand what cooperative concurrency is. First, look at the code below.

```Python
import time


def display(num):
    time.sleep(1)
    print(num)


for num in range(10):
    display(num)
```

The code above should be easy to understand. The program outputs the numbers 0 through 9, printing one number per second, so the entire program takes approximately 10 seconds to execute. Note that since we're not using multithreading or multiprocessing, there is only one execution unit in the program, and the `time.sleep(1)` operation causes the entire thread to pause for 1 second. For the code above, the CPU is completely idle during this time and doing nothing.

Now let's see what happens when we use coroutines. Starting from Python 3.5, there is more convenient syntax for implementing cooperative concurrency using coroutines. We can use `async` to define asynchronous functions and use `await` to let a blocked sub-program yield the CPU to a cooperating sub-program. In Python 3.7, `async` and `await` became official keywords, which was exciting news for developers. Let's first see how to define an asynchronous function.

```Python
import asyncio


async def display(num):
    await asyncio.sleep(1)
    print(num)
```

Now here's the key point. Asynchronous functions are different from regular functions: calling a regular function returns a value, while calling an asynchronous function returns a coroutine object. We need to place the coroutine object into an event loop to achieve cooperation with other coroutine objects, because the event loop handles the sub-program switching. Simply put, it lets blocked sub-programs yield the CPU to sub-programs that are ready to execute.

First, we create 10 coroutine objects using the list comprehension below, following the same logic as calling the display function in a loop earlier.

```Python
coroutines = [display(num) for num in range(10)]
```

The following code obtains the event loop and places the coroutine objects into it.

```Python
loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(coroutines))
loop.close()
```

When you execute the code above, you'll find that the 10 coroutines, each of which blocks for 1 second, only block for approximately 1 second in total. This demonstrates that **once a coroutine object blocks, it yields the CPU rather than letting the CPU sit idle**, which significantly **improves CPU utilization**. You'll also notice that the numbers 0 through 9 are not printed in the order in which we created the coroutine objects - which is exactly the result we want! Furthermore, running the program multiple times will produce different output each time, which is a natural consequence of the non-deterministic execution order of concurrent programs.

The example above comes from the well-known "Flower Book" (*Advanced Concurrent Programming with Python*). To deepen your understanding of coroutines, minor modifications were made to the original code. Although this example is simple, it has already given you a taste of the power of cooperative concurrency. In commercial projects, if you need to use cooperative concurrency, you can replace the system's default event loop with the one provided by `uvloop` for better performance, since `uvloop` is built on top of the famous cross-platform asynchronous I/O library libuv. Additionally, for HTTP-based network programming, the third-party library **aiohttp** is an excellent choice, as it implements an asynchronous HTTP server and client based on asyncio.
