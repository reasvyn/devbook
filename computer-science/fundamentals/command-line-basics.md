# Command Line Basics

## Description

A practical hands-on guide to using the command line — the most direct way to control a computer. Every developer needs terminal skills to navigate files, run programs, inspect processes, and automate tasks. No prior experience required.

## Prerequisites

- [How Computers Work](how-computers-work.md) — understand what a CPU, memory, and storage are

## Table of Contents

- [What Is the Terminal?](#what-is-the-terminal)
- [Opening the Terminal](#opening-the-terminal)
- [Your First Commands](#your-first-commands)
- [Navigating the Filesystem](#navigating-the-filesystem)
- [Working with Files and Directories](#working-with-files-and-directories)
- [Understanding Command Structure](#understanding-command-structure)
- [Getting Help](#getting-help)
- [File Permissions](#file-permissions)
- [Pipes and Redirection](#pipes-and-redirection)
- [Searching with grep](#searching-with-grep)
- [Managing Processes](#managing-processes)
- [Editing Files with Nano](#editing-files-with-nano)
- [Writing a Shell Script](#writing-a-shell-script)
- [Full Walkthrough Session](#full-walkthrough-session)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is the Terminal?

The terminal is a text-based interface to your operating system. Instead of clicking buttons with a mouse, you type commands and the computer responds with text output. Behind the terminal is a **shell** — a program that interprets your commands and tells the operating system what to do.

The shell is a REPL (Read-Eval-Print-Loop):

```
You type a command  →  Shell reads it
                     →  Shell evaluates it (runs the program)
                     →  Shell prints the output
You see the result   ←  Shell returns to prompt
```

The most common shell on Linux and macOS is **Bash** (Bourne Again SHell). On Windows you typically use PowerShell, or you can install WSL (Windows Subsystem for Linux) to get a Bash-compatible environment. All examples in this document use Bash with a `$` prompt.

A terminal window looks like this when you first open it:

```
user@machine:~$
```

This is called the **prompt**. It tells you:

- `user` — your username
- `machine` — the computer's hostname
- `~` — your current directory (`~` is shorthand for your home directory)
- `$` — the prompt symbol (waiting for your command)

### Opening the Terminal

**Linux**: Press `Ctrl + Alt + T` or search for "Terminal" in the application menu.

**macOS**: Open `Finder → Applications → Utilities → Terminal`. Or press `Cmd + Space`, type "Terminal", and press Enter.

**Windows (WSL)**: Install WSL first:

```
wsl --install
```

Then open the Start Menu and search for "Ubuntu" (or the distro you installed). This gives you a Bash shell on Windows.

### Your First Commands

```bash
$ echo "Hello, world!"
Hello, world!

$ whoami
alice

$ date
Wed Jul  1 14:23:45 UTC 2026
```

`echo` prints its arguments. `whoami` prints your username. `date` prints the current time.

Every command is a program stored on your computer. The shell finds it by searching directories in the `PATH` variable:

```bash
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

$ which echo
/usr/bin/echo
```

### Navigating the Filesystem

Your entire filesystem is a tree starting from the **root directory**, denoted by `/`. Your **home directory** (`/home/alice` on Linux, `/Users/alice` on macOS) is your personal workspace.

**pwd — Print Working Directory**

```bash
$ pwd
/home/alice
```

**ls — List Directory Contents**

```bash
$ ls
Desktop  Documents  Downloads  Music  Pictures  Projects

$ ls -l
total 32
drwxr-xr-x  2 alice alice 4096 Jun 15 10:00 Desktop
drwxr-xr-x  5 alice alice 4096 Jul  1 09:12 Documents
drwxr-xr-x  3 alice alice 4096 Jun 28 16:45 Downloads

$ ls -la
total 128
drwxr-x--- 18 alice alice 4096 Jul  1 14:00 .
drwxr-xr-x  3 root  root  4096 Jan  1  2025 ..
-rw-------  1 alice alice  220 Jan  1  2025 .bash_logout
-rw-r--r--  1 alice alice 3771 Jan  1  2025 .bashrc
```

- `-l` means "long format" — shows permissions, owner, size, and date
- `-a` shows hidden files (those starting with a dot)
- Notice `.` (current directory) and `..` (parent directory). Hidden files like `.bashrc` store shell configuration.

**cd — Change Directory**

```bash
$ cd Documents
$ pwd
/home/alice/Documents

$ cd ..
$ pwd
/home/alice

$ cd           # with no arguments, go to home directory
$ pwd
/home/alice
```

**Absolute vs Relative Paths**

An **absolute path** starts from the root `/` and specifies the full location:

```bash
$ cd /home/alice/Documents/project
```

A **relative path** starts from your current directory:

```bash
$ cd Documents/project    # from /home/alice
$ cd ../Downloads         # go up one level, then into Downloads
```

Special path components:
- `.` — current directory
- `..` — parent directory
- `~` — home directory
- `-` — previous directory (`cd -` toggles between two directories)

```
Filesystem Tree: / → home/ → alice/ → Documents/ → project/
                                    → .bashrc
                    → bin/  → ls, echo, cp
                    → tmp/
```

### Working with Files and Directories

**mkdir** — `mkdir new-project` creates a directory. `mkdir -p a/b/c` creates parents too.

**touch** — `touch README.md` creates an empty file or updates its timestamp.

**cp** — `cp a b` copies file. `cp -r dir1 dir2` copies directories recursively.

**mv** — `mv old new` renames or moves a file. Same operation either way.

**rm** — `rm file` removes a file. `rm -r dir` removes a directory. `rm -rf` forces removal without confirmation. **There is no trash bin** — always `ls` first.

**cat** — `cat file` prints the entire file to the terminal. For long files use `less`.

**less** — `less file` opens a pager. `Space` = next page, `b` = previous, `/pattern` = search, `q` = quit.

**head and tail** — `head -n 5 file` shows first 5 lines. `tail -n 20 file` shows last 20. `tail -f file` follows new lines in real time.

### Understanding Command Structure

Most commands follow this pattern:

```
command  [options]  [arguments]
```

- **command** — the program to run (e.g., `ls`, `cp`, `grep`)
- **options** — modify behavior, usually start with `-` or `--`
- **arguments** — what the command operates on

**Short options** (single dash, single letter) can be combined:

```bash
$ ls -la          # same as ls -l -a
$ rm -rf junk/    # same as rm -r -f junk/
```

**Long options** (double dash, full word):

```bash
$ ls --all
$ grep --ignore-case "hello" file.txt
```

The `--` double dash signals the end of options — everything after it is treated as an argument:

```bash
$ cat -- -filename-with-dashes.txt
```

### Getting Help

Every command supports `--help`:

```bash
$ ls --help
Usage: ls [OPTION]... [FILE]...
```

**man** — Full manual pages, navigated with `less` keybindings:

```bash
$ man ls
```

Sections: 1 (user commands), 2 (system calls), 3 (library functions), 5 (file formats), 8 (admin).

**apropos** — Search man page descriptions:

```bash
$ apropos "list directory"
ls (1)               - list directory contents
```

### File Permissions

Every file and directory has permissions controlling who can read, write, and execute it.

```bash
$ ls -l
-rw-r--r--  1 alice alice  3771 Jan  1  2025 .bashrc
```

The first column shows permissions in three groups of three (owner, group, others):

```
-rw-r--r--
│└───────── file type: d=directory, -=file
└────────── owner: rw-, group: r--, others: r--
```

Three groups: **owner**, **group**, **others**.

For files: `r` = read, `w` = write, `x` = execute. For directories: `r` = list, `w` = create/delete, `x` = enter.

**chmod** — Symbolic: `chmod u+x script.sh` (add execute for owner), `chmod g-w file` (remove write for group).

Octal: `chmod 755 file` gives `rwxr-xr-x`, `chmod 644` gives `rw-r--r--`, `chmod 600` gives `rw-------`.

| Octal | Permission |
|-------|------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |

**chown — Change Owner**

```bash
$ sudo chown bob file.txt              # change owner
$ sudo chown bob:developers file.txt   # change owner and group
$ sudo chown -R alice:alice /home/alice/project/   # recursive
```

### Pipes and Redirection

The shell can connect programs together. This is the Unix philosophy: small tools that do one thing well, combined to perform complex tasks.

**Output Redirection** — `>` sends output to a file (overwrites), `>>` appends:

```bash
$ echo "Hello" > greeting.txt
$ echo "World" >> greeting.txt
$ cat greeting.txt
Hello
World
```

**Input Redirection** — `<` reads input from a file:

```bash
$ sort < names.txt
```

**Pipes** — `|` sends one command's output as another's input:

```bash
$ ls /usr/bin | grep zip | head -5
bunzip2
bzcat
bzip2
bzip2recover
funzip
```

Step by step: `ls /usr/bin` lists files, `grep zip` filters for "zip", `head -5` shows first 5.

Count matching files:

```bash
$ grep -l "error" /var/log/*.log | wc -l
```

Largest files:

```bash
$ du -sh /home/alice/* | sort -rh | head -5
2.1G    /home/alice/Downloads
456M    /home/alice/Projects
128M    /home/alice/.local
12M     /home/alice/Documents
```

**Standard Streams**

Every program has three streams: stdin (0, input), stdout (1, normal output), stderr (2, errors). Redirect stderr with `2>`:

```bash
$ ls /nonexistent 2> error.log   # errors to file
$ ls /nonexistent 2>&1           # merge stderr into stdout
$ ls /nonexistent 2>/dev/null    # discard errors
```

`/dev/null` is the filesystem's black hole — everything written to it disappears.

### Searching with grep

`grep` searches files for lines matching a pattern.

```bash
$ grep "error" log.txt
[2026-07-01 12:00:00] ERROR: connection timeout
```

| Option | Meaning |
|--------|---------|
| `-i` | Case-insensitive |
| `-v` | Invert match |
| `-r` | Recursive |
| `-n` | Show line numbers |
| `-l` | List filenames only |

Examples:

```bash
$ grep -i "warning" log.txt
$ grep -rn "TODO" src/
$ grep -v "^#" config.ini
$ ps aux | grep -i python
```

**Regular Expressions**

```bash
$ grep "^Error" log.txt                # lines starting with "Error"
$ grep "failed$" log.txt               # lines ending with "failed"
$ grep -E "error|warning|fatal" log     # match any of three patterns
```

**Recursive Project Search**

```bash
$ grep -rn "class User" app/
app/models/user.py:10:class User(BaseModel):
app/models/user.py:45:    def get_user(self):
app/views/profile.py:23:from app.models.user import User
```

```bash
$ grep -rl "deprecated_function" src/
src/old_parser.py
src/legacy_api.py
```

### Managing Processes

Every running program is a **process** with a unique **PID** (Process ID).

**ps** — `ps` shows your processes. `ps aux` shows every process on the system:

```bash
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS STAT START   TIME COMMAND
root         1  0.0  0.5 169624 11132 Ss   Jun30   0:03 /sbin/init
alice     1234  0.0  0.1  22864  4528 Ss   14:00   0:00 bash
alice     4590  0.5  2.3 890432 47892 Sl   14:30   0:45 /usr/bin/firefox
```

Columns: `USER`, `PID`, `%CPU`, `%MEM`, `VSZ` (virtual memory), `RSS` (physical memory), `STAT` (state), `COMMAND`.

**top — Interactive Process Viewer**

```
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
 4590 alice     20   0 890432 47892 23148 S   5.2   2.3   0:45.23 firefox
 3201 alice     20   0 567232 28912 15234 S   2.1   1.4   0:12.56 code
```

Press `q` to quit, `k` to kill, `M` to sort by memory, `P` to sort by CPU.

**kill — Send Signals**

```bash
$ kill 1234            # SIGTERM (15) — graceful shutdown
$ kill -9 1234         # SIGKILL — force termination immediately
```

Common signals:

| Signal | Number | Purpose |
|--------|--------|---------|
| SIGHUP | 1 | Reload configuration |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGTERM | 15 | Graceful shutdown |
| SIGKILL | 9 | Force kill (cannot be caught) |

Kill by name:

```bash
$ pkill my-program              # send SIGTERM by name
$ pkill -f "node server.js"     # -f matches full command line
```

**Background and Foreground**

```bash
$ sleep 100 &
[1] 12345

$ jobs          # list background jobs
$ fg            # bring job to foreground
$ bg            # continue stopped job in background
```

Suspend the current foreground job with `Ctrl+Z`.

### Editing Files with Nano

Nano is a simple terminal-based text editor installed on most Linux systems.

```bash
$ nano notes.txt
```

| Key | Action |
|-----|--------|
| `Ctrl + X` | Exit (prompts to save) |
| `Ctrl + O` | Save (Write Out) |
| `Ctrl + W` | Search |
| `Ctrl + K` | Cut line |
| `Ctrl + U` | Paste |
| `Ctrl + G` | Show help |

```bash
$ nano hello.txt
```

Type text, press `Ctrl + O` then Enter to save, `Ctrl + X` to exit.

```bash
$ cat hello.txt
Hello, world!
This is my first file in nano.
```

### Writing a Shell Script

A shell script is a text file containing commands the shell executes sequentially.

**Your First Script**

```bash
$ nano hello.sh
```

```bash
#!/bin/bash
echo "Hello, world!"
echo "Today is $(date)"
echo "You are logged in as $(whoami)"
```

The first line `#!/bin/bash` is the **shebang** — it tells the system which interpreter to use.

Make it executable and run:

```bash
$ chmod +x hello.sh
$ ./hello.sh
Hello, world!
Today is Wed Jul  1 14:40:00 UTC 2026
You are logged in as alice
```

**Variables**

```bash
#!/bin/bash

NAME="Alice"
GREETING="Hello, $NAME!"
echo $GREETING
```

**Positional Arguments**

```bash
#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

```bash
$ ./args.sh hello world 42
Script name: ./args.sh
First argument: hello
All arguments: hello world 42
Number of arguments: 3
```

**Exit Codes**

Every command returns an exit code — 0 for success, non-zero for failure:

```bash
$ ls /exists
$ echo $?
0

$ ls /does-not-exist
$ echo $?
2
```

**Conditionals and Loops**

```bash
#!/bin/bash
FILE="data.txt"

if [ -f "$FILE" ]; then
    echo "$FILE exists."
else
    echo "$FILE does not exist."
    exit 1
fi

for i in {1..5}; do
    echo "Count: $i"
done

for FILE in *.txt; do
    cp "$FILE" "$FILE.bak"
done
```

**A Practical Backup Script**

```bash
#!/bin/bash
# backup.sh — simple backup

SOURCE="$1"
DEST="$2"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)

if [ -z "$SOURCE" ] || [ -z "$DEST" ]; then
    echo "Usage: $0 <source_directory> <destination_directory>"
    exit 1
fi

if [ ! -d "$SOURCE" ]; then
    echo "Error: source '$SOURCE' is not a directory."
    exit 1
fi

BACKUP_DIR="$DEST/backup_$TIMESTAMP"
mkdir -p "$BACKUP_DIR"
cp -r "$SOURCE" "$BACKUP_DIR"
echo "Backup created: $BACKUP_DIR"
```

### Full Walkthrough Session

Try this step-by-step session that demonstrates everything in this document:

```
$ pwd
/home/alice

$ mkdir -p workshop/project/src
$ cd workshop/
$ touch README.md
$ echo "# Workshop Project" > README.md
$ cat README.md
# Workshop Project

$ cp README.md README.md.bak
$ mkdir docs
$ mv README.md.bak docs/

$ cd project/src/
$ touch main.py utils.py config.py
$ echo "print('hello')" > main.py
$ cd ../..

$ grep -rn "print" .
./project/src/main.py:1:print('hello')

$ chmod +x project/src/main.py
$ ./project/src/main.py
hello

$ echo "It works!" > output.txt
$ echo "Second line" >> output.txt
$ cat output.txt | wc -l
2

$ nano project/src/config.py
```

Inside nano, add content, save with `Ctrl+O`, exit with `Ctrl+X`.

```
$ cat project/src/config.py
# Configuration file
DEBUG = True
PORT = 8080
DATABASE_URL = "sqlite:///app.db"

$ cat > scripts/deploy.sh << 'EOF'
#!/bin/bash
PROJECT_DIR="/home/alice/workshop/project"
echo "Deploying project from $PROJECT_DIR..."
cp -r "$PROJECT_DIR" /tmp/deploy
echo "Deployment complete."
EOF

$ chmod +x scripts/deploy.sh
$ ./scripts/deploy.sh
Deployment complete.

$ find . -name "*.py"
./project/src/main.py
./project/src/utils.py
./project/src/config.py

$ rm output.txt docs/README.md.bak
$ rm -r scripts/
$ rmdir docs/

$ ls
project  README.md

$ cd
$ rm -r workshop/
```

Every action in this session can be written into a script and replayed identically on any machine.

## Learning Tips

- **Type, don't paste.** Typing builds muscle memory. Resist copy-pasting during your first sessions.
- **Use Tab completion.** Press `Tab` to auto-complete filenames. Press twice to see all matches.
- **Read error messages.** When a command fails, the error tells you exactly what's wrong.
- **Use history.** Press `Up` for previous commands. Press `Ctrl + R` to search history.
- **Break pipes one at a time.** When debugging a pipeline, run each stage separately to verify.
- **Be careful with rm -rf.** Always `ls` before you `rm`. Consider `alias rm='rm -i'` while learning.
- **Keep a cheat sheet.** After a week of daily use, it becomes second nature.

## Glossary

| Term | Definition |
|------|------------|
| Absolute path | A path starting from root `/`, e.g., `/home/alice/file.txt` |
| Argument | A value passed to a command (filename, directory, text) |
| Bash | Bourne Again SHell — the most common Unix shell |
| chmod | Command to change file permissions |
| chown | Command to change file ownership |
| Command | A program executed by the shell |
| echo | Command that prints its arguments to the terminal |
| grep | Command that searches text for patterns |
| kill | Command that sends signals to processes |
| less | Pager program for viewing files page by page |
| man | Manual page system — command documentation |
| nano | Simple terminal-based text editor |
| PATH | Environment variable listing directories searched for commands |
| PID | Process ID — unique number for each running process |
| Pipe | `\|` operator connecting one command's output to another's input |
| Prompt | Text (ending with `$`) indicating the shell is ready for input |
| pwd | Print Working Directory — shows current location |
| Relative path | A path relative to the current directory |
| Shebang | `#!` at the top of a script indicating the interpreter |
| Signal | Notification sent to a process (SIGTERM, SIGKILL, etc.) |
| Standard error (stderr) | Output stream for error messages (fd 2) |
| Standard input (stdin) | Input stream (fd 0) |
| Standard output (stdout) | Normal output stream (fd 1) |
| sudo | Command to run another command as superuser |
| Terminal | Text-based interface to the shell and OS |
| WSL | Windows Subsystem for Linux — Linux environment on Windows |

## Quick References

- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/) — official Bash reference
- [explainshell.com](https://explainshell.com/) — paste a command to see what each part does
- [tldr.sh](https://tldr.sh/) — simplified man pages with practical examples
- [RegexOne](https://regexone.com/) — interactive regular expressions tutorial
- [ShellCheck](https://www.shellcheck.net/) — lint shell scripts for bugs

## Next Steps

- [Programming Fundamentals](../../programming/fundamentals/intro/programming-fundamentals.md) — learn core programming concepts
- [Operating Systems](../operating-systems/intro/operating-systems-intro.md) — understand what the OS does behind your terminal
- Back to [Computer Science Introduction](../intro/index.md)
