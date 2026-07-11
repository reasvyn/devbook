# First Steps in Programming

## Description

Your foundations are ready. Now you learn to speak the language of computers. This document guides you from knowing what programming is to writing working code, understanding core concepts, and building the mental model that makes you a real programmer.

## Prerequisites

- [Building Foundations](building-foundations.md) — computer literacy, command-line basics, English proficiency, and foundational math

## Table of Contents

- [What Is Programming?](#what-is-programming)
- [Choosing Your First Language](#choosing-your-first-language)
- [Core Concepts (The Universal Building Blocks)](#core-concepts-the-universal-building-blocks)
- [Problem-Solving and Algorithmic Thinking](#problem-solving-and-algorithmic-thinking)
- [Writing and Running Your First Program](#writing-and-running-your-first-program)
- [Practice Makes Permanent](#practice-makes-permanent)
- [Milestones for This Phase](#milestones-for-this-phase)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Programming?

Programming is the act of giving instructions to a computer. That is all. You tell the computer what to do, step by step, in a language it can interpret. A program is nothing more than a sequence of those instructions, saved to a file and executed.

Computers are literal. They do exactly what you tell them to do, not what you meant to tell them. This is the most important concept in programming and the source of nearly every bug you will ever encounter. The computer has no intuition, no common sense, and no ability to infer your intent. If you tell it to divide by zero, it will try. If you tell it to access an element that does not exist, it will crash. The machine is a perfect, unfailingly obedient servant that needs every step spelled out explicitly.

This precision is what makes programming both frustrating and powerful. Frustrating, because a single misplaced character can break everything. Powerful, because once you give the correct instructions, the computer will execute them perfectly, billions of times, without getting tired or making mistakes.

Programming is the foundation of software engineering. Before you can design systems, architect solutions, or lead teams, you must first master the act of giving instructions to a machine. That is where every software engineer starts.

For a broader exploration of what programming is and why it matters, see [What Is Programming?](../../programming/intro/what-is-programming.md).

### Choosing Your First Language

The first language you learn matters less than actually learning it. Most programmers know multiple languages, and the first one is rarely the one they use for the rest of their career. The goal is not to master a specific syntax. The goal is to internalize concepts — variables, loops, functions, conditionals — that transfer to every language.

That said, your first language shapes how you think about programming. A good first language is readable, forgiving, and has a gentle learning curve. It should let you make progress quickly and not punish you for small mistakes.

#### Python (Recommended)

Python is the most common first language taught in universities and bootcamps today, for good reason:

- **Readable syntax** — Python code looks almost like plain English. There are no curly braces, semicolons, or arcane punctuation. Indentation defines structure, which trains you to write clean code from day one.
- **Immediate feedback** — Python is an interpreted language. You write code and run it immediately. No compile step, no waiting. This makes the edit-run-debug cycle fast.
- **Huge community** — Whatever problem you face, someone has solved it in Python. Documentation, tutorials, and Stack Overflow answers are abundant.
- **Most forgiving** — Python does not require you to declare types. It handles memory management automatically. It has a large standard library that handles common tasks. You focus on logic, not ceremony.
- **Vast ecosystem** — Python is used in web development, data science, automation, scripting, game development, and AI. The skills you learn are immediately applicable.

```python
# Python version of "Hello, World!"
print("Hello, World!")
```

#### JavaScript (Alternative)

JavaScript is the language of the web. If you want to see visual results immediately, JavaScript lets you build things that run in a browser. You can create buttons that respond to clicks, animations, and interactive pages within your first week of learning.

```javascript
// JavaScript version of "Hello, World!"
console.log("Hello, World!");
```

The downside is that JavaScript has more historical baggage than Python. Its type system has quirks, and the ecosystem changes rapidly. It is not worse — just less beginner-friendly.

#### The Trap: Language Choice Paralysis

You will see debates online about which language is best. Beginners spend weeks asking "Should I learn Python or JavaScript?" instead of learning either one. The answer is always the same: pick one and start. If you cannot decide, pick Python. If that decision bothers you, pick JavaScript. Either choice is infinitely better than not starting.

Every concept you learn in your first language transfers. Once you understand loops and functions in Python, learning them in JavaScript takes an afternoon. The investment is in concepts, not syntax.

For a structured curriculum in either language, see the [Programming Fundamentals](../../programming/fundamentals/index.md) module.

### Core Concepts (The Universal Building Blocks)

All programming languages share a common set of concepts. Once you learn these, you can pick up any language. Each concept is presented with a Python example and a link to deeper reading.

#### Variables

A variable is a named container that stores a value. Think of it as a labeled box. You put something in the box, and later you can look inside the box to retrieve it.

```python
name = "Alice"
age = 25
temperature = 36.6
is_raining = False
```

Variables let you store data, refer to it by name, and change it later. The name should describe what the variable holds. Well-named variables make code self-documenting. Badly named variables (like `x`, `temp`, `data`) make code impossible to understand.

For the full treatment: [Variables](../../programming/fundamentals/variables.md).

#### Data Types

Every value in a program has a type. The type determines what you can do with the value. The three most common types are:

- **Numbers** — integers (`42`) and floating-point numbers (`3.14`). Used for counting, measuring, and arithmetic.
- **Strings** — text enclosed in quotes (`"hello"`, `'world'`). Used for names, messages, labels.
- **Booleans** — `True` or `False`. Used for conditions and decisions.

```python
# Numbers
count = 10
price = 19.99

# Strings
greeting = "Hello"
name = 'Alice'

# Booleans
is_valid = True
has_permission = False
```

Python also has `None`, which represents the absence of a value. It is not zero, not an empty string — it is nothing.

For the full treatment: [Data Types](../../programming/fundamentals/data-types.md).

#### Operators

Operators are the symbols that perform operations on values. There are three categories you will use constantly:

**Arithmetic operators** perform math: `+`, `-`, `*`, `/`, `//` (integer division), `%` (remainder), `**` (exponent).

```python
sum = 10 + 5          # 15
difference = 10 - 5   # 5
product = 10 * 5      # 50
quotient = 10 / 3     # 3.333...
remainder = 10 % 3    # 1
power = 2 ** 3        # 8
```

**Comparison operators** compare values and return a boolean: `==` (equal), `!=` (not equal), `<`, `>`, `<=`, `>=`.

```python
is_equal = (5 == 5)         # True
is_greater = (10 > 20)      # False
is_not_equal = (5 != 3)     # True
```

**Logical operators** combine boolean values: `and`, `or`, `not`.

```python
can_enter = (age >= 18) and (has_ticket == True)
is_weekend = (day == "Saturday") or (day == "Sunday")
is_not_finished = not is_done
```

For the full treatment: [Operators and Expressions](../../programming/fundamentals/operators-and-expressions.md).

#### Control Flow

Control flow statements let your program make decisions and repeat actions. Without them, your program runs from top to bottom and stops.

**Conditionals (if/else)** execute code only when a condition is true.

```python
temperature = 30

if temperature > 35:
    print("It is very hot today.")
elif temperature > 20:
    print("It is warm.")
else:
    print("It is cool.")
```

**Loops** repeat a block of code. A `for` loop iterates over a sequence. A `while` loop repeats while a condition is true.

```python
# for loop — repeat a fixed number of times
for i in range(5):
    print(f"Iteration {i}")

# while loop — repeat while condition holds
count = 0
while count < 5:
    print(f"Count is {count}")
    count += 1
```

For the full treatment: [Control Flow](../../programming/fundamentals/control-flow.md).

#### Functions

A function is a reusable block of code that performs a specific task. You give it a name, define what it does, and then call it whenever you need that task done.

```python
def greet(name):
    """Return a greeting string for the given name."""
    return f"Hello, {name}! Welcome to programming."

message = greet("Alice")
print(message)  # Hello, Alice! Welcome to programming.
```

Functions are the primary tool for organizing code. They let you:
- Avoid repeating yourself (DRY principle — Don't Repeat Yourself)
- Break complex problems into small, testable pieces
- Give names to operations, making your code read like a story

For the full treatment: [Functions](../../programming/fundamentals/functions.md).

#### Collections

Collections let you store multiple values in a single structure. The two most common are lists and dictionaries.

**Lists** store ordered sequences of values. You can add, remove, and access elements by position.

```python
fruits = ["apple", "banana", "cherry"]
fruits.append("date")          # Add to end
first_fruit = fruits[0]        # "apple" — index starts at 0
print(len(fruits))             # 4
```

**Dictionaries** store key-value pairs. You look up values by their keys, like a real dictionary.

```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "Tokyo"
}
print(person["name"])          # "Alice"
person["age"] = 26             # Update value
```

For the full treatment: [Collections](../../programming/fundamentals/collections.md).

#### Error Handling

Things go wrong. Files are missing. Users enter invalid input. Networks fail. Error handling is how you deal with these situations without crashing your program.

```python
try:
    number = int(input("Enter a number: "))
    result = 10 / number
    print(f"Result: {result}")
except ValueError:
    print("That was not a valid number.")
except ZeroDivisionError:
    print("Cannot divide by zero.")
```

The `try` block contains code that might fail. If it does, the program jumps to the matching `except` block instead of crashing. This is called "catching" an exception.

Error handling is not punishment — it is a sign that you are thinking like an engineer. Professional software anticipates failure.

For the full treatment: [Error Handling](../../programming/fundamentals/error-handling.md).

### Problem-Solving and Algorithmic Thinking

Programming is not about knowing syntax. It is about solving problems. The syntax is just the tool you use to express the solution. You could memorize the entire Python manual and still be unable to write a program if you cannot decompose a problem into steps.

Algorithmic thinking is the skill of taking a problem, breaking it into small pieces, and expressing each piece as a step the computer can execute. This skill transfers across every language, framework, and domain.

#### The Problem-Solving Process

When faced with a programming problem, follow these steps:

**1. Understand the problem.** Read the problem statement carefully. Ask questions: What input does the program receive? What output should it produce? What are the edge cases? If you cannot explain the problem in plain English, you are not ready to code.

**2. Break it down into small steps.** Divide the problem into sub-problems. Each sub-problem should be small enough that you can solve it in one function. If a step feels too big, break it further.

**3. Write each step as code.** Translate each small step into the programming language you are using. Do not worry about elegance yet. Get it working first.

**4. Test and fix.** Run your code with sample inputs. Compare the output to what you expected. If it does not match, trace through your logic and find where the deviation occurs. Fix it and test again.

#### Pseudocode: Think Before You Code

Before writing actual code, write pseudocode — a plain-language description of the steps your program will take. Pseudocode has no syntax rules. It is just you thinking on paper.

```
PROGRAM: Check if a number is even
INPUT: a number
OUTPUT: "even" or "odd"

1. Read the number from the user
2. If the number divided by 2 has remainder 0:
3.     Print "even"
4. Else:
5.     Print "odd"
```

Writing pseudocode forces you to think through the logic before worrying about syntax. It separates problem-solving from programming. Most experienced engineers sketch solutions in pseudocode or comments before writing real code.

#### Tracing: Walk Through Your Code

Tracing means executing your program in your head, line by line, keeping track of what each variable contains. This is the single most effective debugging skill.

```python
# What does this program print?
x = 10
y = 3
result = x // y           # integer division: 10 // 3 = 3
remainder = x % y         # 10 % 3 = 1
print(f"{x} divided by {y} is {result} with remainder {remainder}")
```

If you can trace through this and predict the output without running it, you understand variables, operators, and execution order. Practice tracing every piece of code you write.

#### Small Exercises

The best way to develop algorithmic thinking is to solve small problems. These are classic beginner exercises that teach you to think in steps:

- **FizzBuzz:** Print numbers 1 to 100. For multiples of 3, print "Fizz". For multiples of 5, print "Buzz". For multiples of both, print "FizzBuzz".
- **Find the largest number:** Given three numbers, find the largest.
- **Reverse a string:** Given "hello", produce "olleh".
- **Count vowels:** Given a string, count how many vowels it contains.
- **Check palindrome:** A word reads the same forward and backward (racecar, level).
- **Sum of list:** Given a list of numbers, calculate the total.

Each of these problems can be solved in 5-15 lines of code. Each one forces you to think about loops, conditionals, variables, and string manipulation. Do not skip them. They are the programming equivalent of scales and arpeggios for a musician.

### Writing and Running Your First Program

Theory is useless without practice. Let us write and run actual code.

#### Setting Up Python

1. Open a terminal.
2. Check if Python is installed by running:
   ```
   python --version
   ```
   or if that does not work:
   ```
   python3 --version
   ```
3. If Python is not installed, download it from [python.org](https://python.org) or use your package manager:
   ```
   sudo apt install python3    # Debian/Ubuntu
   brew install python3         # macOS
   ```
4. You also need a text editor. Any plain-text editor works. Do not use a word processor like Word or Google Docs. VS Code, Sublime Text, or even Notepad will work.

#### Your First Program

Create a file called `hello.py` with the following content:

```python
print("Hello, World!")
```

Save the file. Open your terminal in the directory where you saved it. Run:

```
python hello.py
```

Or on some systems:

```
python3 hello.py
```

You should see:

```
Hello, World!
```

You just wrote and executed a program. This is the same fundamental process — write code, run it, see output — that professional software engineers use thousands of times a day.

#### The Edit-Run-Debug Cycle

Software development is a loop:

1. **Edit** — write or change code in your editor.
2. **Run** — execute the code and observe the result.
3. **Debug** — if the result is wrong, figure out why and fix it.
4. Repeat.

This loop is fast in Python. You write a line, run it, see the result, fix it, run again. The faster you can cycle through this loop, the faster you learn. Optimize for short feedback loops.

#### Common Errors and How to Read Error Messages

Beginners panic when they see error messages. Do not. Error messages are the computer telling you exactly what went wrong. They are helpful, not hostile.

**SyntaxError:** You wrote something that is not valid Python. Usually a missing colon, parenthesis, or quote.

```
  File "hello.py", line 1
    print("Hello, World!"
                         ^
SyntaxError: unexpected EOF while parsing
```

Fix: Add the missing closing parenthesis.

**NameError:** You used a variable name that does not exist. Usually a typo.

```
NameError: name 'prnt' is not defined. Did you mean 'print'?
```

Fix: Check the spelling.

**TypeError:** You performed an operation on the wrong type of value.

```
TypeError: can only concatenate str (not "int") to str
```

Fix: Convert the number to a string: `"Age: " + str(25)`.

**IndexError:** You tried to access an element at a position that does not exist.

```
IndexError: list index out of range
```

Fix: Check the list length before accessing, or use a loop.

The most important skill in debugging is reading the error message. It tells you:
- The file and line number where the error occurred.
- The type of error.
- A description of what went wrong.

Always start by reading the error message from top to bottom. The bottom line is the cause. The top lines are the chain of calls that led there. For the full treatment, see [Debugging](../../programming/fundamentals/debugging.md) and [Programming Fundamentals](../../programming/fundamentals/intro/programming-fundamentals.md).

### Practice Makes Permanent

You cannot learn programming by reading. You learn by doing. The concepts will feel abstract and confusing until you write them yourself. Every programmer goes through this. The difference between those who succeed and those who quit is not talent — it is consistent practice.

#### Where to Practice

**Coding challenge platforms** provide structured exercises with instant feedback:

- **Exercism** — starts with guided tracks that teach you concepts step by step. The best choice for a complete beginner.
- **Codewars** — ranked challenges (kyu levels) that increase in difficulty. Good after you have completed a few weeks of practice.
- **LeetCode** — more algorithm-focused, commonly used for interview preparation. Use the "Easy" problems first.
- **HackerRank** — structured tracks by topic. Good for practicing specific areas like loops or strings.

Do not compare yourself to others on these platforms. The goal is to improve your own understanding, not to achieve a high rank. Solve one problem a day. If you cannot solve it, look at the solution, understand why it works, and come back to it the next week.

#### Build Tiny Projects

After you have done 20-30 guided exercises, build something from scratch. The project does not need to be original or useful. It needs to be something you plan, build, and complete.

- **Calculator** — takes two numbers and an operator, performs the calculation, prints the result.
- **To-do list** — stores tasks, lets you add and remove them, displays the list.
- **Number guessing game** — the computer picks a random number, you guess, it tells you higher or lower.
- **Temperature converter** — converts between Celsius and Fahrenheit.
- **Mad Libs** — takes words from the user and inserts them into a story template.
- **Simple quiz** — asks multiple-choice questions, keeps score, shows results.

Each project will force you to combine multiple concepts: variables, input/output, conditionals, loops, and functions. You will hit problems you did not anticipate, which is exactly the point.

#### Read Others' Code

Find simple projects on GitHub and read the code. Try to understand what each line does. Even if you only understand 20 percent, you are learning how real programs are structured. Look at how the author organized functions, named variables, and handled errors.

#### Join Communities

Programming is a social activity. Communities help you when you are stuck and give you perspective when you feel overwhelmed:

- **r/learnprogramming** — supportive community for beginners. No question is too basic.
- **r/Python** — Python-specific discussions and resources.
- **Coding discords** — many have beginner-help channels where you can ask questions in real time.

Do not ask for help without trying first. Spend at least 15 minutes trying to solve the problem yourself. Search for the error message. Then post what you tried and what went wrong. You will get better help, and you will learn more from the experience.

#### Consistency Over Intensity

Thirty minutes every day is more effective than five hours once a week. Programming is a skill that benefits from the constant background processing your brain does between sessions. A daily habit keeps the syntax fresh and the concepts present.

If you miss a day, do not make it two. If you are stuck, do something smaller. Do not stop.

### Milestones for This Phase

You have completed this phase when you can do all of the following:

- Write a program that takes input from the user, processes it, and produces output
- Explain in your own words what variables, loops, conditionals, and functions are and when to use each
- Read a basic error message (SyntaxError, NameError, TypeError, IndexError) and fix the problem
- Trace through a simple program line by line and predict the output
- Complete at least 20-30 small exercises on a coding platform
- Build 1-2 tiny projects from scratch without following a tutorial
- Break down a simple problem into steps (pseudocode) before writing code
- Understand that programming is about problem-solving, not syntax memorization

Do not rush through this phase. The foundation you build here determines how fast you progress later. A programmer who rushed through variables and loops will struggle with data structures and algorithms. A programmer who mastered them will fly.

## Glossary

| Term | Definition |
|------|------------|
| Variable | A named container that stores a value in memory |
| Function | A reusable block of code that performs a specific task, optionally taking input and returning output |
| Loop | A construct that repeats a block of code multiple times until a condition is met |
| Conditional | A construct that executes different code depending on whether a condition is true or false (if/elif/else) |
| Debugging | The process of finding and fixing errors in code |
| Exception | An error that occurs during program execution, which can be caught and handled |
| String | A sequence of characters, used to represent text in a program |
| Boolean | A data type with two values: True and False, used in conditions and logical operations |
| Integer | A whole number (positive, negative, or zero), without a decimal point |
| Float | A number with a decimal point, used for real numbers in calculations |
| List | An ordered collection of items, accessible by index position |
| Dictionary | A collection of key-value pairs, where each value is accessed by its unique key |
| Operator | A symbol that performs an operation on values (e.g., + for addition, == for comparison) |
| Expression | A combination of values, variables, and operators that evaluates to a single value |
| Statement | A single line of code that performs an action (e.g., assignment, function call) |
| Parameter | A variable listed in a function's definition that receives a value when the function is called |
| Argument | The actual value passed to a function when calling it |
| Return Value | The value that a function produces and sends back to the caller |
| Scope | The region of a program where a variable is accessible and can be used |
| Pseudocode | A plain-language description of a program's logic, used for planning before writing actual code |
| Algorithm | A step-by-step procedure for solving a problem or accomplishing a task |
| Trace | The process of manually executing code line by line to verify behavior |
| Syntax | The set of rules that defines the structure of a valid program in a given language |
| Syntax Error | An error caused by writing code that does not follow the language's grammar rules |
| Runtime Error | An error that occurs while the program is running, often due to invalid operations or data |
| Logic Error | An error where the program runs without crashing but produces incorrect results |
| IDE (Integrated Development Environment) | A software application that provides a code editor, debugger, and other tools in one interface |
| REPL (Read-Eval-Print Loop) | An interactive programming environment that executes code one statement at a time |
| Iteration | One complete execution of the body of a loop |
| Recursion | A technique where a function calls itself to solve a smaller instance of the same problem |

## Quick References

- [Python.org Official Tutorial](https://docs.python.org/3/tutorial/) — The official Python tutorial, written by the creators of the language. Start here for authoritative coverage.
- [Exercism Python Track](https://exercism.org/tracks/python) — Guided, mentored coding exercises that teach Python through practice.
- [Learn Python the Hard Way](https://learnpythonthehardway.org/) — A book that teaches Python through deliberate, repetitive practice.
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/) — Free book focused on practical Python for real-world tasks.
- [Python Tutor](https://pythontutor.com/) — A visual tool that shows how Python executes code line by line, useful for tracing.
- [r/learnprogramming](https://reddit.com/r/learnprogramming) — A supportive community for beginner programmers to ask questions and share progress.

## Next Steps

- [Core Computer Science](core-computer-science.md) — level up your understanding with data structures, algorithms, and the theory behind the code
- Back to [The Journey Ahead](intro/the-journey-ahead.md) — see your progress and plan the road forward
