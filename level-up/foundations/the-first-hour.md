# The First Hour

## Description

This is the hour in which you stop being someone who uses a computer and become someone who makes things with one. Every developer — every person who has ever shipped code, built a system, or written a document that mattered — began with an hour like this one. You will turn on a machine, open a terminal, create a file, write words into it, save it, and read it back. By the end, you will have produced something that did not exist before. That is not a small thing. That is the beginning of everything.

## Prerequisites

- [Touching the Keyboard](touching-the-keyboard.md) — familiarity with the physical act of typing and the layout of a keyboard
- [Why Foundations Matter](../why-foundations-matter.md) — the philosophical ground for starting from zero without shame

## Table of Contents

- [The Machine in Front of You](#the-machine-in-front-of-you)
- [The Desktop Is Not the World](#the-desktop-is-not-the-world)
- [The Terminal: A Window Into the Machine](#the-terminal-a-window-into-the-machine)
- [Six Commands That Open Doors](#six-commands-that-open-doors)
- [Building Your First Directory](#building-your-first-directory)
- [The Text Editor: Where Things Get Made](#the-text-editor-where-things-get-made)
- [Writing Your First Document](#writing-your-first-document)
- [Saving the Work](#saving-the-work)
- [Reading It Back](#reading-it-back)
- [What You Just Did](#what-you-just-did)

---

## 🖥️ The Machine in Front of You

The computer is on the desk, or on the table, or on your lap. It is closed, or it is black, or it is showing a picture of something that is not what you need. This is fine. Everything begins here.

### Turning It On

Find the power button. On a laptop, it is usually a button on the top row of the keyboard, sometimes marked with a circle-and-line symbol — a small circle with a vertical line emerging from its top. On a desktop, it is a button on the front of the tower, or sometimes on the monitor. Press it.

What happens next depends on the machine. The screen may light up. A logo may appear. A progress indicator may spin. The fans may whir. All of these are signs that the machine is waking up. Do not rush it. The machine needs time to load its operating system — the software that makes all other software possible. This can take ten seconds or two minutes. Both are normal.

When the machine finishes starting, you will see a screen asking for a password, a PIN, or perhaps nothing at all. This is the login screen. Type your password if one is required. If you do not know your password, this is a problem to solve before continuing — ask whoever set up the machine. If the machine logs in automatically, you will land on the desktop.

### What the Desktop Is

The desktop is the first screen you see after logging in. It is a metaphor — the operating system's way of giving you a familiar starting point. It usually shows:

- A background image (a photograph, a color, a pattern)
- Icons (small images representing programs, files, or shortcuts)
- A taskbar or dock (a strip at the bottom or top of the screen with more icons)
- A clock (usually in one of the corners)

Everything you can see on this screen is a representation of something deeper. The icons are shortcuts to programs. The programs are tools. The tools manipulate files. The files live in directories — organized folders on the machine's storage. None of this is visible on the surface. The desktop is the lobby of a very large building, and most of the rooms are behind closed doors.

You will learn to open those doors. But first, you need to learn to move.

### The Mouse and the Trackpad

If you have a mouse, move it. Watch the screen. A small pointer — an arrow, a hand, a beam of light — moves in response. This is the cursor. It is your hand inside the machine. Where the cursor goes, your attention goes. Where you click, things happen.

If you have a trackpad (the flat rectangle below the keyboard on a laptop), slide your finger across it. The cursor responds the same way. A light press and release of the trackpad is a click. A firmer press is a right-click. You can also often tap the trackpad once for a click, or tap with two fingers for a right-click.

A **left-click** selects things, opens things, and activates things.

A **right-click** opens a context menu — a small list of options that appears next to the cursor, offering actions relevant to whatever you right-clicked on.

If you see a file icon on the desktop, move the cursor over it and double-click (two quick presses). The file opens. You have just launched your first program — even if you did not realize it.

## 🪟 The Desktop Is Not the World

The desktop is convenient, but it is not where developers spend their time. Developers work in two primary environments: the **file manager** and the **terminal**. The file manager is a visual interface — windows showing folders and files, navigated with a mouse. The terminal is a text-based interface — a blank screen with a cursor, navigated by typing commands.

The file manager is useful for browsing. The terminal is essential for building. You need both, but the terminal is where real power lives. Learning the terminal is like learning to drive a car when you have only ever been a passenger. The file manager lets you see where you are going. The terminal lets you go there faster, with more control, and to places the file manager cannot reach.

### Finding the Terminal

The terminal — also called the command line, the console, the shell, or the command prompt — is a program. You find it the same way you find any program:

**On Linux:**
- Open the application menu (often a grid icon in the bottom-left or top-left corner)
- Search for "Terminal" or "Konsole" or "GNOME Terminal"
- Click the icon that appears

**On macOS:**
- Press `Cmd + Space` to open Spotlight Search
- Type `Terminal`
- Press Enter when the Terminal app appears

**On Windows:**
- Press the `Windows` key on the keyboard
- Type `Terminal` or `cmd` or `PowerShell`
- Press Enter when the appropriate app appears

When the terminal opens, you will see a window with a blinking cursor. It may show some text — the name of your user, the name of the machine, a dollar sign (`$`) or a percent sign (`%`). This text is called the **prompt**. It is the machine asking: what would you like me to do?

The blinking cursor is where you type. The machine is waiting. This is the moment where many people freeze — the blank terminal feels intimidating because it offers no hints, no buttons, no visual cues. There is nothing to click. There is only the cursor, the prompt, and your willingness to type.

Type something. Type `hello`. Press Enter. The machine will respond — likely with an error message like `command not found: hello`. That is fine. You just had your first conversation with the terminal. The machine listened, processed your input, and responded. The error message is not a failure. It is proof that the machine is alive and paying attention.

## ⚡ Six Commands That Open Doors

The terminal responds to **commands** — short text instructions you type and press Enter to execute. Each command is a word, sometimes followed by additional words that modify its behavior. These additional words are called **arguments** or **flags**. You do not need to memorize all commands. You need six to start.

### 1. `pwd` — Where Am I?

```bash
pwd
```

Type `pwd` and press Enter. The terminal will print a path — a string of text that tells you exactly where you are in the machine's file system. It might look like:

```
/home/yourusername
```

This is your **home directory** — the machine's name for your personal space. Every file you create, every document you write, every program you build will live somewhere inside this space. The path is the address.

`pwd` stands for **print working directory**. It answers the question: where am I right now?

### 2. `ls` — What Is Here?

```bash
ls
```

Type `ls` and press Enter. The terminal will list the contents of the directory you are in — all the files and subdirectories at this location. The output might be empty (if there is nothing here yet) or it might show names like `Documents`, `Downloads`, `Desktop`.

`ls` stands for **list**. It answers the question: what is in this room?

You can combine `ls` with arguments to see more:

```bash
ls -la
```

The `-l` flag tells `ls` to show details (permissions, sizes, dates). The `-a` flag tells it to show hidden files (files whose names begin with a dot). Together, they show you everything.

### 3. `cd` — Go Somewhere Else

```bash
cd Desktop
```

Type `cd Desktop` and press Enter. You have now moved into the `Desktop` directory. If you type `pwd` again, you will see:

```
/home/yourusername/Desktop
```

`cd` stands for **change directory**. It is how you move through the machine's file system — walking from room to room, from folder to folder.

To go back up one level (out of the current directory and into its parent), type:

```bash
cd ..
```

The two dots (`..`) are a universal convention: they mean "the parent directory." One dot (`.`) means "the current directory."

To go back to your home directory from anywhere, type:

```bash
cd ~
```

The tilde (`~`) is shorthand for your home directory. It is always available, no matter where you are in the file system.

### 4. `mkdir` — Create a Folder

```bash
mkdir my-project
```

Type `mkdir my-project` and press Enter. You have just created a new directory named `my-project` inside your current location. If you type `ls`, you will see it listed.

`mkdir` stands for **make directory**. A directory is a folder — a container for files. Organizing your work into directories is how you keep things from becoming chaos.

You can create nested directories in one step with the `-p` flag:

```bash
mkdir -p projects/devbook
```

This creates `projects/` and then creates `devbook/` inside it, all in one command.

### 5. `touch` — Create a File

```bash
touch hello.md
```

Type `touch hello.md` and press Enter. You have just created an empty file named `hello.md` in the current directory. The file has no content yet — it is a blank page.

`touch` stands for... well, it technically updates the file's timestamp. But its most common use is creating empty files. The `.md` at the end is the **file extension** — it tells the machine (and you) that this is a Markdown file. Extensions are conventions, not laws, but they are conventions worth following.

### 6. `cat` — Read a File

```bash
cat hello.md
```

Type `cat hello.md` and press Enter. Since the file is empty, nothing will appear — the terminal will simply show a new prompt. That is correct. The file exists, but it contains nothing.

`cat` stands for **concatenate**, but its simplest use is displaying the contents of a file. When you add content to `hello.md` later, `cat` will show you what is inside.

### The Commands at a Glance

| Command | Purpose | Mnemonic |
|---------|---------|----------|
| `pwd` | Show current directory | **P**rint **W**orking **D**irectory |
| `ls` | List contents of current directory | **L**i**s**t |
| `cd` | Move to a different directory | **C**hange **D**irectory |
| `mkdir` | Create a new directory | **M**a**k**e **Dir**ectory |
| `touch` | Create a new empty file | Touch a blank page into existence |
| `cat` | Display the contents of a file | Cat (concatenate) a file to the screen |

## 📂 Building Your First Directory

Now you will create a dedicated space for your work. This is important — not because the machine requires it, but because you do. A clean workspace is a clear mind. Everything you build in this journey deserves a home.

Open the terminal. Navigate to your home directory:

```bash
cd ~
```

Verify where you are:

```bash
pwd
```

Expected output:

```
/home/yourusername
```

Now create a directory for your DevBook work:

```bash
mkdir devbook-work
```

Move into it:

```bash
cd devbook-work
```

Verify:

```bash
pwd
```

Expected output:

```
/home/yourusername/devbook-work
```

You are now standing inside a clean, empty directory that belongs to you. This is your workshop. Everything you create in this journey starts here.

List the contents to confirm it is empty:

```bash
ls -la
```

You should see two special entries: `.` (the current directory) and `..` (the parent directory). These exist in every directory. They are the floor and the ceiling of the room you are in. Ignore them for now. What matters is that there is nothing else here — the space is yours to fill.

## ✏️ The Text Editor: Where Things Get Made

You have a directory. Now you need a tool to write in it. That tool is a **text editor** — a program designed specifically for creating and editing plain text files. Do not confuse this with a word processor like Microsoft Word or Google Docs. A word processor formats text — bold, italic, font sizes, margins, columns. A text editor writes text. Plain, unformatted, raw text. This distinction matters because code, configuration files, and Markdown are all plain text. They do not need formatting. They need content.

### Choosing an Editor

There are many text editors. For your first hour, you need one that is simple, free, and available on your operating system.

**Visual Studio Code** (commonly called VS Code) is the most widely used editor among developers. It is free, runs on all major operating systems, and has a clean interface that makes basic editing straightforward.

To install VS Code:

1. Open a web browser
2. Navigate to `https://code.visualstudio.com`
3. Click the download button for your operating system
4. Run the installer and follow the prompts

If you cannot install VS Code, or if you prefer something lighter, here are alternatives:

| Editor | Platform | Notes |
|--------|----------|-------|
| VS Code | All | Full-featured, large community |
| Notepad++ | Windows | Lightweight, fast |
| TextEdit (plain text mode) | macOS | Pre-installed, minimal |
| Nano | All (terminal) | Built into every terminal, zero setup |
| Gedit | Linux | Simple, pre-installed on many distributions |

If you have nothing else, you have the terminal. Type `nano hello.md` and you are editing. Nano shows its commands at the bottom of the screen. `Ctrl + O` saves. `Ctrl + X` exits. That is enough.

### Opening the Editor

If you installed VS Code, you can open it from the terminal:

```bash
code .
```

The period (`.`) means "the current directory." VS Code will open with your `devbook-work` folder as the workspace. You will see the folder structure on the left side and an empty editor area on the right.

If you are using a different editor, open it from your application menu and then use its "Open Folder" or "Open File" option to navigate to `devbook-work`.

You now have a text editor open, pointed at your workshop directory. The cursor is blinking in an empty file. The stage is set.

## 📝 Writing Your First Document

Create a new file in your editor. If you are using VS Code, press `Ctrl + N` (or `Cmd + N` on macOS) to create a new tab. Then press `Ctrl + S` (or `Cmd + S`) to save it. Name the file `hello-world.md`. Make sure it is saved inside your `devbook-work` directory.

Now type the following content. Type it exactly as shown. Do not copy and paste — typing it yourself is part of the process. You are building muscle memory. You are teaching your fingers that the keyboard is a tool, not a foreign object.

```markdown
# Hello, World

This is my first document. I made it myself.

## What I Did Today

- I turned on the computer
- I opened a terminal
- I created a directory
- I opened a text editor
- I wrote this document
- I saved the file

## What I Learned

The terminal is not as scary as it looks.
The commands are just words that the machine understands.
The text editor is just a place to write.

## My Next Step

I will read this document in a browser, and then I will keep going.
```

Take a moment to look at what you have written. You are using **Markdown** — a lightweight markup language that uses plain text characters to indicate structure. Here is what those characters mean:

- The `#` symbol creates a **heading**. One `#` is the largest heading. Two `##` is a subheading. Three `###` is a smaller subheading.
- The `-` symbol creates a **list item**. Each line that starts with `-` becomes a bullet point.
- Blank lines separate paragraphs and elements.

Markdown is the language DevBook is written in. It is the language that documentation, README files, and developer notes are written in across the entire software industry. Learning Markdown is learning to speak the native language of the web.

### What You Are Looking At

The editor shows your text. There may be a line number in the left margin. There may be a cursor blinking at the end of your last line. There may be syntax highlighting — the editor coloring different parts of the text differently (headings in one color, list bullets in another). All of this is normal. All of it is the editor helping you see the structure of your document.

The content is simple. It does not need to be complex. What matters is that you wrote it — that you decided what to say and then said it, character by character, line by line, on a machine that did not exist in this form a generation ago.

## 💾 Saving the Work

If you have not already saved the file, save it now.

- **VS Code:** `Ctrl + S` (Windows/Linux) or `Cmd + S` (macOS)
- **Notepad++:** `Ctrl + S`
- **Nano:** `Ctrl + O` then Enter
- **Any editor:** Look for "Save" in the File menu

When you save, the file writes from the computer's temporary memory (RAM) to its permanent storage (the hard drive or solid-state drive). Before saving, your work exists only in the machine's volatile memory — if the power went out, it would vanish. After saving, it exists on disk. It is permanent. It will survive a restart, a shutdown, a crash.

This distinction matters more than it seems. The act of saving is an act of commitment — you are declaring that what you have written is worth keeping. In software development, saving is not a final step. It is an intermediate one. You will save, change, save, change, save again. The file is never finished. It is always in progress. But it must be saved at each stage, or the progress is lost.

Save the file. Confirm the name is `hello-world.md`. Confirm the location is `devbook-work`. Now the file is on disk.

### Verifying from the Terminal

Open your terminal (or switch back to it). Make sure you are in the right directory:

```bash
cd ~/devbook-work
ls
```

You should see:

```
hello-world.md
```

Your file is there. It exists. The terminal confirms it. Read its contents:

```bash
cat hello-world.md
```

Expected output:

```
# Hello, World

This is my first document. I made it myself.

## What I Did Today

- I turned on the computer
- I opened a terminal
- I created a directory
- I opened a text editor
- I wrote this document
- I saved the file

## What I Learned

The terminal is not as scary as it looks.
The commands are just words that the machine understands.
The text editor is just a place to write.

## My Next Step

I will read this document in a browser, and then I will keep going.
```

There it is. The words you typed, displayed by the machine, exactly as you wrote them. The terminal read the file from disk and printed it to the screen. This is the most basic act of computing: create a file, store it, read it back. Everything else — every application, every website, every operating system — is built on this loop.

## 🌐 Reading It Back

The terminal shows you the raw Markdown — the plain text with its `#` symbols and `-` markers. But Markdown is designed to be rendered — to be transformed from plain text into a formatted document with headings, lists, and structure. Seeing the rendered version is seeing your document as others will see it.

### Option 1: The Browser

1. Open your file manager (the visual folder browser on your computer)
2. Navigate to `devbook-work`
3. Right-click on `hello-world.md`
4. Select "Open With" and choose your web browser (Firefox, Chrome, Edge, Safari)

The browser will display the Markdown. The `#` symbols will become large headings. The `-` symbols will become bullet points. The blank lines will become paragraph spacing. Your raw text has become a formatted document.

### Option 2: VS Code Preview

If you are using VS Code:

1. Open `hello-world.md` in the editor
2. Press `Ctrl + Shift + V` (Windows/Linux) or `Cmd + Shift + V` (macOS)

A preview pane opens alongside your editor, showing the rendered Markdown. As you edit the text on the left, the preview updates on the right in real time.

### Option 3: A Dedicated Markdown Viewer

Some editors and websites offer Markdown preview. The principle is the same: the plain text is parsed, the Markdown syntax is interpreted, and a formatted document is produced. The underlying text does not change. Only its presentation changes.

Look at the rendered document. The heading "Hello, World" is large and bold. The list items have bullets. The paragraphs are separated. This is your document. You wrote every word of it. The machine did not generate it, suggest it, or autocomplete it. You typed it, saved it, and now you are reading it.

This is what creation feels like.

## ⚔️ What You Just Did

Pause for a moment. Do not skip ahead. Do not minimize this section and move on to something more advanced. Read this, and let it settle.

You turned on a machine. You navigated an operating system. You opened a terminal — a tool that professional developers use every day — and you spoke to it in its own language. You created directories and files from the command line. You opened a text editor, wrote a document in Markdown, saved it to disk, and read it back in a browser.

Each of these actions is small. None of them is difficult in isolation. But the sequence — the full sequence from power button to rendered document — is the foundation of everything that follows in this journey. Every program you will write, every system you will build, every document you will publish begins with this loop: create, save, view, iterate.

### The Significance of Making Something

There is a psychological shift that happens when you produce a tangible artifact. Before this hour, the computer was a consumer device — a screen you looked at, content someone else made. After this hour, the computer is a production tool — a machine you use to bring things into existence.

This shift is not trivial. It is the same shift that occurs when a person who has only ever eaten food cooked by others picks up a knife and cooks their first meal. The meal may be simple — rice and beans, a fried egg, toast. It does not matter. The act of creation changes the relationship between the person and the tool. The kitchen is no longer someone else's space. The computer is no longer someone else's machine. You have claimed it. You have made it yours.

The document you created — `hello-world.md` — is not a masterpiece. It is not meant to be. It is proof. Proof that you can do this. Proof that the distance between "I do not know how to use a computer" and "I just made a document on a computer" is one hour long. Not a semester. Not a year. One hour.

### The Hello World Tradition

In programming, the first program a person writes is traditionally called "Hello, World." The tradition began in the 1970s with Brian Kernighan, a computer scientist at Bell Labs, who used the phrase in his tutorials. The idea was simple: before you learn anything complex, make the machine say "Hello, World." It is the smallest possible proof that the system works — that the tools are connected, that the pipeline from input to output is intact.

Your Markdown document is a variation on this tradition. You did not make the machine say "Hello, World." You said it yourself, in your own words, in a document you created. The effect is the same: you have verified that the system works. The tools are connected. The pipeline from intention to creation is intact.

### What This Means for the Journey Ahead

This hour is the first step of the level-up journey. It is not a detour into technical skill. It is not a prerequisite you had to clear before getting to the "real" content. It is the real content. The act of making — of choosing to build something rather than merely consume it — is the psychological foundation on which everything else rests.

You are no longer someone who is going to learn to code. You are someone who has already started. The file exists. The directory exists. The terminal responded. The browser rendered. These are facts. They are evidence. And when doubt arrives — and it will arrive, whispering that you are not smart enough, not technical enough, not cut out for this — you can point to this hour and say: I made that. I was there. I did the thing.

That is enough to keep going.

---

## 💡 Learning Tips

- **Type everything by hand.** Resist the urge to copy and paste code examples. Typing builds muscle memory, reinforces command syntax, and forces you to engage with every character. Speed comes later. Accuracy comes now.
- **Expect errors.** The terminal will reject your commands. It will say `command not found`, `no such file or directory`, `permission denied`. These are not failures. They are feedback. Read the error message. It tells you what went wrong and often suggests how to fix it.
- **Use `pwd` liberally.** When in doubt about where you are, type `pwd`. It costs nothing and orients you instantly. Lost is a feeling. `pwd` is a fact.
- **Make the terminal your friend, not your enemy.** The terminal feels hostile because it is terse — it gives you the minimum information needed. But terseness is not hostility. It is efficiency. The terminal respects you enough to not waste your time. Return the respect by learning its language.
- **Keep the `devbook-work` directory sacred for now.** Do not create files randomly on the desktop or in Downloads. Practice the discipline of organized space from the beginning. Future you will be grateful.
- **Revisit this hour tomorrow.** Turn off the machine. Turn it back on. Open the terminal. Navigate to `devbook-work`. Read the file with `cat`. The repetition reinforces the path. The second time is easier. The third time is natural. The tenth time is automatic.
- **Celebrate the small win.** You made a document. You read it back. The machine listened to you. That is not nothing. That is everything.

## 📚 Glossary

| Term | Definition |
|------|------------|
| Command | A text instruction typed into the terminal that the operating system executes |
| Command Line | A text-based interface for interacting with the operating system, as opposed to a graphical interface |
| Console | Another name for the terminal or command line interface |
| Cursor | The blinking indicator on screen that shows where text will appear when you type |
| Desktop | The visual workspace shown after logging into a graphical operating system |
| Directory | A container for files and other directories — the file system equivalent of a folder |
| Extension | A suffix at the end of a filename (like `.md` or `.txt`) that indicates the file's type |
| File Manager | A graphical program for browsing, opening, and organizing files and folders |
| Home Directory | The default directory assigned to a user — the starting point for navigation |
| Markdown | A lightweight markup language that uses plain text symbols to indicate formatting and structure |
| Path | A string of text that describes a file's or directory's location in the file system |
| Plain Text | Text without formatting — no bold, no italic, no font sizes, no colors |
| Prompt | The text the terminal displays to indicate it is ready for a command |
| Render | The process of transforming plain text with Markdown syntax into a formatted visual document |
| Shell | The program that interprets and executes commands entered in the terminal |
| Syntax Highlighting | The color-coding of different parts of text in an editor to indicate their role or meaning |
| Text Editor | A program designed for creating and editing plain text files |
| Terminal | A text-based interface for communicating with the operating system via typed commands |
| Working Directory | The directory you are currently inside — the location where commands operate by default |

## 🔗 Quick References

- [GNU Coreutils — Basic Commands](https://www.gnu.org/software/coreutils/manual/coreutils.html) — official documentation for `ls`, `cat`, `mkdir`, `touch`, `pwd`, and other essential Unix utilities
- [VS Code Documentation](https://code.visualstudio.com/docs) — the official guide for Visual Studio Code, covering editing, navigation, and terminal integration
- [Markdown Guide — Getting Started](https://www.markdownguide.org/getting-started/) — a clear, concise introduction to Markdown syntax and its uses
- [The Missing Semester of CS Education (MIT)](https://missing.csail.mit.edu/) — a course covering the command line, version control, and other tools that formal education often skips
- [DevBook: Programming](../../programming/index.md) — languages, paradigms, and software development practices

## ➡️ Next Steps

- [Building Confidence](building-confidence.md) — the psychological dimension of starting from zero: shame, frustration, persistence, and the dignity of continuing
- [Programming Index](../../programming/index.md) — where languages, paradigms, and the craft of building software begin
- [Touching the Keyboard](touching-the-keyboard.md) — a deeper exploration of typing, keyboard familiarity, and the physical foundation of digital work
