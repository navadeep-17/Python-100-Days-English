## Why I Chose Python

Currently, the momentum of the Python language is unstoppable both domestically and internationally. Python has stood out from the crowd of languages with its simple and elegant syntax and powerful ecosystem, and is now firmly seated in the top three of programming language rankings. Many Python developers in China have transitioned from Java development, and I'm no exception. Let me briefly explain why I chose Python.

### Python vs. Java

Let's compare how Java and Python code looks when doing the same things through several examples.

Example 1: Print "hello, world" to the terminal.

Java code:

```Java
class Test {

    public static void main(String[] args) {
        System.out.println("hello, world");
    }
}
```

Python code:

```Python
print('hello, world')
```

Example 2: Sum from 1 to 100.

Java code:

```Java
class Test {

    public static void main(String[] args) {
        int total = 0;
        for (int i = 1; i <= 100; i += 1) {
            total += i;
        }
        System.out.println(total);
    }
}
```

Python code:

```Python
print(sum(range(1, 101)))
```

Example 3: Random lottery number selection (Double Color Ball).

Java code:

```Java
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

class Test {

    /**
     * Generate a random integer in the range [min, max)
     */
    public static int randomInt(int min, int max) {
        return (int) (Math.random() * (max - min) + min);
    }

    public static void main(String[] args) {
        // Initialize candidate red balls
        List<Integer> redBalls = new ArrayList<>();
        for (int i = 1; i <= 33; ++i) {
            redBalls.add(i);
        }
        List<Integer> selectedBalls = new ArrayList<>();
        // Select six red balls
        for (int i = 0; i < 6; ++i) {
            selectedBalls.add(redBalls.remove(randomInt(0, redBalls.size())));
        }
        // Sort the red balls
        Collections.sort(selectedBalls);
        // Add one blue ball
        selectedBalls.add(randomInt(1, 17));
        // Output the selected random numbers
        for (int i = 0; i < selectedBalls.size(); ++i) {
            System.out.printf("%02d ", selectedBalls.get(i));
            if (i == selectedBalls.size() - 2) {
                System.out.print("| ");
            }
        }
        System.out.println();
    }
}
```

Python code:

```Python
from random import randint, sample

# Initialize candidate red balls
red_balls = [x for x in range(1, 34)]
# Select six red balls
selected_balls = sample(red_balls, 6)
# Sort the red balls
selected_balls.sort()
# Add one blue ball
selected_balls.append(randint(1, 16))
# Output the selected random numbers
for index, ball in enumerate(selected_balls):
    print('%02d' % ball, end=' ')
    if index == len(selected_balls) - 2:
        print('|', end=' ')
print()
```

After seeing these examples, I'm sure you can see that my choice of Python is well justified.
