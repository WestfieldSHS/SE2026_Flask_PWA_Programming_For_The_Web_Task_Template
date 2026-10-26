# Creating project folders and files with Bash

A student guide to the Bash steps in the [TempeHS Flask PWA tutorial](https://github.com/TempeHS/Flask_PWA_Programming_For_The_Web_Task_Source/blob/main/docs/README.md).

**Learning goal:** Use a text file and a loop to create your Flask project's folders and starting files.

## 1. Understand the two parts

Bash is a program that runs commands typed into a terminal. A **script** is a set of instructions. You can type or paste these instructions into the terminal, or save them in a file to run later.

| Item | Purpose |
| --- | --- |
| `folders.txt` | Stores the folder names: the **data**. |
| The Bash loop | Reads each name and creates the folder: the **instructions**. |

**Creating the text file does not create the folders automatically. You must also run the loop.**

## 2. Open a Bash terminal

1. Open your project folder in VS Code.
2. Select **Terminal → New Terminal**.
3. On Windows, use the dropdown beside the terminal's **+** button to select **Git Bash**. These commands need Bash, rather than PowerShell or Command Prompt. In Codespaces, Bash is normally available already. On macOS or Linux, if your terminal uses another shell, type `bash` and press Enter to start Bash.

Run these commands separately:

```bash
pwd
```

```bash
ls
```

- `pwd` shows the folder the terminal is currently working in.
- `ls` lists its files and folders.

Work in your **project root**: the main folder for your project, where `main.py` will be stored. Use `cd` to change folders if needed. For example, `cd my_project` moves into a folder called `my_project` inside your current folder.

**The loop will look for your text file and create the folders relative to the terminal's current folder.**

## 3. Prepare the folder list

Run these commands separately in the **terminal**:

```bash
touch folders.txt
```

```bash
code folders.txt
```

`touch` creates the file if it does not already exist. `code` opens it in VS Code.

In the **editor**, type or paste this into `folders.txt`:

```text
database
working_documents
static
static/css
static/icons
static/images
static/js
templates
```

Each line contains one folder path. `static/css` means a folder called `css` **inside** `static`. Listing only `css` would create it in the project root instead.

Before continuing:

- Press Enter once after `templates`, so the final name ends with a newline. Leave the cursor at the start of the next empty line; do not add extra blank lines between names.
- Check the line-ending indicator in VS Code's bottom status bar. If it says **CRLF**, click it and choose **LF**. This avoids unwanted carriage-return characters in names when Bash reads a Windows-formatted file.
- **Save the file** with Ctrl+S, or Cmd+S on macOS.

Run `ls` in the terminal. You should see `folders.txt`.

## 4. Run the loop

Copy this **whole block** and paste it into the **Bash terminal**. Press Enter if needed:

```bash
while read -r line; do
    echo "$line"
    mkdir -p "$line"
done < folders.txt
```

The loop displays each folder name and creates that folder.

### If you type it manually

**Press Enter after each line.** Bash recognises that the loop is unfinished and waits for the remaining instructions.

You may see a `>` prompt while typing. This means Bash is waiting for more input. **Do not type the `>` yourself.**

After you type `done < folders.txt` and press Enter, the loop runs and the normal terminal prompt returns.

If you make a mistake while entering the loop, press **Ctrl+C** to cancel the unfinished command, then start again.

### If you put it all on one line

Line breaks separate commands. On one line, you need semicolons in the right places instead:

```bash
while read -r line; do echo "$line"; mkdir -p "$line"; done < folders.txt
```

Both versions work. Use the version with separate lines while learning because it is easier to read.

## 5. How does it read the file?

The connection is **`< folders.txt`**. Although it appears at the end, it supplies the file's contents as input to the entire loop.

| Code | Meaning |
| --- | --- |
| `while read -r line` | Read the next line into a variable called `line`. Repeat while another line can be read. |
| `-r` | Treat backslashes in the input as ordinary characters. |
| `do` | Start the instructions to repeat. |
| `echo "$line"` | Display the value stored in `line`. |
| `mkdir -p "$line"` | Create that folder, including missing parent folders. Existing folders are fine. |
| `done` | End the repeated instructions. |
| `< folders.txt` | Use this file as input for the whole loop. |

`line` is a variable name chosen by the programmer. **`$line` retrieves its value.**

When the loop reads `static/css`, the instruction `mkdir -p "$line"` acts like:

```bash
mkdir -p "static/css"
```

The quotation marks keep the value together as one path, even if it contains spaces. Keep them in your commands.

## 6. Check your folders

Run:

```bash
ls
```

You should see `database`, `working_documents`, `static` and `templates`, along with your existing files.

Then run:

```bash
ls static
```

You should see `css`, `icons`, `images` and `js`.

## 7. Create the starting files

From the project root, run these commands separately:

```bash
touch files.txt
```

```bash
code files.txt
```

Put this list into `files.txt`:

```text
LICENSE
main.py
database_manager.py
```

Press Enter once after the final name, use **LF** line endings and save the file. Then paste this block into the **Bash terminal**:

```bash
while read -r line; do
    echo "$line"
    touch "$line"
done < files.txt
```

**Correction to the tutorial:** Use `touch "$line"`, not `touch -p "$line"`. The `-p` option belongs to `mkdir`; it is not a valid option for `touch`.

This creates empty files for your code or text. If a file already exists, `touch` updates its timestamp without clearing its contents, so an existing `LICENSE` keeps its text. Unlike `mkdir -p`, `touch` does not create missing parent folders.

Run `ls` to check that the files are present.

## Optional: save a reusable script

1. Create a file called `create_folders.sh` in your project root.
2. Put the four-line folder loop from Step 4 into it.
3. Use **LF** line endings and save it.
4. From the project root, run:

```bash
bash create_folders.sh
```

The `.sh` extension identifies a shell script. Here, `bash` runs the instructions in that file. Keep `folders.txt` in the project root too: the script still reads it from the terminal's current folder.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| `folders.txt: No such file or directory` | Run `pwd` and `ls`. Move to the folder containing the saved text file. |
| A `>` prompt appears after a line | Bash is waiting for the rest of the loop. Continue typing, or use Ctrl+C to cancel. |
| The loop fails when typed on one line | Use separate lines, or copy the one-line version with its semicolons. |
| The final folder is missing | Press Enter after the final name, save the text file and run the loop again. |
| A name has an unexpected character at the end | Set the text file to LF and save it. Ask your teacher for help with folders already created with incorrect names. |
| `mkdir` reports a missing or empty folder name | Remove extra blank lines from the list. Keep the newline immediately after the final name. |
| `touch` reports an invalid `-p` option | Remove `-p`. Use `touch "$line"`. |

## Check your understanding

1. What is the difference between `folders.txt` and the Bash loop?
2. Which part of the loop tells Bash to read `folders.txt`?
3. Why does the list use `static/images` rather than just `images`?
4. Why can you press Enter partway through typing a loop without running it immediately?

