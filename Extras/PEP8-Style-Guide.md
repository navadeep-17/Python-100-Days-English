## PEP 8 Style Guide

PEP stands for Python Enhancement Proposal. Each PEP is a technical document that provides guidance to the Python community on how to improve Python. The 8th enhancement proposal (PEP 8) is a code style guide for the Python language. While we can write Python code however we want as long as the syntax is correct, in real-world development, writing readable code with a consistent style is something every professional programmer should do. It is also a requirement in every company's coding standards, which becomes especially important during collaborative multi-person development (team development). You can find this document on the Python official website at [PEP 8 link](https://www.python.org/dev/peps/pep-0008/). Below, we provide a brief summary of the key parts of the document.

### Use of Spaces

1. <u>Use spaces for indentation rather than tabs.</u> This may seem unreasonable for those accustomed to other programming languages, since the vast majority of programmers use tabs for indentation. However, Python does not have curly braces for defining code blocks like C/C++ or Java. In Python, branches and loop structures use indentation to indicate which code belongs to the same level. Because of this, Python code depends on indentation and indentation width much more than many other languages. In different editors, a tab can be 2, 4, 8 characters wide, or even some other value. Using tabs for indentation can be a disaster for Python code.
2. <u>Use 4 spaces for each level of syntactic indentation.</u>
3. <u>Each line should not exceed 79 characters. If an expression is too long and spans multiple lines, all lines except the first should add 4 extra spaces on top of the normal indentation width.</u>
4. <u>Use two blank lines to separate function and class definitions from surrounding code.</u>
5. <u>Within a class, methods should be separated by a single blank line.</u>
6. <u>There should be one space on each side of a binary operator, and only one space.</u>

### Identifier Naming

PEP 8 advocates using different naming styles for different types of identifiers in Python, so that the role of an identifier can be determined by its name when reading code (Python's own built-in modules and some third-party modules don't always do this very well themselves).

1. <u>Variables, functions, and attributes should be written in lowercase letters, with words separated by underscores.</u>
2. <u>Protected instance attributes in a class should start with a single underscore.</u>
3. <u>Private instance attributes in a class should start with two underscores.</u>
4. <u>Classes and exceptions should use CamelCase (capitalize the first letter of each word).</u>
5. <u>Module-level constants should be written in all uppercase letters, with words separated by underscores.</u>
6. <u>Instance methods of a class should name the first parameter `self` to represent the object itself.</u>
7. <u>Class methods should name the first parameter `cls` to represent the class itself.</u>

### Expressions and Statements

In the Zen of Python (which can be viewed using `import this`), there is a famous saying: "There should be one-- and preferably only one --obvious way to do it." This philosophy is pervasive throughout PEP 8.

1. <u>Use inline negation instead of placing the negation before the entire expression.</u> For example, `if a is not b` is easier to understand than `if not a is b`.
2. Don't check the length to determine whether a string, list, etc. is `None` or empty. Use the `if not x` pattern instead.
3. <u>Even if an `if` branch, `for` loop, or `except` block contains only one line of code, don't write it on the same line as `if`, `for`, or `except`. Writing them on separate lines makes the code clearer.</u>
4. <u>`import` statements should always be placed at the top of the file.</u>
5. <u>When importing modules, `from math import sqrt` is better than `import math`.</u>
6. <u>If there are multiple `import` statements, they should be divided into three sections, from top to bottom: Python **standard library** modules, **third-party** modules, and **custom** modules. Within each section, modules should be sorted in **alphabetical order** by module name.</u>
