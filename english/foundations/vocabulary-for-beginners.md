# Vocabulary for Beginners

## Description

The English used in programming has a core vocabulary of roughly 200 words that appear again and again — in documentation, error messages, commit messages, terminal output, and code comments. Mastering these words gives you the ability to understand the vast majority of technical text you will encounter. This document teaches them in context, grouped by where you will see them.

## Prerequisites

- [Reading from Scratch](reading-from-scratch.md) — ability to decode English words by sounding them out

## Table of Contents

- [How Vocabulary Works in Technical English](#how-vocabulary-works-in-technical-english)
- [Words You Will See Everywhere](#words-you-will-see-everywhere)
- [Words in Code](#words-in-code)
- [Words in Error Messages](#words-in-error-messages)
- [Words in Documentation](#words-in-documentation)
- [Words in Git and Collaboration](#words-in-git-and-collaboration)
- [Words in the Terminal](#words-in-the-terminal)
- [Words in File Management](#words-in-file-management)
- [Building Your Own Vocabulary](#building-your-own-vocabulary)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### How Vocabulary Works in Technical English

Technical English reuses everyday words with specialized meanings. When a developer says "the function is **throwing** an error," they do not mean someone is physically throwing something. The word "throw" in programming means "to signal an error."

This is why learning vocabulary in context matters more than memorizing definitions. Each word below is presented in the sentence where you will actually encounter it.

There are three layers to technical vocabulary:

1. **Everyday words with new meanings.** Common English words like "branch," "commit," "flag," and "pipe" acquire precise technical definitions. You already know the word — you need to learn the new context.
2. **Specialized technical terms.** Words like "polymorphism," "segmentation fault," and "higher-order function" exist only in technical contexts. These require direct memorization.
3. **Abbreviations and acronyms.** Terms like "CI/CD," "API," and "CLI" are shortened forms of longer phrases. Recognizing the full phrase helps you remember the abbreviation.

The vocabulary in this document covers all three layers. Focus first on the words you encounter most frequently in your own work — the ones that appear in error messages, documentation, and terminal output every day.

### Words You Will See Everywhere

These words appear in every area of programming — code, documentation, error messages, terminals:

| Word | Meaning | Example sentence |
|------|---------|-----------------|
| **run** | To execute a program | "Run the script with `python app.py`." |
| **output** | What a program produces | "The output shows the calculation result." |
| **input** | What you provide to a program | "The function takes two inputs: a name and a value." |
| **value** | A piece of data | "The variable holds the value `42`." |
| **result** | The outcome of an operation | "The result of `3 + 4` is `7`." |
| **process** | A running program, or to perform a step | "Kill the process and restart it." |
| **data** | Information stored or processed | "The database stores user data." |
| **function** | A reusable block of code | "Call the function with the correct arguments." |
| **return** | To send back a value from a function | "The function returns `true` on success." |
| **argument** | A value passed to a function | "The function takes three arguments." |
| **parameter** | A variable in a function definition | "The parameter `name` receives the argument." |
| **type** | The kind of data (string, number, boolean) | "What type is this variable?" |
| **string** | A sequence of characters (text) | "The name is stored as a string." |
| **number** | A numeric value | "The age must be a number." |
| **boolean** | A true/false value | "The condition returns a boolean." |
| **null** | The absence of a value | "The variable is null." |
| **true** | Boolean value meaning yes/affirmative | "The test passed — the result is true." |
| **false** | Boolean value meaning no/negative | "The file was not found — the result is false." |
| **error** | Something went wrong | "An error occurred during compilation." |
| **warning** | Something might be wrong | "The compiler issued a warning." |
| **success** | The operation completed correctly | "The operation completed successfully." |
| **fail** | The operation did not complete | "The test failed — assertion error." |
| **valid** | Conforming to expected rules | "The input is not a valid email address." |
| **invalid** | Not conforming to rules | "Invalid syntax on line 5." |
| **implement** | To build or create according to a specification | "Implement the sorting algorithm." |
| **configure** | To set up options and settings | "Configure the database connection." |
| **validate** | To check that data meets requirements | "Validate the email format before submitting." |
| **compile** | To convert source code into machine-readable form | "Compile the program before running it." |
| **interpret** | To read and execute code line by line at runtime | "Python interprets the script directly." |
| **debug** | To find and fix errors in code | "Debug the function using print statements." |
| **deploy** | To publish software to a server or environment | "Deploy the application to production." |
| **refactor** | To restructure code without changing its behavior | "Refactor the function to improve readability." |
| **optimize** | To make code faster or more efficient | "Optimize the database query for large datasets." |

### Words in Code

When reading or writing code, these words appear constantly:

**Variables and storage:**

| Word | Meaning | Example |
|------|---------|---------|
| **variable** | A named container for data | "Declare a variable called `count`." |
| **constant** | A value that cannot change | "Define `MAX_SIZE` as a constant." |
| **assign** | To put a value into a variable | "Assign `0` to the counter." |
| **initialize** | To set a starting value | "Initialize the array as empty." |
| **scope** | Where a variable is accessible | "The variable is local to this function scope." |

**Control flow:**

| Word | Meaning | Example |
|------|---------|---------|
| **condition** | An expression that is true or false | "Check the condition before proceeding." |
| **loop** | To repeat a block of code | "Loop through each item in the list." |
| **iterate** | To perform one cycle of a loop | "Iterate over the array elements." |
| **break** | To exit a loop early | "Break the loop if the value is found." |
| **skip** | To ignore the current iteration | "Skip this item if it is null." |

**Data structures:**

| Word | Meaning | Example |
|------|---------|---------|
| **list** | An ordered collection of items | "The function returns a list of strings." |
| **array** | A fixed-size ordered collection | "Access the first element of the array." |
| **dictionary** | A collection of key-value pairs | "Look up the value by its key in the dictionary." |
| **element** | A single item in a collection | "The third element is a string." |
| **key** | An identifier for a value in a dictionary | "Use `'username'` as the key." |

**Operations:**

| Word | Meaning | Example |
|------|---------|---------|
| **concatenate** | To join two strings together | "Concatenate the first and last names." |
| **sort** | To arrange items in order | "Sort the list alphabetically." |
| **filter** | To select items matching a condition | "Filter the results where age > 18." |
| **map** | To transform each item | "Map each number to its square." |
| **reduce** | To combine all items into one value | "Reduce the list to its sum." |

**Object-Oriented Programming:**

| Word | Meaning | Example |
|------|---------|---------|
| **class** | A blueprint for creating objects | "Define a class called `User`." |
| **object** | An instance of a class | "Create an object from the `User` class." |
| **method** | A function defined inside a class | "Call the `save()` method on the object." |
| **property** | A value stored inside an object | "Access the `name` property." |
| **inheritance** | A class deriving behavior from another class | "The `Admin` class inherits from `User`." |
| **polymorphism** | Different types responding to the same interface | "Polymorphism allows `draw()` to work on shapes." |

**Functional Programming:**

| Word | Meaning | Example |
|------|---------|---------|
| **pure** | A function with no side effects | "A pure function always returns the same output." |
| **immutable** | Data that cannot be changed after creation | "Strings are immutable in many languages." |
| **callback** | A function passed as an argument to another function | "Pass a callback to handle the response." |
| **closure** | A function that retains access to its outer scope | "The closure remembers the variable `count`." |
| **higher-order** | A function that takes or returns another function | "Map is a higher-order function." |

**Error Handling:**

| Word | Meaning | Example |
|------|---------|---------|
| **throw** | To signal an error explicitly | "Throw an exception if the input is null." |
| **catch** | To handle an error when it occurs | "Catch the error and log a message." |
| **try** | A block of code that may produce an error | "Wrap risky code in a try block." |
| **finally** | A block that runs regardless of success or failure | "Finally closes the file handle." |
| **exception** | An error that disrupts normal execution | "The exception propagates up the call stack." |

### Words in Error Messages

Error messages follow predictable patterns. Learning these words lets you decode any error:

**Pattern words:**

| Word | Meaning | Example |
|------|---------|---------|
| **cannot** | The program is unable to do something | "Cannot read property of undefined." |
| **expected** | The program was waiting for something else | "Expected `;` but found `}`." |
| **unexpected** | The program found something it did not expect | "Unexpected token." |
| **undefined** | A value that has not been defined | "Variable `x` is undefined." |
| **missing** | Something required is not present | "Missing semicolon at end of statement." |
| **exceeds** | A value is too large | "Value exceeds maximum allowed." |
| **permission** | Authorization to do something | "Permission denied." |
| **denied** | Refused access | "Access denied." |
| **timeout** | Too much time has passed | "Connection timeout after 30 seconds." |
| **refused** | Connection was rejected | "Connection refused." |

**Common error message patterns:**

```
Error: {WHAT went wrong} at {WHERE it happened}
TypeError: {WHAT went wrong}
{NOUN} is not {EXPECTED state}
{NOUN} cannot be {ACTION}
{NOUN} was not {PAST TENSE ACTION}
```

**More error message patterns:**

| Pattern | Meaning | Example |
|---------|---------|---------|
| **segmentation fault** | The program accessed invalid memory | "Segmentation fault (core dumped)" |
| **stack overflow** | Too many nested function calls exceeded the stack size | "RangeError: Maximum call stack size exceeded" |
| **heap out of memory** | The program used more memory than available | "JavaScript heap out of memory" |
| **type mismatch** | An operation received the wrong kind of data | "TypeError: cannot convert string to integer" |
| **syntax error** | The code contains a grammatical mistake | "SyntaxError: unexpected token `;`" |
| **runtime error** | An error that occurs while the program is running | "RuntimeError: division by zero" |
| **logical error** | The code runs but produces wrong results | "The function returned the wrong total" |
| **deadlock** | Two processes are waiting for each other indefinitely | "Deadlock detected — transaction rolled back" |
| **race condition** | The outcome depends on the timing of events | "Race condition: data corrupted during write" |

### Words in Documentation

Documentation uses specific vocabulary to describe software. API references, README files, and developer guides all follow a consistent pattern: they describe what a function accepts, what it produces, what can go wrong, and how to configure it. Learning the words below allows you to read any technical documentation, regardless of the programming language or framework.

| Word | Meaning | Example |
|------|---------|---------|
| **parameter** | An input a function accepts | "Parameters: `url` (string), `options` (object)" |
| **returns** | The output a function produces | "Returns: a Promise resolving to a Response" |
| **throws** | Errors the function might produce | "Throws: `TypeError` if argument is null" |
| **optional** | Not required; you may omit it | "The `callback` parameter is optional." |
| **default** | The value used when none is provided | "Default: `0`" |
| **deprecated** | No longer recommended for use | "This method is deprecated — use `fetch()` instead." |
| **stable** | Safe to use in production | "This API is stable." |
| **experimental** | May change without notice | "This feature is experimental." |
| **platform** | The operating system or environment | "Cross-platform: works on Windows, macOS, Linux." |
| **dependency** | A package this software requires | "Install all dependencies with `npm install`." |
| **configuration** | Settings that control behavior | "Edit the configuration file to change the port." |
| **initialize** | To set up for first use | "Initialize the database before starting the server." |
| **yields** | A function that produces values lazily (generator) | "Yields: an iterator of integers" |
| **awaits** | Waits for an asynchronous operation to complete | "Awaits the Promise before returning." |
| **async** | A function that runs asynchronously | "This method is async and returns a Promise." |
| **sync** | A function that runs synchronously (blocking) | "Use the sync version for simple operations." |
| **static** | A member that belongs to the class, not instances | "Call `Math.max()` — it is a static method." |
| **final** | Cannot be overridden or reassigned | "This method is final in the base class." |
| **abstract** | A class or method that must be implemented by subclasses | "Abstract classes cannot be instantiated directly." |
| **interface** | A contract defining methods a class must implement | "The `Serializable` interface requires a `serialize()` method." |
| **extends** | A class that inherits from another | "The `Admin` class extends `User`." |
| **implements** | A class that fulfills an interface contract | "The class implements the `Comparable` interface." |
| **override** | To replace a parent class method with a new version | "Override the `toString()` method." |

### Words in Git and Collaboration

Version control and team collaboration have their own vocabulary. Git commands, pull request workflows, and project management concepts all use specific terms that appear in daily developer communication. Understanding these words lets you follow along in team meetings, read issue trackers, and participate in code reviews.

| Word | Meaning | Example |
|------|---------|---------|
| **commit** | A saved snapshot of changes | "Commit your changes with a descriptive message." |
| **branch** | A parallel line of development | "Create a branch for the new feature." |
| **merge** | To combine two branches | "Merge the feature branch into main." |
| **conflict** | When two changes contradict each other | "Resolve the merge conflict before proceeding." |
| **pull** | To fetch and integrate remote changes | "Pull the latest changes from origin." |
| **push** | To send local changes to remote | "Push your commits to the remote repository." |
| **clone** | To copy a repository locally | "Clone the repository with `git clone`." |
| **checkout** | To switch to a different branch | "Checkout the `develop` branch." |
| **stage** | To prepare changes for a commit | "Stage the modified files." |
| **revert** | To undo a commit by creating a new one | "Revert the last commit." |
| **review** | To examine code before merging | "The code review found two issues." |
| **approve** | To accept the changes in a review | "The reviewer approved the pull request." |
| **rebase** | To reapply commits on top of another base | "Rebase your branch onto main before merging." |
| **cherry-pick** | To apply a specific commit from another branch | "Cherry-pick the fix commit into the release branch." |
| **stash** | To temporarily shelve uncommitted changes | "Stash your changes and switch branches." |
| **fork** | To create a personal copy of someone else's repository | "Fork the repository to propose changes." |
| **upstream** | The original repository you forked from | "Pull the latest changes from upstream." |
| **downstream** | Repositories that depend on yours | "Downstream projects may break if you change the API." |
| **CI/CD** | Continuous Integration / Continuous Deployment | "The CI/CD pipeline runs tests on every push." |
| **pipeline** | An automated sequence of build, test, and deploy steps | "The pipeline failed at the test stage." |
| **build** | To compile and prepare code for deployment | "The build succeeded — all artifacts are ready." |
| **deploy** | To publish the built software to an environment | "Deploy the new version to staging." |
| **rollback** | To revert to a previous version after a bad deploy | "Roll back to the last stable release." |
| **hotfix** | An urgent fix applied to production | "Deploy a hotfix for the security vulnerability." |
| **release** | A packaged version of the software distributed to users | "The v2.1 release includes three new features." |
| **milestone** | A significant point or event in a project timeline | "Complete all tasks before the milestone deadline." |
| **epic** | A large body of work broken into smaller tasks | "The authentication epic spans three sprints." |
| **sprint** | A fixed time period for completing a set of tasks | "The current sprint ends on Friday." |
| **retrospective** | A meeting to review what went well and what to improve | "The retrospective identified communication gaps." |

### Words in the Terminal

The terminal is where developers interact with the operating system directly. Terminal vocabulary describes the commands, options, data streams, and processes that you will manage on a daily basis. Each word below represents a concept you will use every time you open a terminal window.

| Word | Meaning | Example |
|------|---------|---------|
| **command** | An instruction to the computer | "Type the command and press Enter." |
| **flag** | An option that modifies a command | "Use the `-v` flag for verbose output." |
| **argument** | A value passed to a command | "Pass the filename as an argument." |
| **path** | The location of a file or directory | "Provide the full path to the file." |
| **execute** | To run a command or program | "Execute the script with `./run.sh`." |
| **directory** | A folder in the file system | "Navigate to the project directory." |
| **environment** | The set of variables and settings | "Check your environment variables." |
| **install** | To add a program to your system | "Install the package with `pip install`." |
| **uninstall** | To remove a program | "Uninstall the old version first." |
| **update** | To get the latest version | "Update your packages regularly." |
| **list** | To show items | "List all files in the current directory." |
| **search** | To find something | "Search for the file by name." |
| **alias** | A shortcut for a longer command | "Create an alias so `ll` runs `ls -la`." |
| **export** | To set an environment variable for child processes | "Export the `PATH` variable." |
| **source** | To load a file's commands into the current shell | "Source the `.bashrc` file to apply changes." |
| **history** | A list of recently executed commands | "Check your command history." |
| **pipe** | To send the output of one command as input to another | "Pipe the output to `grep` for filtering." |
| **redirect** | To send output to a file instead of the screen | "Redirect the output to `log.txt`." |
| **stdin** | Standard input — data fed into a program | "Read from stdin using `cat`." |
| **stdout** | Standard output — normal output from a program | "The result is written to stdout." |
| **stderr** | Standard error — error messages from a program | "Errors are sent to stderr, not stdout." |
| **background** | To run a process without blocking the terminal | "Run the process in the background with `&`." |
| **foreground** | A process that is currently active in the terminal | "Bring the process back to the foreground." |
| **daemon** | A background service that runs continuously | "The web server runs as a daemon." |
| **cron** | A time-based job scheduler | "Schedule a backup with a cron job." |
| **symlink** | A symbolic link pointing to another file or directory | "Create a symlink to the shared library." |
| **mount** | To attach a filesystem to a directory | "Mount the external drive at `/mnt/usb`." |

### Words in File Management

Files and directories are the building blocks of any project. File management vocabulary describes how you create, organize, protect, and transport files. These words appear in command-line operations, file manager interfaces, and when configuring access rights on a shared system.

| Word | Meaning | Example |
|------|---------|---------|
| **create** | To make a new file or directory | "Create a new file called `index.html`." |
| **delete** | To remove a file | "Delete the temporary files." |
| **rename** | To change a file's name | "Rename the file to `main.js`." |
| **move** | To change a file's location | "Move the file to the `src/` directory." |
| **copy** | To make a duplicate | "Copy the file before editing." |
| **overwrite** | To replace existing content | "This operation will overwrite the file." |
| **backup** | A copy kept for safety | "Always keep a backup of important files." |
| **temporary** | Short-lived; will be removed | "Store temporary files in `/tmp/`." |
| **source** | The original file (code, data) | "The source code is in `src/`." |
| **destination** | Where something is being moved to | "Specify the destination directory." |
| **symbolic link** | A file that points to another file by name | "Create a symbolic link to the config file." |
| **hard link** | A direct reference to a file's data on disk | "A hard link shares the same inode as the original." |
| **mount** | To attach a filesystem to a directory tree | "Mount the partition before accessing files." |
| **unmount** | To detach a filesystem safely | "Unmount the drive before removing it." |
| **archive** | A collection of files packaged into a single file | "Create an archive of the project files." |
| **compress** | To reduce file size for storage or transfer | "Compress the directory into a `.tar.gz` file." |
| **extract** | To unpack files from an archive | "Extract the downloaded archive." |
| **permissions** | Rules controlling who can read, write, or execute a file | "Change the file permissions with `chmod`." |
| **ownership** | The user and group that own a file | "Check the file ownership with `ls -l`." |
| **group** | A set of users who share file access rights | "Add the user to the `developers` group." |
| **world** | All users on the system | "World-readable means everyone can see it." |
| **read** | Permission to view file contents | "Grant read permission to the group." |
| **write** | Permission to modify file contents | "Only the owner has write permission." |
| **execute** | Permission to run a file as a program | "Make the script executable with `chmod +x`." |

### Building Your Own Vocabulary

The vocabulary in this document is a starting point. As you read more technical text, you will encounter new words. Here is a system for building your own vocabulary:

**Step 1: Encounter.** Read a sentence with an unfamiliar word. Try to understand the meaning from context.

**Step 2: Confirm.** Look up the word in a dictionary or search for it online. Confirm your contextual guess.

**Step 3: Record.** Write the word, its meaning, and the sentence where you found it. Keep a vocabulary notebook (physical or digital).

**Step 4: Use.** Use the word in a sentence of your own. Write a commit message, a comment, or a note that uses the word.

**Step 5: Review.** Revisit your vocabulary list weekly. Words you can recall move to long-term memory. Words you cannot recall need more practice.

**Spaced repetition.** Review new words at increasing intervals — after one day, then three days, then one week, then two weeks. This technique is the most efficient way to transfer vocabulary from short-term to long-term memory. Tools like Anki or physical flashcard systems can automate this schedule.

**Flashcards.** Write the technical word on one side and the definition plus an example sentence on the other. Include the context where you encountered the word — error message, documentation, code review — because context strengthens recall. Review five to ten flashcards per day rather than memorizing an entire list.

**Vocabulary in code reviews.** When reviewing a teammate's code, pay attention to how they describe changes in comments and commit messages. Note unfamiliar words, look them up, and add them to your list. Code reviews expose you to the vocabulary that practitioners actually use, which may differ from textbook terminology.

**Learning from documentation.** Read the documentation of a library or tool you use regularly. Technical documentation is one of the richest sources of domain-specific vocabulary. Start with the README, then explore the API reference. Each section introduces words in context — parameters, return types, error conditions, configuration options. This is vocabulary acquisition in its most natural form.

## Learning Tips

- **Do not try to memorize this entire list at once.** Learn 5–10 words per day. Focus on the words you encounter most often in your own work.
- **Group words by context, not alphabetically.** The "Words in Error Messages" section is more useful than an alphabetical list because you will encounter errors in context.
- **Read error messages slowly.** Most error messages are short sentences. Read each word, decode it, and you will usually understand the problem.
- **Use the words you learn.** Write a commit message that uses "initialize," "function," and "parameter." The act of using a word locks it into memory.
- **Accept that you will not know every word.** Even experienced developers look up words. The goal is not to know every word — it is to know enough to understand the gist and look up what you do not know.
- **Read release notes and changelogs.** These documents are concise summaries of what changed, why, and how. They use a high density of the vocabulary covered in this document and are an excellent source of contextual reading practice.

## Glossary

| Term | Definition |
|------|------------|
| Argument | A value passed to a function or command |
| Boolean | A data type with only two values: true or false |
| Commit | A saved snapshot of code changes in version control |
| Concatenate | To join two strings end to end |
| Dependency | A package or library that a project requires |
| Deprecate | To mark as no longer recommended for use |
| Documentation | Written materials explaining how software works |
| Function | A reusable block of code that performs a specific task |
| Parameter | A variable in a function definition that receives an argument |
| Scope | The region of code where a variable is accessible |
| String | A sequence of characters (text data) |
| Variable | A named container for a value |
| Class | A blueprint for creating objects in object-oriented programming |
| Exception | An error that disrupts normal program execution |
| Refactor | To restructure code without changing its behavior |
| Deploy | To publish software to a server or environment |
| Callback | A function passed as an argument to another function |
| Immutable | Data that cannot be changed after creation |
| Closure | A function that retains access to its outer scope |
| Polymorphism | Different types responding to the same interface |
| Pipeline | An automated sequence of build, test, and deploy steps |
| Daemon | A background service that runs continuously |

## Quick References

- [Python — Built-in Functions](https://docs.python.org/3/library/functions.html) — many of these function names (print, input, type, range) use the vocabulary from this document
- [MDN Web Docs — Glossary](https://developer.mozilla.org/en-US/docs/Glossary) — comprehensive web development glossary
- [Google — Technical Writing Vocabulary](https://developers.google.com/tech-writing/vocabulary) — curated list for technical writers

## Next Steps

- [Writing Basics](writing-basics.md) — learn to construct English sentences and paragraphs
- [Reading Comprehension](reading-comprehension.md) — understand technical paragraphs and instructions
- Back to [English Introduction](../intro/index.md)
