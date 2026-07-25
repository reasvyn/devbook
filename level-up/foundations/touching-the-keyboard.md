# Touching the Keyboard

## 🎯 Description

You have reached the third threshold of the foundations. The first was literacy — the ability to decode and produce text. The second was numeracy — the ability to reason with numbers and quantities. This third threshold is different in kind. It is not a skill you apply to the world. It is a skill you apply to the machine — the machine that will become your workshop, your studio, your instrument. This document addresses minimum digital competency: the ability to turn on a computer, navigate its interface, manage files, open a terminal, and type into a text editor. It does not teach computing. It removes the barrier that prevents computing from being taught.

## 📋 Prerequisites

- [Why Foundations Matter](why-foundations-matter.md) — the philosophical ground: why functional literacy, numeracy, and digital competency are prerequisites to everything that follows
- [Numbers and Reasoning](numbers-and-reasoning.md) *(planned)* — the numerical foundation that precedes this document in the foundations sequence

## 🗺️ Table of Contents

- [The Nature of Digital Competency](#the-nature-of-digital-competency)
- [Device Operation — The First Act](#device-operation--the-first-act)
- [The Terminal — The Developer's native tongue](#the-terminal--the-developers-native-tongue)
- [File Systems — The Architecture of storage](#file-systems--the-architecture-of-storage)
- [Text Editors — The Developer's workbench](#text-editors--the-developers-workbench)
- [Markdown — The Language of DevBook](#markdown--the-language-of-devbook)
- [The Psychology of the First Encounter](#the-psychology-of-the-first-encounter)
- [The Connection to Programming](#the-connection-to-programming)

---

## 🎯 The Nature of Digital Competency

There is a distinction that most people never make explicit, and it matters enormously: the difference between **owning** a device and **using** a device.

### Ownership Versus Competency

A person may own a smartphone, a laptop, a tablet, and a smartwatch — and possess no meaningful ability to operate any of them beyond the most superficial interactions. They tap icons. They scroll feeds. They send messages. They have never managed a file system, never opened a terminal, never written a line of text longer than a social media post. The devices surround them like furniture in a room they have never learned to rearrange.

This is not a moral failing. It is a skills gap, and skills gaps are solvable. But they are solvable only when they are recognized for what they are: not evidence of inability, but evidence of absence. The skill has not been acquired. That is all. It can be acquired.

Digital competency, as defined by the European Commission's DigComp 2.2 framework, is the ability to use digital technology to achieve goals related to work, learning, leisure, and participation in society. The operative word is **achieve**. Not own. Not access. Achieve.

### What This Document Covers

The minimum digital competency for engaging with DevBook involves five capabilities, arranged in a dependency chain:

1. **Device operation** — turning on a computer, navigating a graphical interface, managing basic settings
2. **File management** — creating, locating, moving, renaming, and deleting files and folders
3. **Terminal basics** — opening a command-line interface and executing basic commands
4. **Text editing** — using a text editor to create and modify plain-text files
5. **Markdown literacy** — understanding the lightweight formatting language that DevBook is written in

Each capability builds on the previous one. You cannot manage files without first being able to operate the device. You cannot use the terminal without understanding files and folders. You cannot edit text effectively without understanding what a file is and how the terminal relates to it.

This chain is not arbitrary. It mirrors the actual architecture of computing. The device is the hardware. The operating system manages the hardware and presents an interface. The file system is the operating system's model for storing information. The terminal is an alternative interface to the operating system. A text editor is a program that reads and writes files. Markdown is a convention for formatting text files. Each layer sits on top of the previous one.

Understanding this layering is the first act of digital literacy — recognizing that the computer is not a magical black box but a structured system of interlocking components, each of which can be learned, understood, and mastered.

### The Merit of Starting from Zero

Starting from zero with computers carries a peculiar advantage that experienced users often lack: you have no habits to unlearn. The person who has spent years clicking through graphical interfaces without understanding what lies beneath has accumulated a set of mental models — assumptions about how computing works — that may be deeply wrong. They believe the desktop is where files live. They believe the Recycle Bin is the only way to delete things. They believe the terminal is a place where hackers go.

You have none of these assumptions. Your mind is blank, and blankness is the ideal canvas for accurate knowledge. The experienced user must first dismantle their misconceptions before they can build correct understanding. You can build directly.

This is not consolation. It is structural fact. The beginner who learns correctly from the start is, within a few weeks, better positioned than the intermediate user who learned incorrectly and must now relearn.

## ⚙️ Device Operation — The First Act

Before any software can be learned, the hardware must be understood. This section addresses the minimum operations required to use a computer as a developer's tool.

### Powering On

Every computing session begins with a single physical act: pressing the power button. The machine initializes — hardware components are tested, the operating system is loaded from storage into memory, and the graphical interface appears. This process, called the **boot sequence**, takes anywhere from a few seconds to a minute depending on the hardware.

When the boot sequence completes, you see the operating system's interface. On most machines, this is a desktop — a visual metaphor borrowed from the physical office, complete with a flat surface (the desktop), containers for documents (folders), a waste container (the trash or recycle bin), and tools arranged along the edges (the taskbar or dock).

### The Desktop Metaphor

The desktop metaphor was invented at Xerox PARC in the 1970s and popularized by Apple in 1984. It persists because it maps onto physical experience. You understand what a folder is because you have used physical folders. You understand what a trash bin is because you have thrown things away. You understand what a window is because rooms have windows.

But the metaphor has limits. A physical folder holds papers. A digital folder holds references to files that may be stored anywhere on the disk. A physical trash bin holds waste until it is collected. A digital trash bin holds deleted files until they are permanently removed — or until the bin is emptied and the space is reclaimed. The metaphors are useful starting points, but they obscure the actual mechanics.

A developer must see past the metaphor to the mechanism. The desktop is not a place. It is a view. The folder is not a container. It is a named location in a hierarchical directory structure. The trash is not a bin. It is a temporary holding area for files marked for deletion.

### Navigating the Interface

The graphical interface responds to two primary input devices: the **mouse** (or trackpad) and the **keyboard**. The mouse controls a pointer on the screen. Clicking on an icon opens the associated application. Double-clicking opens a file. Right-clicking reveals a context menu — a list of actions relevant to the clicked element.

The keyboard produces text when a text input field is active. Outside of text fields, keyboard combinations — called **keyboard shortcuts** — perform actions directly. The most essential shortcuts for a developer are:

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` (or `Cmd+C` on macOS) | Copy selected content to clipboard |
| `Ctrl+V` (or `Cmd+V` on macOS) | Paste clipboard content |
| `Ctrl+X` (or `Cmd+X` on macOS) | Cut selected content to clipboard |
| `Ctrl+Z` (or `Cmd+Z` on macOS) | Undo the last action |
| `Ctrl+S` (or `Cmd+S` on macOS) | Save the current file |
| `Alt+Tab` (or `Cmd+Tab` on macOS) | Switch between open applications |

These shortcuts are not optional knowledge. They are the minimum vocabulary of computer interaction. The developer who must use the mouse for every operation moves at a fraction of the speed of the developer who uses keyboard shortcuts. Speed matters not for its own sake but because friction kills momentum, and momentum is the engine of learning.

### Opening Applications

An application is a program — a set of instructions that the computer executes to perform a specific task. The web browser is an application. The text editor is an application. The terminal is an application. Every tool you will use as a developer is an application.

Opening an application typically involves clicking its icon, searching for it by name, or executing it from the terminal. The third method is the one developers use most frequently, and it is addressed in the next section.

## 💻 The Terminal — The Developer's Native Tongue

The terminal — also called the **command line**, the **shell**, or the **console** — is an interface that accepts text commands and returns text responses. It has no windows to drag, no buttons to click, no icons to double. It is a blinking cursor on a blank surface, waiting for you to type.

### Why the Terminal Exists

The terminal is older than the graphical interface. Computing began as a text-only discipline. The first programmers typed commands directly into the machine. There was no mouse, no desktop, no windows. There was a keyboard and a screen, and the conversation between human and machine was entirely textual.

When graphical interfaces arrived in the 1980s, they made computing accessible to people who would never learn command-line syntax. But they also introduced a layer of abstraction between the user and the machine. Every graphical action — every click, every drag, every menu selection — translates into a command that the operating system executes. The graphical interface is a translation layer. The terminal is the original language.

This is why developers return to the terminal. Not out of nostalgia or elitism, but because the terminal provides direct access to the machine's capabilities without the limitations of a graphical interface. Complex operations that would require dozens of clicks in a graphical interface — renaming a thousand files, searching for text across an entire project, running a build script — can be accomplished in a single terminal command.

### Opening the Terminal

Every major operating system includes a terminal application:

- **Windows:** Command Prompt, PowerShell, or Windows Terminal
- **macOS:** Terminal (found in `Applications/Utilities/`)
- **Linux:** GNOME Terminal, Konsole, xterm, or any of dozens of alternatives

Opening the terminal reveals a prompt — a short string of text that indicates the system is ready for input. The prompt typically shows the current user, the current directory, and a symbol (often `$` for regular users or `#` for administrators):

```text
username@hostname:~/project$
```

The blinking cursor after the `$` is where you type commands. This is the moment the document's title describes: touching the keyboard for the first time in a context where the machine is listening.

### Essential Commands

The terminal operates through **commands** — text strings that the shell interprets and executes. The following commands form the minimum vocabulary for navigating a computer from the command line:

**Navigating the file system:**

| Command | Purpose | Example |
|---------|---------|---------|
| `pwd` | Print the current directory | `pwd` |
| `ls` | List files and folders in the current directory | `ls` |
| `cd` | Change to a different directory | `cd Documents` |
| `cd ..` | Move up one directory level | `cd ..` |
| `cd ~` | Move to the home directory | `cd ~` |

**Manipulating files:**

| Command | Purpose | Example |
|---------|---------|---------|
| `mkdir` | Create a new directory | `mkdir my-project` |
| `touch` | Create a new empty file | `touch notes.txt` |
| `cp` | Copy a file | `cp notes.txt backup.txt` |
| `mv` | Move or rename a file | `mv notes.txt diary.txt` |
| `rm` | Delete a file | `rm backup.txt` |

**Reading content:**

| Command | Purpose | Example |
|---------|---------|---------|
| `cat` | Display the contents of a file | `cat notes.txt` |
| `clear` | Clear the terminal screen | `clear` |

A concrete example of how these commands combine into a workflow:

```bash
pwd                          # Where am I?
cd ~                         # Go to home directory
mkdir learning               # Create a folder called "learning"
cd learning                  # Enter the folder
touch first-file.txt         # Create an empty file
echo "Hello, world." > first-file.txt  # Write text into the file
cat first-file.txt           # Read it back
# Output: Hello, world.
```

This sequence — create a directory, enter it, create a file, write to it, read it back — is the fundamental loop of all computing. Everything else is elaboration on this loop. The terminal is where you perform it most directly.

### The Shell

The program that interprets your commands is called the **shell**. It is not the terminal itself — the terminal is the window; the shell is the interpreter inside it. The most common shell on Linux and macOS is **Bash** (Bourne Again SHell). On Windows, the default shells are **Command Prompt** (cmd.exe) and **PowerShell**.

The shell reads your command, parses it, locates the corresponding program on the system, executes it with any arguments you provided, and displays the output. This entire cycle happens in milliseconds. The speed of the feedback loop is one of the terminal's greatest advantages — you type, the machine responds, and the conversation continues without interruption.

## 📁 File Systems — The Architecture of Storage

Every file on a computer exists within a **file system** — a hierarchical structure of directories (also called folders) that organizes data on a storage device. Understanding this structure is essential because every tool a developer uses — the terminal, the text editor, the compiler, the version control system — operates on files within this structure.

### The Directory Tree

The file system is organized as a tree — a branching hierarchy that begins at a single point called the **root directory**. On Linux and macOS, the root is represented by a single forward slash (`/`). On Windows, each drive has its own root (e.g., `C:\`).

From the root, directories branch outward. Each directory can contain files and subdirectories. Those subdirectories can contain their own files and subdirectories. The structure extends downward and outward like the branches of an inverted tree:

```text
/                          ← root directory
├── home/
│   └── username/
│       ├── Documents/
│       │   ├── resume.pdf
│       │   └── cover-letter.txt
│       ├── Downloads/
│       │   └── installer.deb
│       └── Desktop/
│           └── shortcut.lnk
├── etc/
│   └── configuration/
└── var/
    └── log/
        └── system.log
```

This tree structure is not a visual convenience. It is the actual organization of data on the storage device. Every file has a unique **path** — a sequence of directory names separated by slashes that describes its exact location in the tree.

### Absolute and Relative Paths

A path can be expressed in two ways:

**Absolute path:** The full route from the root directory to the file. It always begins with the root symbol.

```text
/home/username/Documents/resume.pdf
```

On Windows:

```text
C:\Users\username\Documents\resume.pdf
```

**Relative path:** The route from the current directory to the target file. It does not begin with the root symbol. The special directory name `..` refers to the parent directory.

```text
# If current directory is /home/username/
Documents/resume.pdf          # relative path to the same file
../Downloads/installer.deb    # go up one level, then into Downloads
```

The distinction between absolute and relative paths is one of the most practically important concepts in computing. Every tool a developer uses — every script, every configuration file, every build system — relies on paths to locate files. An incorrect path means the tool cannot find what it needs, and the error messages that result are among the most common sources of frustration for beginners.

### The Home Directory

Every user on a multi-user system has a **home directory** — a personal directory where their files are stored by default. On Linux and macOS, this is typically `/home/username/`. On Windows, it is `C:\Users\username\`.

The home directory is where you begin. It is your base of operations. From here, you navigate outward into the rest of the file system. The tilde character (`~`) is a shortcut that represents the home directory in terminal commands.

```bash
cd ~        # navigate to home directory
cd          # same effect — cd without arguments goes to ~
```

Understanding the home directory and the path concept resolves one of the most common beginner confusions: "Where did my file go?" The file went somewhere specific in the tree. If you can describe where it is — if you can construct its path — you can find it. If you cannot describe its location, you are navigating blind.

### File Extensions

Files in the file system have **names** and often **extensions** — a suffix at the end of the name, preceded by a period, that indicates the file's type or format. Common extensions include:

| Extension | Typical Content |
|-----------|-----------------|
| `.txt` | Plain text |
| `.md` | Markdown text |
| `.py` | Python source code |
| `.js` | JavaScript source code |
| `.html` | HTML document |
| `.json` | JSON data |
| `.pdf` | PDF document |
| `.jpg` / `.png` | Image files |

Extensions are conventions, not requirements. The operating system uses them to determine which application opens a file, but the extension itself does not change what the file contains. A text file renamed from `notes.txt` to `notes.py` is still a text file — it just happens to have a `.py` extension. This is important to understand because developers routinely create and rename files, and the extension must match the file's actual content for tools to work correctly.

## ✏️ Text Editors — The Developer's Workbench

A **text editor** is a program designed to create and modify plain text — text without formatting, without fonts, without bold or italic or underline, without page margins or columns. A text editor is to a word processor what a whiteboard is to a printed book: raw, flexible, and unadorned.

### Text Editors Versus Word Processors

The distinction between a text editor and a word processor is foundational and often misunderstood:

| Characteristic | Text Editor | Word Processor |
|----------------|-------------|----------------|
| **Output format** | Plain text (`.txt`, `.md`, `.py`) | Formatted document (`.docx`, `.odt`) |
| **Formatting** | None — text only | Bold, italic, fonts, colors, margins |
| **File size** | Small — only the text content | Large — includes formatting metadata |
| **Use by developers** | Primary tool for writing code | Rarely used for code |
| **Examples** | Notepad, VS Code, vim, nano | Microsoft Word, Google Docs, LibreOffice |

Developers use text editors because code is plain text. A Python script does not contain bold text or colored fonts. It contains characters — letters, numbers, punctuation, whitespace. The word processor adds invisible formatting metadata that corrupts code files. The text editor produces exactly what you type, character for character.

### Choosing an Editor

The developer ecosystem offers many text editors, ranging from minimal to feature-rich:

- **Notepad** (Windows) / **TextEdit** (macOS) — the most basic editors, included with the operating system. They open, edit, and save plain text. Nothing more.
- **VS Code** (Visual Studio Code) — a feature-rich editor by Microsoft. Syntax highlighting, integrated terminal, extensions, version control integration. The most popular editor among professional developers.
- **Sublime Text** — a fast, lightweight editor with a plugin ecosystem. Known for speed and responsiveness.
- **vim** / **neovim** — modal editors that operate entirely from the keyboard. A steep learning curve, but extraordinary efficiency once mastered. Pre-installed on virtually every Linux and macOS system.
- **nano** — a simple terminal-based editor with on-screen instructions. The most accessible editor for someone who has never used a terminal.

For a beginner who is touching the keyboard for the first time, **nano** is the recommended starting point. It displays its own command list at the bottom of the screen, eliminating the need to memorize key bindings. Opening it is simple:

```bash
nano notes.txt
```

This opens the file `notes.txt` in the nano editor. If the file does not exist, nano creates it when you save. The cursor sits at the top of the screen. You type. When you have finished, you save and exit with `Ctrl+O` (write out) followed by `Enter`, then `Ctrl+X` (exit).

### Why Editors Matter

The text editor is where developers spend the majority of their working hours. It is the surface on which code is born, revised, and refined. The editor you choose affects your workflow, your speed, and your comfort — but the choice of editor is secondary to the act of editing. Any editor will serve. The important thing is to open one and begin.

## 📝 Markdown — The Language of DevBook

DevBook is written in **Markdown** — a lightweight markup language created by John Gruber in 2004. Markdown allows you to format text using plain-text syntax that is readable in its raw form and can be converted to HTML or other formatted output.

### Why Markdown

Markdown exists because HTML is tedious to write by hand, and word processors are unsuitable for technical documentation. Markdown occupies the middle ground: it is simple enough to write without specialized tools, powerful enough to produce well-structured documents, and universal enough to be supported by virtually every text-based platform.

For DevBook, Markdown is the medium. Every document, every index, every link is plain Markdown. This means you can read DevBook in any text editor, on any operating system, without any special software. It also means that contributing to DevBook requires only the ability to write plain text with Markdown syntax.

### Basic Syntax

The essential Markdown elements:

**Headings** — use `#` symbols at the beginning of a line. The number of `#` symbols determines the heading level:

```markdown
# Heading 1
## Heading 2
### Heading 3
```

**Paragraphs** — separate paragraphs with a blank line:

```markdown
First paragraph.

Second paragraph.
```

**Bold and italic** — wrap text in asterisks or underscores:

```markdown
*italic text*
**bold text**
***bold italic***
```

**Lists** — use `-` for unordered lists and numbers for ordered lists:

```markdown
- First item
- Second item
  - Nested item
  - Another nested item

1. First step
2. Second step
3. Third step
```

**Code** — use backticks for inline code and triple backticks for code blocks:

```markdown
Use the `ls` command to list files.

```bash
ls -la
```
```

**Links** — use square brackets for the text and parentheses for the URL:

```markdown
[DevBook](../index.md)
```

**Images** — use an exclamation mark before the link syntax:

```markdown
![Alt text](path/to/image.png)
```

**Blockquotes** — prefix lines with `>`:

```markdown
> This is a quote.
> It can span multiple lines.
```

**Horizontal rules** — use three hyphens:

```markdown
---
```

This is the complete set of Markdown constructs that DevBook uses. You do not need to memorize them all at once. The documents ahead use these elements consistently, and exposure will build familiarity naturally.

### Reading Markdown

You are reading a Markdown file right now. In its raw form, this file is plain text — the same characters you would see in any `.txt` file. The `#` symbols, the `**` markers, the `---` lines, the ```` ``` ```` fences — these are Markdown syntax. When rendered (displayed by a Markdown-aware tool), they produce the formatted headings, bold text, horizontal rules, and code blocks you see.

Understanding this duality — the raw text and the rendered output — is the key insight. Markdown is both the source and the format. You write Markdown; you read Markdown. The tool that renders it does not add anything you did not provide. It simply interprets your syntax and displays the result.

## 🧠 The Psychology of the First Encounter

The cursor blinks. The terminal is open. You are staring at a blank prompt, and you do not know what to type.

This experience — the experience of the first encounter with an open command line — is one of the most psychologically potent moments in the learning journey. It is the moment when the gap between who you are and who you intend to become feels widest. The cursor does not care about your intentions. It simply waits.

### The Paralysis of Possibility

When the cursor blinks and nothing is typed, the cause is not ignorance. It is the **paradox of possibility** — the paralysis that occurs when the range of possible actions is perceived as infinite and the correct action is unknown.

In a graphical interface, the possibilities are constrained by what is visible. You can click what you see. In the terminal, the possibilities are constrained only by the language, and the language is vast. The beginner does not know what commands exist, what they do, or what will happen when they type them. The result is freezing — a psychological state in which the desire to act is overwhelmed by the fear of acting incorrectly.

### Why Freezing Is Normal

Every experienced developer has experienced this paralysis. It does not disappear with expertise — it shifts. The junior developer freezes at the terminal prompt. The senior developer freezes at the beginning of an unfamiliar system. The principle is the same: when the territory is unknown, the mind hesitates before stepping onto it.

The resolution is also the same: **take one step**. Type a command you know — even if it is only `ls` or `pwd`. Execute it. Observe the result. Then type another. The freezing breaks not through understanding the entire system but through confirming that the system responds to action. The feedback loop — action, response, understanding — is the engine that drives learning forward.

### The Role of Error

The terminal will produce error messages. You will type commands incorrectly. You will spell file names wrong, use the wrong syntax, and attempt operations that the system does not understand. These errors are not failures. They are **the mechanism by which the terminal teaches you**.

An error message is the system telling you what went wrong and, often, what you should have done instead. The beginner reads error messages as evidence of inadequacy. The experienced developer reads them as data — information that narrows the search space and reveals the correct path.

```bash
$ cd learning
bash: cd: learning: No such file or directory
```

This error tells you exactly what happened: the directory `learning` does not exist in the current location. Perhaps you need to create it first. Perhaps you are in the wrong directory. The error is not an indictment. It is a signpost.

### Building the Tolerance

The psychological skill being built in these early encounters is **tolerance for ambiguity** — the ability to act without complete understanding, to accept partial results, and to iterate toward the desired outcome. This skill is not specific to computing. It is the skill of learning itself. Computing simply provides a particularly unforgiving and immediate feedback environment in which to develop it.

The terminal does not grade you. It does not judge you. It does not compare you to other learners. It responds to what you type, and nothing more. The relationship is honest in a way that few human interactions are. This honesty is its gift and its challenge.

## 🔗 The Connection to Programming

This document has covered the minimum digital competency needed to engage with DevBook. It has not taught programming. That is deliberate.

### Foundations Versus Programming

The relationship between this document and the Programming subject is the relationship between learning to hold a hammer and learning to build a house. Holding the hammer is necessary. It is not sufficient. The house requires additional knowledge — materials, techniques, design principles, structural engineering — that cannot be acquired by practicing the grip alone.

Similarly, knowing how to open a terminal, create a file, and type into an editor does not make you a programmer. It makes you **able to begin programming**. The distinction is crucial because it defines the boundary of this document's responsibility and the beginning of the Programming subject's responsibility.

### What Programming Adds

The [Programming subject](../../programming/index.md) addresses what comes next:

- **Variables and data types** — naming values and classifying them
- **Control flow** — making decisions (if/else) and repeating actions (loops)
- **Functions** — packaging reusable logic
- **Data structures** — organizing information for efficient manipulation
- **Error handling** — responding to unexpected conditions
- **Testing** — verifying that code behaves correctly

Each of these concepts builds directly on the digital competency covered here. You must be able to create files before you can write code into them. You must be able to use the terminal before you can run programs. You must be able to edit text before you can debug it. The foundations are not separate from programming — they are the ground on which programming stands.

### The Bridge

The next document in this module — [The First Hour](the-first-hour.md) *(planned)* — provides a structured walkthrough that bridges the gap between this narrative and actual practice. It walks you through your first hour with the machine: turning it on, opening a terminal, creating a file, writing in it, saving it, and reading it back. By the end of that hour, you will have produced a document. That document is proof that you can do this.

After The First Hour, the path leads to the [Programming Foundations](../../programming/foundations/index.md), where the real construction begins. But the construction cannot begin until you have touched the keyboard, and you have touched it now.

---

## 💡 Learning Tips

- **Start with the terminal, not the desktop.** The graphical interface is familiar and comfortable, but the terminal is where developers live. Open the terminal first. Make it your default workspace. The graphical interface is a crutch you will eventually set aside.
- **Type every command yourself.** Reading about commands is not the same as executing them. The muscle memory — the physical act of typing `cd`, `ls`, `mkdir`, `cat` — is built through repetition, not through observation. Type each command at least three times.
- **Do not fear the error message.** Error messages are not punishments. They are the machine telling you what it understood and what it expected. Read them slowly. They contain more information than most beginners realize.
- **Keep a command journal.** Write down every command you learn, what it does, and an example of its use. This journal becomes your personal reference — a document you built yourself, from your own experience, tailored to your own needs. It is also evidence of your progress, which matters when doubt arrives.
- **Practice file navigation daily.** Spend five minutes each day navigating the file system from the terminal. Create directories, create files, move between directories, rename files, delete files. The file system is the geography of computing. You must know it before you can navigate it with confidence.
- **Do not skip the reading.** Markdown is the language of DevBook. Read this document's raw source — the actual `.md` file — to see what Markdown looks like before it is rendered. This demystifies the format and makes it yours.

## 📚 Glossary

| Term | Definition |
|------|------------|
| Absolute Path | A file path that begins at the root directory and describes the complete route to a file |
| Application | A program that performs a specific task — a text editor, a browser, a terminal, or any other software |
| Boot Sequence | The process by which a computer initializes hardware and loads the operating system when powered on |
| Clipboard | A temporary storage area where cut or copied content is held until pasted |
| Command | A text string entered into a terminal that the shell interprets and executes |
| Console | An interface — typically text-based — for interacting with a computer system |
| Directory | A named container in the file system that holds files and other directories; synonymous with folder |
| Extension | A suffix at the end of a filename, preceded by a period, indicating the file's type or format |
| File Extension | See Extension |
| File System | The hierarchical structure that a computer uses to organize and store files on a storage device |
| Graphical Interface | A user interface that uses visual elements — windows, icons, menus — instead of text commands |
| Home Directory | The default directory assigned to a user on a multi-user system; typically the starting point for file navigation |
| Keyboard Shortcut | A combination of keys pressed simultaneously to perform an action without using the mouse |
| Markdown | A lightweight markup language that uses plain-text syntax to produce formatted documents |
| Operating System | The software that manages a computer's hardware and provides services for application programs |
| Path | A sequence of directory names that describes the location of a file within the file system |
| Plain Text | Text without any formatting — no bold, no italic, no fonts, no colors; just characters |
| Relative Path | A file path that describes the route to a file starting from the current directory |
| Root Directory | The topmost directory in a file system; the starting point of the directory hierarchy |
| Shell | A program that interprets and executes commands entered in a terminal |
| Text Editor | A program designed for creating and editing plain text, as distinguished from a word processor |
| Terminal | A text-based interface for communicating with a computer's operating system |
| Word Processor | A program designed for creating formatted documents with styled text, images, and layout |

## 🔗 Quick References

- [LinuxCommand.org](https://linuxcommand.org/lc3_lts0010.php) — comprehensive introduction to the Linux command line for beginners
- [The Missing Semester of Your CS Education (MIT)](https://missing.csail.mit.edu/) — a course covering the tools and practices that computer science programs often skip, including the terminal, version control, and debugging
- [Markdown Guide — Basics](https://www.markdownguide.org/basic-syntax/) — a concise reference for standard Markdown syntax
- [ExplainShell](https://explainshell.com/) — paste a command and get a human-readable explanation of what each part does
- [DevBook: Programming](../../programming/index.md) — the subject that teaches you to build software, beginning with language foundations
- [DevBook: Foundations](../../programming/foundations/index.md) — the entry point for programming: variables, control flow, functions, and collections

## ➡️ Next Steps

- [The First Hour](the-first-hour.md) *(planned)* — a structured, zero-assumption walkthrough of your first hour with a computer and a text editor: turning on the machine, opening a terminal, creating a file, writing, saving, and reading it back
- [Programming Foundations](../../programming/foundations/index.md) — the beginning of actual programming: variables, types, control flow, and the mental models that make code comprehensible
