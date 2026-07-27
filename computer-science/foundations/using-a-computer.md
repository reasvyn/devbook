# Using a Computer

## Description

Before writing code, before opening a terminal, before anything else — you need to know how to operate the machine itself. This document starts from absolute zero: turning the computer on, understanding the desktop, using a mouse and keyboard, navigating windows, managing files through a graphical interface, and browsing the web. No prior experience assumed.

## Prerequisites

- None. This is the very first step.

## Table of Contents

- [What Is a Computer?](#what-is-a-computer)
- [Turning It On](#turning-it-on)
- [The Desktop](#the-desktop)
- [The Mouse and Trackpad](#the-mouse-and-trackpad)
- [The Keyboard](#the-keyboard)
- [Windows and Applications](#windows-and-applications)
- [Files and Folders](#files-and-folders)
- [Saving and Opening Files](#saving-and-opening-files)
- [The Web Browser](#the-web-browser)
- [Connecting to the Internet](#connecting-to-the-internet)
- [What to Do When Something Goes Wrong](#what-to-do-when-something-goes-wrong)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Computer?

A computer is an electronic device that takes input (what you type, click, or say), processes it, and produces output (text on screen, sound from speakers, files saved to disk). You are about to learn how to command one.

There are three main types of personal computers you might encounter:

| Type | Description | Examples |
|------|-------------|----------|
| **Desktop** | A stationary computer with a separate monitor, keyboard, and mouse | Office workstations, gaming PCs |
| **Laptop** | A portable computer with built-in screen, keyboard, and trackpad | MacBook, ThinkPad, Chromebook |
| **Tablet/Phone** | Touchscreen devices running mobile operating systems | iPad, Android tablet |

This document focuses on desktops and laptops, as they are the primary tools for software development. The concepts apply to all three types.

### Turning It On

The **power button** is usually located:

- **Desktop**: on the front or top of the computer case (the box), or on the monitor
- **Laptop**: above the keyboard, often in the top-right corner, sometimes on the side

Press the power button once and release. Do not hold it down — a brief press is enough. The screen will light up and the computer will begin its startup sequence.

**What happens during startup:**

```
Power button pressed
       │
       ▼
┌──────────────┐
│ Hardware test │  (a few seconds — you may see a logo)
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Operating    │  (Windows, macOS, or Linux loads)
│ System loads │  (this takes 10–60 seconds)
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Login screen │  (you enter your password or PIN)
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Desktop      │  (you can now use the computer)
└──────────────┘
```

**If there is a login screen:** type your password and press the **Enter** key. If you do not know the password, ask whoever set up the computer. On some laptops, you may use a fingerprint reader or face recognition instead.

**If the screen stays black:** make sure the monitor is plugged in and turned on (check for a small power light on the monitor). On a laptop, try pressing any key or moving the mouse to wake the screen.

### The Desktop

Once you log in, you see the **desktop** — the main workspace. It contains:

```
┌──────────────────────────────────────────────────────┐
│  ┌────────────────────────────────────────────────┐  │
│  │              Menu bar / Taskbar                │  │
│  │  (clock, Wi-Fi, battery, app icons)           │  │
│  ├────────────────────────────────────────────────┤  │
│  │                                                │  │
│  │                  Desktop                       │  │
│  │                                                │  │
│  │   📄 document.txt    📁 Projects               │  │
│  │   🌐 Browser         📁 Documents              │  │
│  │                                                │  │
│  │                                                │  │
│  │                                                │  │
│  └────────────────────────────────────────────────┘  │
│  ┌────────────────────────────────────────────────┐  │
│  │              Taskbar / Dock                    │  │
│  │  (app icons for quick access)                  │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

| Element | What it is | Where to find it |
|---------|-----------|-----------------|
| **Desktop** | The main area with icons for files and folders | Center of the screen |
| **Taskbar** (Windows) | Bar at the bottom showing open apps, Start button, clock | Bottom of the screen |
| **Dock** (macOS) | Row of app icons at the bottom | Bottom of the screen |
| **Menu bar** (macOS) | Top bar showing app name, File, Edit, etc. | Top of the screen |
| **System tray** (Windows) | Small icons near the clock (Wi-Fi, volume, battery) | Bottom-right corner |

**Desktop icons** are shortcuts to files, folders, or applications. Double-click an icon to open it.

### The Mouse and Trackpad

The **mouse** controls a small arrow called the **cursor** (or pointer) on the screen. Move the mouse to move the cursor.

| Action | How to do it | What happens |
|--------|-------------|--------------|
| **Move** | Slide the mouse on the desk | Cursor follows your hand |
| **Click** | Press the left button once | Selects an item or places the text cursor |
| **Double-click** | Press the left button twice quickly | Opens a file or application |
| **Right-click** | Press the right button once | Opens a context menu (options for the item) |
| **Scroll** | Roll the wheel in the middle of the mouse | Moves the page up or down |
| **Drag** | Press and hold the left button, then move the mouse | Moves an item (icon, window, selected text) |

On a **laptop**, you use the **trackpad** (touchpad) instead of a mouse:

| Gesture | How to do it | What happens |
|---------|-------------|--------------|
| **Move cursor** | Slide one finger across the trackpad | Cursor moves |
| **Click** | Tap once with one finger | Same as mouse click |
| **Double-click/Select** | Tap twice with one finger | Opens or selects |
| **Right-click** | Tap with two fingers | Opens context menu |
| **Scroll** | Slide two fingers up or down | Scrolls the page |
| **Pinch to zoom** | Spread or pinch two fingers | Zooms in or out (in browsers, images) |

**Practice:** move the cursor to the bottom-left corner of the screen (Windows) or top-left (macOS) and click. Notice how the Start menu or Apple menu appears.

### The Keyboard

The keyboard is your primary input device for typing text and issuing commands.

**Key groups:**

```
┌─────────────────────────────────────────────────────────────┐
│  Function keys: F1 F2 F3 ... F12 (top row)                  │
├─────────────────────────────────────────────────────────────┤
│  Number row: ` 1 2 3 4 5 6 7 8 9 0 - =                     │
├──────┬──────┬──────┬──────┬──────┬──────┬──────┬────────────┤
│ Tab  │ Q W  │ E R  │ T Y  │ U I  │ O P  │ [ ]  │  \ |       │
├──────┤──────┼──────┼──────┼──────┼──────┼──────┤            │
│ Caps │ A S  │ D F  │ G H  │ J K  │ L ;  │ ' "  │  Enter     │
├──────┤──────┼──────┼──────┼──────┼──────┼──────┤            │
│Shift │ Z X  │ C V  │ B N  │ M ,  │ . /  │Shift │            │
├──────┴──────┴──────┴──────┴──────┴──────┴──────┤            │
│ Ctrl  │ Alt │    Spacebar              │ Alt │ Ctrl │       │
└───────┴─────┴──────────────────────────┴─────┴──────┘
```

**Important keys:**

| Key | Name | What it does |
|-----|------|-------------|
| `Enter` | Return / Enter | Confirms a command, moves to the next line, or submits a form |
| `Backspace` | Backspace | Deletes the character to the left of the cursor |
| `Delete` | Delete | Deletes the character to the right of the cursor |
| `Tab` | Tab | Moves to the next input field, or inserts indentation |
| `Space` | Spacebar | Inserts a space |
| `Esc` | Escape | Cancels the current action or closes a menu |
| `Shift` | Shift | Types uppercase letters or the symbol on a key (e.g., Shift+1 = `!`) |
| `Ctrl` / `Cmd` | Control / Command | Used with other keys for shortcuts (see below) |
| `Alt` / `Option` | Alternate | Used with other keys for shortcuts |

**Essential keyboard shortcuts:**

| Shortcut | Windows | macOS | What it does |
|----------|---------|-------|-------------|
| Copy | `Ctrl + C` | `Cmd + C` | Copies selected text or file to clipboard |
| Paste | `Ctrl + V` | `Cmd + V` | Pastes from clipboard |
| Cut | `Ctrl + X` | `Cmd + X` | Cuts (copies and removes) selected text |
| Undo | `Ctrl + Z` | `Cmd + Z` | Reverses the last action |
| Select all | `Ctrl + A` | `Cmd + A` | Selects everything in the current context |
| Save | `Ctrl + S` | `Cmd + S` | Saves the current file |
| Find | `Ctrl + F` | `Cmd + F` | Opens a search/find dialog |
| Switch app | `Alt + Tab` | `Cmd + Tab` | Cycles between open applications |
| Close tab/window | `Ctrl + W` | `Cmd + W` | Closes the current tab or window |

**Practice:** open any text field (e.g., a search bar), type a few words, then use `Ctrl+A` to select all, `Ctrl+C` to copy, click somewhere else, and `Ctrl+V` to paste.

### Windows and Applications

An **application** (also called a program or app) is a piece of software that does a specific task — a web browser for browsing the internet, a text editor for writing text, a calculator for math.

**Opening an application:**

- **Windows**: Click the **Start button** (bottom-left corner) and type the app name, or click it from the list
- **macOS**: Click the **Launchpad** (rocket icon in the dock) or open **Finder → Applications**
- **Both**: Double-click a desktop shortcut if one exists

**Understanding windows:**

When an application opens, it appears in a **window** — a rectangular area on the screen:

```
┌─────────────────────────────────────────┐
│ ─ ─ ─ Title bar ─ ─ ─     ─  □  ✕    │  ← window controls
├─────────────────────────────────────────┤
│ Menu bar (File, Edit, View, ...)       │
├─────────────────────────────────────────┤
│                                         │
│           Window content                │
│           (text, web page, image, ...) │
│                                         │
│                                         │
├─────────────────────────────────────────┤
│ Status bar (optional)                   │
└─────────────────────────────────────────┘
```

| Control | Windows | macOS | Action |
|---------|---------|-------|--------|
| Minimize | `_` (bottom-left of title bar) | Yellow button (top-left) | Hides the window (click its taskbar icon to restore) |
| Maximize/Restore | `□` (bottom-left) | Green button (top-left) | Fills the screen / returns to previous size |
| Close | `✕` (bottom-left) | Red button (top-left) | Closes the window (and sometimes the app) |

**Switching between open applications:**

- Click the app's icon in the taskbar (Windows) or dock (macOS)
- Use `Alt + Tab` (Windows) or `Cmd + Tab` (macOS) to cycle through open apps

### Files and Folders

Everything stored on your computer is organized into **files** and **folders** (also called directories).

- A **file** is a container for data: a document, an image, a song, a program
- A **folder** is a container for files and other folders

Think of it like a filing cabinet:
- The cabinet is your **hard drive** (the physical storage)
- Drawers are **folders** at the top level
- Inside each drawer are **files** and **sub-folders**

**The file system is a tree:**

```
📁 This PC / Macintosh HD
├── 📁 Desktop
│   ├── 📄 notes.txt
│   └── 📄 shopping-list.txt
├── 📁 Documents
│   ├── 📁 School
│   │   ├── 📄 essay.docx
│   │   └── 📄 report.pdf
│   ├── 📄 resume.pdf
│   └── 📄 budget.xlsx
├── 📁 Downloads
│   ├── 📄 photo.jpg
│   └── 📄 installer.exe
├── 📁 Pictures
│   ├── 📁 Vacation
│   │   ├── 📄 beach.jpg
│   │   └── 📄 sunset.jpg
│   └── 📄 profile.png
└── 📁 Music
    └── 📄 song.mp3
```

**Navigating with a file manager:**

| Action | Windows (File Explorer) | macOS (Finder) |
|--------|------------------------|----------------|
| Open | Double-click | Double-click |
| Go back | Click `←` arrow or press `Backspace` | Click `←` arrow |
| Go up one level | Click the folder name in the address bar | Click `←` arrow or `Cmd + ↑` |
| Create new folder | Right-click → New → Folder | Right-click → New Folder |
| Rename | Click once to select, then click again (or press `F2`) | Click once to select, then press `Enter` |
| Delete | Right-click → Delete (or press `Delete` key) | Right-click → Move to Trash |
| Search | Click the search bar (top-right of window) | Click the search bar (top-right) |

**File paths:** every file has a unique location described by its **path** — the sequence of folders you would click to reach it:

- **Windows**: `C:\Users\alice\Documents\report.pdf`
- **macOS**: `/Users/alice/Documents/report.pdf`
- **Linux**: `/home/alice/Documents/report.pdf`

The path separator is `\` on Windows and `/` on macOS/Linux.

### Saving and Opening Files

When you work in an application (writing a document, editing an image), the data lives in the computer's temporary memory (**RAM**). To keep it permanently, you must **save** it to a file on the disk.

**Saving a file for the first time:**

1. Press `Ctrl + S` (or `Cmd + S` on macOS), or go to **File → Save**
2. A dialog box appears asking:
   - **Where** to save it (which folder?)
   - **What name** to give it (filename?)
   - **What type** to save it as (file format?)
3. Choose a folder, type a name, and click **Save**

**File extensions:** the last part of a filename after the dot tells the computer what kind of file it is:

| Extension | File type | Created by |
|-----------|-----------|------------|
| `.txt` | Plain text | Any text editor |
| `.docx` | Word document | Microsoft Word, Google Docs |
| `.pdf` | Portable Document Format | PDF readers, any "Print to PDF" |
| `.jpg` / `.png` | Image | Cameras, image editors |
| `.mp3` | Audio | Music players, recorders |
| `.xlsx` | Spreadsheet | Microsoft Excel, Google Sheets |
| `.html` | Web page | Web browsers, text editors |

**Opening an existing file:**

1. Double-click the file in the file manager — it opens in the default application
2. Or: open the application first, then go to **File → Open** and navigate to the file

**Tip:** if a file opens in the wrong application, right-click it and choose **Open with** to select the correct one.

### The Web Browser

A **web browser** is the application you use to access websites on the internet. The most common browsers are:

| Browser | Where to find it |
|---------|-----------------|
| Google Chrome | Usually pre-installed on Android and Windows |
| Mozilla Firefox | Pre-installed on most Linux distributions |
| Safari | Pre-installed on macOS and iOS |
| Microsoft Edge | Pre-installed on Windows |

**Parts of the browser:**

```
┌─────────────────────────────────────────────────────────────┐
│  ← → ↻    🔒 https://example.com/page       ☆  ⋮         │
├─────────────────────────────────────────────────────────────┤
│  Back  Forward  Reload    Address bar (URL bar)  Bookmarks │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                   Web page content                          │
│                                                             │
│                                                             │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  Tab 1: Example   │  Tab 2: Search   │  +  (new tab)       │
└─────────────────────────────────────────────────────────────┘
```

| Element | What it is |
|---------|-----------|
| **Address bar** | Type a web address (URL) here to go to a website |
| **URL** | Uniform Resource Locator — the address of a web page (e.g., `https://example.com`) |
| **Tabs** | Allow you to have multiple web pages open in one window |
| **Back/Forward buttons** | Navigate through your browsing history |
| **Bookmarks** | Saved links to websites you visit often |

**How to visit a website:**

1. Click in the address bar (it highlights the current URL)
2. Type the web address: `https://www.example.com`
3. Press `Enter`
4. The page loads — you see its content

**How to search the web:**

1. Click in the address bar
2. Type a search query: `how to learn programming`
3. Press `Enter`
4. A search engine (Google, Bing, DuckDuckGo) shows results
5. Click a result to visit that page

**Opening a new tab:** click the `+` button next to the existing tabs, or press `Ctrl + T` (Windows) / `Cmd + T` (macOS).

### Connecting to the Internet

The internet is the global network that connects your computer to servers around the world. Websites, email, streaming video — all of it travels over the internet.

**Wi-Fi connection:**

1. Look for the **Wi-Fi icon** in the system tray (Windows, bottom-right) or menu bar (macOS, top-right)
2. Click it to see available networks
3. Click your network name
4. Enter the password if prompted
5. The icon changes to show you are connected

**If you cannot connect:**

| Problem | What to try |
|---------|------------|
| Wi-Fi not showing | Make sure Wi-Fi is turned on (there is usually a physical switch or function key) |
| Wrong password | Re-enter the password carefully — it is case-sensitive |
| No internet | The router might be off or there might be a service outage — restart the router by unplugging it for 10 seconds |
| Slow connection | Move closer to the router, or disconnect other devices using the network |

**Ethernet (wired connection):** some computers connect via an Ethernet cable — a thick cable plugged into the back or side of the computer, leading to a router. This is usually faster and more reliable than Wi-Fi. If you plug it in, you should be connected automatically.

### What to Do When Something Goes Wrong

Every computer user encounters problems. Here is a systematic approach:

**The application is frozen:**

1. Wait 10 seconds — it may recover
2. If not, try clicking inside the window and pressing `Ctrl + C` (or `Cmd + .` on macOS) to interrupt
3. If still frozen: `Ctrl + Alt + Delete` (Windows) or `Cmd + Option + Esc` (macOS) to open the force-quit dialog
4. Select the frozen application and click **End Task** / **Force Quit**

**The screen is frozen:**

1. Move the mouse — if the cursor moves, the computer is still running, just the application is stuck
2. Try `Alt + Tab` to switch to a different application
3. Press `Ctrl + C` in the terminal to cancel whatever is running
4. As a last resort, hold the power button for 5–10 seconds to force a shutdown

**Something looks wrong (visual glitch):**

1. Try minimizing and restoring the window
2. Try resizing the window by dragging its edges
3. Restart the application

**You lost a file:**

1. Check the **Recycle Bin** (Windows) or **Trash** (macOS) — recently deleted files go there
2. Use the file manager's **search** feature — type part of the filename
3. Check if the application has an **AutoRecover** or **Recent Files** feature

**The golden rule:** if something goes wrong and you do not know what to do, **search for the error message or symptom** in a web browser. Almost every computer problem has been solved and documented by someone else.

## Learning Tips

- **Click on things.** The safest way to learn is to explore. Click icons, open menus, try the options. You cannot break the computer by clicking around.
- **Learn three keyboard shortcuts per day.** Start with `Ctrl+C`, `Ctrl+V`, and `Ctrl+S`. Add one more each day. Within a week they become automatic.
- **Use the search function.** Both the file manager and the browser have search bars. When you cannot find something, search for it.
- **Read error messages.** They look intimidating, but they usually tell you exactly what went wrong. Search for the exact text if you do not understand it.
- **Ask someone.** If you are stuck for more than 5 minutes, ask a friend, colleague, or search online. There is no shame in asking — every expert was once a beginner.
- **Take breaks.** Staring at a screen for hours is tiring. Every 30 minutes, look away from the screen for 20 seconds and stretch.

## Glossary

| Term | Definition |
|------|------------|
| Application | A program that performs a specific task (browser, editor, calculator) |
| Browser | An application for accessing websites on the internet |
| Cursor | The small arrow or pointer on the screen controlled by the mouse |
| Desktop | The main workspace area shown after logging in |
| Directory | A folder that contains files and other folders (synonym for folder) |
| Dock | macOS bar of app icons at the bottom of the screen |
| Download | Receiving data from the internet and saving it to your computer |
| Extension | The suffix after a dot in a filename indicating file type (.txt, .jpg) |
| File | A container for data stored permanently on the disk |
| File manager | The application for browsing files and folders (File Explorer, Finder) |
| Folder | A container for files and other folders in the file system |
| Internet | The global network connecting computers worldwide |
| Path | The location of a file described as a sequence of folders |
| RAM | Random Access Memory — temporary working memory for active data |
| Startup | The process of turning on a computer and loading the operating system |
| Taskbar | Windows bar at the bottom showing open apps and system icons |
| Trackpad | Touch-sensitive pad on a laptop used instead of a mouse |
| URL | Uniform Resource Locator — the address of a web page |
| Wi-Fi | Wireless networking technology for connecting to the internet |
| Window | A rectangular area on the screen showing an application's content |

## Quick References

- [GCFGlobal — Computer Basics](https://edu.gcfglobal.org/en/computerbasics/) — free beginner tutorials on using a computer
- [Digital Unite — Computer Guides](https://www.digitalunite.com/technology-guides/computer-basics) — step-by-step guides for absolute beginners
- [GCFLearnFree — Internet Basics](https://edu.gcfglobal.org/en/internetbasics/) — how to use a browser and search the web safely

## Next Steps

- [What Is a File](what-is-a-file.md) — understand files, extensions, directories, and paths in depth
- [How Computers Work](how-computers-work.md) — what happens inside the machine when you use it
- [Command Line Basics](command-line-basics.md) — control your computer through text commands instead of clicking
