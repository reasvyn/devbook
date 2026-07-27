# What Is a File

## Description

Every piece of data on a computer — a document, a photo, a program, a configuration — is stored as a file. Understanding what files are, how they are named, how they are organized into directories, and how paths describe their location is foundational knowledge that every subsequent computing skill depends on. This document explains files from the ground up.

## Prerequisites

- [Using a Computer](using-a-computer.md) — navigating the desktop, file manager, and basic GUI operations

## Table of Contents

- [What Is a File?](#what-is-a-file)
- [File Content and Size](#file-content-and-size)
- [File Names and Extensions](#file-names-and-extensions)
- [Hidden Files](#hidden-files)
- [What Is a Directory?](#what-is-a-directory)
- [Paths: Describing Locations](#paths-describing-locations)
- [Absolute vs Relative Paths](#absolute-vs-relative-paths)
- [File Systems](#file-systems)
- [File Metadata](#file-metadata)
- [Creating, Copying, Moving, and Deleting Files](#creating-copying-moving-and-deleting-files)
- [How the Terminal Represents Files](#how-the-terminal-represents-files)
- [Common Mistakes](#common-mistakes)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a File?

A file is a named container for data stored on a storage device (hard drive, SSD, USB stick). The operating system manages files — it keeps track of where each file's data is physically stored, what it is called, and who is allowed to access it.

Files come in two fundamental categories:

| Category | Description | Examples |
|----------|-------------|----------|
| **Text files** | Contain human-readable characters encoded as bytes (UTF-8, ASCII) | `.txt`, `.md`, `.html`, `.py`, `.csv` |
| **Binary files** | Contain data in a format that is not directly human-readable | `.jpg`, `.mp3`, `.pdf`, `.exe`, `.zip` |

A text file opened in a text editor shows readable words. A binary file opened in a text editor shows garbled characters. Both are ultimately sequences of bytes on disk — the difference is how those bytes are interpreted.

```
Text file (notes.txt):
  Bytes: 72 101 108 108 111 44 32 119 111 114 108 100 33
  Text:  H  e  l  l  o  ,     w  o  r  l  d  !

Binary file (photo.jpg):
  Bytes: 255 216 255 224 0 16 74 70 73 70 0 1 1 0 0 1 ...
  (not meaningful as text — must be interpreted by an image viewer)
```

### File Content and Size

A file's **content** is the data it holds. The **size** of a file is the number of bytes it occupies on disk.

| Size | What it holds approximately |
|------|----------------------------|
| 1 byte | A single character (e.g., `A`) |
| 1 kilobyte (KB) = 1,024 bytes | A short email or a paragraph of text |
| 1 megabyte (MB) = 1,024 KB | A 3-minute MP3 song, or a high-resolution photo |
| 1 gigabyte (GB) = 1,024 MB | An HD movie, or the contents of a small bookshelf |
| 1 terabyte (TB) = 1,024 GB | ~250,000 photos, or the entire works of Shakespeare 50,000 times |

Your operating system reports file sizes in these units. When you right-click a file and choose **Properties** (Windows) or **Get Info** (macOS), you see its size — for example, `4.2 MB`.

**Why does size matter?** Large files take longer to open, copy, and send over the internet. A 5 KB text email sends instantly; a 5 GB video file might take minutes. Understanding size helps you make practical decisions about storage and sharing.

### File Names and Extensions

Every file has a **name** chosen by whoever created it. A file name consists of two parts:

```
    filename    .    extension
    ────────         ─────────
    "report"         "pdf"
```

The **extension** (the part after the dot) tells the operating system what kind of file it is and which application should open it.

| Extension | File type | Default application |
|-----------|-----------|-------------------|
| `.txt` | Plain text | Notepad, TextEdit, gedit |
| `.md` | Markdown (formatted text) | Any text editor, GitHub |
| `.docx` | Microsoft Word document | Microsoft Word, LibreOffice Writer |
| `.pdf` | Portable Document Format | Adobe Acrobat, browser |
| `.jpg` / `.jpeg` | JPEG image | Photos app, GIMP |
| `.png` | PNG image (supports transparency) | Photos app, GIMP |
| `.mp3` | Audio (compressed) | Music player, VLC |
| `.mp4` | Video (compressed) | Video player, VLC |
| `.html` | Web page | Web browser |
| `.py` | Python program | Python interpreter, text editor |
| `.js` | JavaScript program | Node.js, browser |
| `.zip` | Compressed archive | Archive utility, 7-Zip |
| `.xlsx` | Excel spreadsheet | Microsoft Excel, LibreOffice Calc |

**Naming rules and conventions:**

| Rule | Example | Why |
|------|---------|-----|
| No `/` or `\` in file names | `my-file.txt` ✅ | These characters are reserved for path separators |
| Case sensitivity matters (Linux) | `File.txt` ≠ `file.txt` on Linux | Linux treats them as different files; Windows and macOS do not |
| Use lowercase with hyphens | `my-report-final.md` ✅ | `My Report Final.md` works but is harder to type and reference |
| Avoid spaces when possible | `my-report.md` ✅ | Spaces work but cause problems in scripts and terminals |
| Keep names descriptive | `budget-2026-january.xlsx` ✅ | `a.xlsx` is legal but unhelpful |

### Hidden Files

Some files are **hidden** — they do not appear in the file manager by default. These are usually configuration files used by the operating system or applications.

| OS | How to show hidden files | Naming convention |
|----|------------------------|-------------------|
| Windows | View → show hidden items (File Explorer) | Files with `Hidden` attribute |
| macOS | `Cmd + Shift + .` in Finder | Names starting with `.` |
| Linux | `Ctrl + H` in file manager, or `ls -a` in terminal | Names starting with `.` |

Examples of hidden files:

```
.bashrc          ← shell configuration (Linux/macOS)
.gitconfig       ← git configuration
.config/         ← application settings folder
.env             ← environment variables (never share this!)
```

**Important:** hidden files are not secret — they are just hidden to reduce clutter. You can view and edit them like any other file.

### What Is a Directory?

A **directory** (also called a **folder**) is a container that holds files and other directories. Directories form a **tree structure** — a hierarchy where every item has a parent.

```
📁 / (root directory)
├── 📁 home
│   └── 📁 alice
│       ├── 📁 Documents
│       │   ├── 📄 report.pdf
│       │   └── 📁 School
│       │       └── 📄 essay.docx
│       ├── 📁 Downloads
│       │   └── 📄 photo.jpg
│       └── 📄 notes.txt
├── 📁 etc
│   └── 📄 config.conf
└── 📁 tmp
```

Key concepts:

| Term | Meaning |
|------|---------|
| **Root directory** | The top of the tree, represented as `/` (on Linux/macOS) or `C:\` (on Windows) |
| **Parent directory** | The directory one level up |
| **Subdirectory** | A directory nested inside another |
| **Current directory** | The directory you are currently "in" or viewing |
| **Home directory** | Your personal directory: `/home/alice` (Linux), `/Users/alice` (macOS), `C:\Users\alice` (Windows) |

**The tilde `~`** is shorthand for your home directory. In a terminal, `cd ~` means "go to my home directory."

### Paths: Describing Locations

A **path** describes where a file or directory is located in the tree. It is a string of directory names separated by a path separator, ending with the file or directory name.

**Path separators differ by operating system:**

| OS | Separator | Example |
|----|-----------|---------|
| Linux | `/` (forward slash) | `/home/alice/report.pdf` |
| macOS | `/` (forward slash) | `/Users/alice/report.pdf` |
| Windows | `\` (backslash) | `C:\Users\alice\report.pdf` |

> Note: modern Windows applications often accept `/` as well, but `\` is the traditional convention.

**Anatomy of a path:**

```
Linux:   /home/alice/Documents/report.pdf
         ─  ────────  ─────────  ──────────
         root  home    Documents   report.pdf
               dir     dir         file

Windows: C:\Users\alice\Documents\report.pdf
         ─  ─────  ─────  ─────────  ──────────
         drive  Users  alice Documents  report.pdf
```

Each segment represents one level of the tree. The path tells the operating system: start at the root, go into `home`, then into `alice`, then into `Documents`, and you will find `report.pdf`.

### Absolute vs Relative Paths

There are two ways to describe a file's location:

**Absolute path** — starts from the root `/` and gives the complete location:

```bash
/home/alice/Documents/report.pdf     # Linux/macOS
C:\Users\alice\Documents\report.pdf  # Windows
```

An absolute path always works, no matter where you are in the file system.

**Relative path** — starts from your **current directory** and describes the location relative to where you are:

```bash
Documents/report.pdf          # if you are in /home/alice
../Downloads/photo.jpg        # go up one level, then into Downloads
School/essay.docx             # if you are in /home/alice/Documents
```

**Special path components:**

| Symbol | Meaning | Example |
|--------|---------|---------|
| `.` | Current directory | `./file.txt` (same as `file.txt`) |
| `..` | Parent directory | `cd ..` (go up one level) |
| `~` | Home directory | `cd ~` (go to home) |
| `/` | Root directory (or path separator) | `/home/alice` (absolute path) |

**Practice — relative path exercise:**

Assume you are currently in `/home/alice/Documents`. How do you reach these files?

```
Target: /home/alice/Documents/School/essay.docx
Answer: School/essay.docx

Target: /home/alice/Downloads/photo.jpg
Answer: ../Downloads/photo.jpg

Target: /home/alice/notes.txt
Answer: ../notes.txt

Target: /tmp/temp.txt
Answer: /tmp/temp.txt    (this needs an absolute path — you cannot reach /tmp from /home with relative paths)
```

### File Systems

A **file system** is the method the operating system uses to organize and keep track of files on a storage device. It is the invisible structure that makes files possible.

| File system | Used by | Key characteristics |
|------------|---------|-------------------|
| **NTFS** | Windows | Default for Windows, supports permissions, encryption, large files |
| **APFS** | macOS | Default for modern Macs, optimized for SSDs, snapshots |
| **ext4** | Linux | Default for most Linux distributions, robust and fast |
| **FAT32** | USB drives, SD cards | Compatible with everything, but file size limited to 4 GB |
| **exFAT** | USB drives, SD cards | Like FAT32 but supports files larger than 4 GB |

When you **format** a storage device, you choose a file system. Formatting erases all existing data and prepares the device for a new file system.

**What the file system tracks:**

For each file, the file system maintains a record (called an **inode** on Linux/macOS or a **file entry** on Windows) containing:

| Metadata field | Description |
|---------------|-------------|
| File name | The human-readable name |
| Size | Number of bytes |
| Location | Which disk blocks contain the data |
| Permissions | Who can read, write, or execute |
| Timestamps | Created, modified, last accessed |
| Owner | Which user account owns the file |

### File Metadata

**Metadata** is data about data. A file's metadata describes the file without being the file's content.

**How to view metadata:**

- **GUI**: Right-click the file → **Properties** (Windows) or **Get Info** (macOS)
- **Terminal**: `ls -l filename` (Linux/macOS)

```bash
$ ls -l report.pdf
-rw-r--r-- 1 alice staff 204847 Jun 15 14:30 report.pdf
```

Breaking down the output:

| Part | Meaning |
|------|---------|
| `-rw-r--r--` | Permissions: owner can read/write, group can read, others can read |
| `1` | Number of hard links |
| `alice` | File owner |
| `staff` | Group owner |
| `204847` | File size in bytes (~200 KB) |
| `Jun 15 14:30` | Last modified date and time |
| `report.pdf` | File name |

### Creating, Copying, Moving, and Deleting Files

**Creating a new file:**

- **GUI**: Right-click in the file manager → New → Text Document (or New → Folder)
- **Terminal**: `touch filename.txt` creates an empty file; `echo "content" > filename.txt` creates a file with content

**Copying a file** creates an identical duplicate at a new location. The original remains unchanged.

```bash
$ cp report.pdf backup-report.pdf       # copy in the same directory
$ cp report.pdf ~/Documents/            # copy to Documents folder
$ cp -r Projects/ Projects-backup/      # copy a directory (recursive)
```

**Moving a file** changes its location. The file disappears from the old location and appears at the new one. On the same disk, moving is instant (only the path changes). Moving to a different disk copies then deletes.

```bash
$ mv report.pdf ~/Documents/            # move to Documents
$ mv old-name.txt new-name.txt          # rename a file
```

**Deleting a file** removes it from the file system.

```bash
$ rm filename.txt          # delete a file (Linux/macOS)
$ rm -r directory/         # delete a directory and its contents
```

On Windows, deleted files go to the **Recycle Bin**. On macOS, they go to the **Trash**. Both allow recovery. In the terminal, `rm` does **not** use a trash bin — the file is gone immediately. Be careful.

### How the Terminal Represents Files

In the terminal, everything is a file. The terminal itself represents files as text paths. Understanding this is critical:

```bash
$ pwd                      # where am I?
/home/alice

$ ls                       # what files are here?
Documents  Downloads  notes.txt  Projects

$ ls Documents/            # what is inside Documents?
report.pdf  School/

$ file notes.txt           # what type of file is this?
notes.txt: ASCII text

$ file photo.jpg
photo.jpg: JPEG image data, JFIF standard 1.01

$ wc -l notes.txt         # how many lines in this file?
42 notes.txt

$ stat notes.txt          # full metadata
  File: notes.txt
  Size: 1247       Blocks: 8          IO Block: 4096   regular file
  Access: (0644/-rw-r--r--)  Uid: ( 1000/   alice)   Gid: ( 1000/   alice)
  Modify: 2026-06-15 14:30:00.000000000 +0000
```

The `file` command is particularly useful — it tells you what kind of file something actually is, regardless of its extension. A file named `data.txt` might actually be a JPEG image that was mislabeled.

### Common Mistakes

| Mistake | What goes wrong | How to avoid it |
|---------|----------------|-----------------|
| No extension on a file | OS does not know which application to open it with | Always include the extension |
| Wrong extension | File opens in wrong application | Use the correct extension for the content type |
| Confusing `/` and `\` | Path does not work | Use `/` on Linux/macOS, `\` on Windows |
| Confusing relative and absolute | File not found error | When in doubt, use the absolute path |
| Spaces in filenames | Breaks terminal commands and scripts | Use hyphens or underscores instead |
| Deleting with `rm` without checking | File is permanently gone | Always `ls` before `rm`; use `rm -i` to confirm |
| Moving a file to the wrong location | Cannot find it later | Verify the destination path before moving |

### Learning Tips

- **Use the file manager before the terminal.** Visual navigation builds intuition about directory trees. Once you understand the structure, the terminal becomes much easier.
- **Practice path navigation.** Open a terminal, use `pwd` to see where you are, `ls` to see what is there, and `cd` to move around. Try reaching a specific file using both absolute and relative paths.
- **Learn the `tree` command.** On Linux, `tree` shows the directory structure as a visual tree — it makes the hierarchy concrete. On macOS, install it with `brew install tree`.
- **Never guess a path.** If you are unsure where a file is, use `find / -name "filename"` (Linux) or the file manager's search. Guessing leads to confusion.
- **Think in trees.** Every time you navigate a file system, imagine the tree structure. Where is the root? Where are you now? What is above you? What is below you?

## Glossary

| Term | Definition |
|------|------------|
| Absolute path | A path starting from the root directory, describing the full location of a file |
| Binary file | A file containing data in a non-text format (images, audio, executables) |
| Directory | A container for files and other directories (synonym: folder) |
| Extension | The suffix after a dot in a filename indicating the file type |
| File | A named container for data stored permanently on a storage device |
| File system | The method an OS uses to organize and track files on a storage device |
| File size | The number of bytes a file occupies on disk |
| Home directory | The personal directory for a user account |
| Inode | A data structure on Unix file systems storing file metadata |
| Metadata | Data about data — a file's name, size, permissions, and timestamps |
| Path | A string describing the location of a file in the directory tree |
| Path separator | The character dividing directory names in a path (`/` on Unix, `\` on Windows) |
| Relative path | A path starting from the current directory |
| Root directory | The top of the file system tree (`/` on Unix, `C:\` on Windows) |
| Text file | A file containing human-readable characters |
| Tilde (`~`) | Shell shorthand for the current user's home directory |

## Quick References

- [Linux File System Hierarchy](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/) — official guide to the Linux directory structure
- [HowStuffWorks — File Systems](https://computer.howstuffworks.com/file-system.htm) — beginner-friendly explanation of how file systems work
- [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/) — documentation for `cp`, `mv`, `rm`, `ls`, and other file commands

## Next Steps

- [Command Line Basics](command-line-basics.md) — navigate the file system and manipulate files through the terminal
- [How Computers Work](how-computers-work.md) — understand what happens inside the machine when you save or read a file
- Back to [Computer Science Introduction](../intro/index.md)
