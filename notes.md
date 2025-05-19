# Learning Zsh

## 1. Navigating the File System

### Terminal basics:

- `pwd`: show current directory

- `ls`: list files in directory

- `cd folderName`: change directory

- `cd ..`: go up one level

- `cd ~` or `cd`: go to home directory

- `mkdir folderName`: create directory

- `touch file.txt`: create a file

- `rm file.txt`: delete a file

- `open file.txt`: open a file

- `code file.txt`: open a file via VS Code

### Shortcuts & Keybindings:

- Tab auto-completion

- Wildcard matching: `ls *.txt` lists all `.txt` files

- Arrow keys to scroll through command history

- `control`+`C`: to cancel out and get back to a fresh prompt

- `control`+`A`/`E`: jump to the beginnging/end of a line

- `control`+`U`: delete a line

- `control`+`W`: delete the last word

## 2. Aliases

An alias is a custom shortcut for a command (or series of commands). It saves time and reduces typing.

`alias`: view existing aliases

:bulb: to make an alias permanent, it must be added to your *zshrc* file.

Examples:

- `alias ..='cd ..'`

- `alias ...='cd ../..'`

- `alias c='clear'`

- `alias h='history'`

- `alias gs='git status'`

- `alias ga='git add .'`

- `alias gc='git commit -m'`

## 3. Functions

Functions can take arguments, run multiple commands, and include logic (conditionals or loops).

:bulb: to make a function permanent, it must be added to your *zshrc* file.

Syntax:

```Bash
function greet() {
  echo "Hello, $1!"
}
```

Examples:

```Bash
# make a directory and go into it
mkcd() {             
  mkdir -p "$1" && cd "$1"
}

# reload .zshrc
reload() {             
  source ~/.zshrc && echo "Zsh config reloaded!"
}

# go to a projects folder (customized path)
proj() {               
  cd ~/OneDrive/Projects/"$1"
}
```

## 4. Scripting

### 4.1 Basics

A Zsh script is a file that contains a list of commands, just like you’d type in the terminal. It allows you to automate repetitive tasks.

**Shebang line**: this first line of a script determines which interpreter should be used to run the script.

```Bash
#!/bin/zsh
```

Example:

```Bash
# 1. Create script file
touch info.zsh       # create the file
chmod +x info.zsh    # make it executable
code info.zsh        # open in VS Code

# 2. Add to script
#!/bin/zsh

echo "👋 Hello, $USER!"
echo "📅 Today is: $(date)"
echo "📁 You're in: $(pwd)"

#3. Run script
./info.zsh
# output:
# 👋 Hello, burntsoup!
# 📅 Today is: Sun 18 May 2025 07:33:51 EDT
# 📁 You're in: /Users/burntsoup/scripts
```

:bulb: you can’t run a script until it's been qualified as an **executible** by running: `chmod +x info.zsh`. `chmod` (change mode) changes a file's permissions (who can read/write/execute it) and `+x` (add executable permission) tells the system that *this file can now be run as a program*.

```Bash
# Before
ls -l info.zsh    # -rw-r--r--  # only readable and writable
./info.zsh    # zsh: permission denied  # trying to run it won't work

# After chmod +x
chmod +x info.zsh
ls -l info.zsh    # -rwxr-xr-x  # now it's executable
./info.zsh    # it runs!
```

:bulb: **What does `./` mean?**: `.` means the current directory and `/` is the path separator, so `./info.zsh` means *run this file named info.zsh in this exact folder*. If we ran `info.zsh` without `./`, your shell would search for it in your `$PATH` and if `info.zsh` isn’t in a directory listed in `$PATH`, the shell won’t find it, even if you’re sitting in the same folder where the file is stored.

:bulb: **What is `$PATH`?**: it's an **environment variable** that tells your shell which directories to search when you run a command. These directories contains executable files. Therefore, if I type `git`, the shell looks for a file called *git* inside each folder add to `$PATH`, in order, until it finds it and runs it. View the directories added to your `$PATH`: `echo $PATH | tr ':' '\n'`.

:bulb: **What's an environment variable?**: it's a named value stored in your shell’s environment that affects how processes run, can be inherited by scripts or programs you launch, and is often used for configuration (e.g. system paths, settings, flags). Think of it as *a global variable that lives inside your shell session and influences how things behave*. Common examples:

- `PATH`: list of folders where executable files live

- `HOME`: path to your home directory

- `USER`: your username

- `SHELL`: your current shell

**Best practice**: keep all your scripts in a `~/scripts` or `~/bin` folder and add that folder to your **PATH** so that you can run your scripts from anywhere. In `zshrc`, add `export PATH="$HOME/scripts:$PATH"`. Now you can run `info.zsh` from anywhere.

### 4.2 Variables

Variables allow you to store and reuse values, pass information to commands, and configure your environment.

**Declaring variables**: `name="John"`

:bulb: no spacing around `=`, they can include letters, numbers, and underscores

**Accessing variables**: `$name`

```Bash
greetings="Hello world"
echo $greetings    # Hello world
```

**Common use cases**:

- **Storing user inputs or arguments**:

  ```Bash
  # greet.zsh script:
  #!/bin/zsh

  name=$1
  echo "Hello, $name!"
  ```

  ```Bash
  ./greet.zsh Luke    # Hello, Luke!
  ```

- **Assigning the output of a command**: `my_variable=$(some_command)`

  :bulb: allows you to reuse the output of a command without having to run the command again.

  ```Bash
  # Example 1:
  now=$(date)
  echo now    # Sun 18 May 2025 08:50:36 EDT

  # Example 2:
  ls *.txt | wc -l    # counts the number of text files in a folder
  file_count=$(ls *.txt | wc -l)    # store that command in a variable
  echo "There are $file_count text files in this folder"    
  ```

- **Setting configuration values**:

  ```Bash
  env="development"
  log_file="log.txt"
  # . . .
  echo "[$env] Starting..." >> "$log_file"
  ```

- **Storing paths or filenames**:

  ```Bash
  #!/bin/zsh

  backup_dir="$HOME/Backups"
  project_dir="$HOME/Projects/myapp"
  backup_name="myapp_$(date +%F).tar.gz"

  mkdir -p "$backup_dir"
  tar -czf "$backup_dir/$backup_name" "$project_dir"

  echo "Backup saved to: $backup_dir/$backup_name"
  ```

**Exporting variables**: an exported variable is one that is available not only in your current shell, but also in any child process you launch from that shell. It’s a variable you *share* with anything else you run from the shell (scripts, programs, terminals, etc.).

```Bash
# Example (Without export)
greeting="Hello"
zsh -c 'echo $greeting'    # No output. greeting only exists in your current shell session. zsh -c opens a new shell that isn't aware of greeting

# Example (With export)
export greeting="Hello"
zsh -c 'echo $greeting'    # Hello    # greeting was inherited by the child shell.
```

```Bash
# Typical use case:

# 1. Set variables that apps/tools rely on. Apps will look for these when they start:
export NODE_ENV=production    # Node apps might behave differently based on NODE_ENV
export EDITOR=nano    # Git will use the editor defined in EDITOR
export PATH="$HOME/bin:$PATH"    # The shell uses PATH to know where to find commands

# 2. Passing data into scripts:
export API_KEY="secret-123"
./upload.zsh
# in your upload.zsh script:
#!/bin/zsh
echo "Using API key: $API_KEY"    # although the script didn’t define API_KEY, it can read it because the variable was exported

# 3. Making variables available globally through your zschrc file
# inside your zshrc file:
export what_is_life="tacos"
# inside terminal:
echo $what_is_life    # tacos
```

### 4.3 Arguments

Arguments are values passed to a script when you run it. They allow you to customize script behavior without modifying the script’s code.

When you run a script like `./greet.zsh Luke`, `Luke` is an *argument* passed to the script. Inside the script, `Luke` is accessed by *positional variables*.

Positional variables:

| Variable | Meaning                      |
|----------|------------------------------|
| `$0`     | The name of the script       |
| `$1`     | First argument               |
| `$2`     | Second argument              |
| `$@`     | All arguments as a list      |
| `$#`     | The number of arguments      |

Example:

```Bash
# greet.zsh script:
#!/bin/zsh

echo "Script name: $0"
echo "Hello, $1!"
echo "You passed $# argument(s): $@"
```

```Bash
./greet.zsh Sally

# output:
# Script name: ./greet.zsh
# Hello, Sally!
# You passed 1 argument(s): Sally
```

**Looping through arguments** using `$@`:

```Bash
#!/bin/zsh

for name in "$@"; do
  echo "Hello, $name!"
done
```

```Bash
./greet.zsh Luke Sally Kyle

# output:
# Hello, Luke!
# Hello, Sally!
# Hello, Kyle!
```

Default values with fallbacks using `${1:-default}`:

```Bash
name=${1:-Guest}
echo "Welcome, $name!"
```

```Bash
./script.zsh Carl    # Welcome, Cark!
./script.zsh         # Welcome, Guest!
```

### 4.4 Conditionals

Conditionals allow your script to do different things depending on circumstances, such as:

- which arguments are passed

- whether a file exists

- whether a number is greater than another

**Conditional statements**:

1. **`If` statement**:

    ```Bash
    if [ condition ]; then
      # do something
    fi
    ```

    Example:

    ```Bash
    # conditions.zsh script:
    #!/bin/zsh

    if [ $1 = "John" ]; then
      echo "Hi $1!"
    fi
    ```

    ```Bash
    conditions.zsh John    # Hi John!
    ```

2. **`If`/`else` statement**:

    ```Bash
    if [ condition ]; then
      # true - do something
    else
      # false - do something else
    fi
    ```

    Example:

    ```Bash
    if [ $1 = "John" ]; then
      echo "Hi $1!"
    else
      echo "Hi stranger!"
    fi
    ```

3. **`If`/`elif`/`else` statement**:

    ```Bash
    if [ condition1 ]; then
      # do this
    elif [ condition2 ]; then
      # do that
    else
      # fallback
    fi
    ```

    Example:

    ```Bash
    if [ $1 = "John" ]; then
      echo "Hi $1!"
    elif [ $1 = "Jane" ]; then
      echo "Hi $1!"
    else
      echo "Hi stranger!"
    fi
    ```

**Test operators**:

1. Strings

    | Operator | Meaning             |
    |----------|---------------------|
    | =        | Equal to            |
    | !=       | Not equal to        |
    | -z       | String is empty     |
    | -n       | String is not empty |

    Example:

    ```Bash
    if [ -z "$1" ]; then
      echo "No input provided"
    fi
    ```

2. Files

    | Operator | Meaning         |
    |----------|----------------|
    | -e       | File exists    |
    | -f       | Is a regular file |
    | -d       | Is a directory |
    | -r       | Is readable    |
    | -w       | Is writable    |
    | -x       | Is executable  |

    ```Bash
    if [ -f "$1" ]; then
      echo "Processing file: $1"
    else
      echo "File not found"
    fi
    ```

3. Numbers

    | Operator | Meaning                |
    |----------|------------------------|
    | -eq      | Equal                  |
    | -ne      | Not equal              |
    | -lt      | Less than              |
    | -le      | Less than or equal     |
    | -gt      | Greater than           |
    | -ge      | Greater or equal       |

    ```Bash
    a=5
    b=10

    if [ $a -lt $b ]; then
      echo "$a is less than $b"
    fi
    ```

Example: create a script that takes a filename as an argument, checks if it exists, and confirms its existance.

```Bash
# check_file.zsh script:
#!/bin/zsh

file=$1

if [ -z "$file" ]; then
  echo "❌ Please provide a filename"
  exit 1
elif [ -f "$file" ]; then
  echo "✅ Found file: $file"
else
  echo "❌ File not found"
fi
```

```Bash
# cd Documents/testFolder/
ls    # file1.txt file2.txt file3.txt

check_file.zsh    # ❌ Please provide a filename
check_file.zsh file1.txt    # ✅ Found file: file1.txt
check_file.zsh file4.txt    # ❌ File not found: file4.txt
```

### 4.5 Loops

Loops allow you to automate repetition - running the same command on multiple files, users, lines, or values.

Types:

1. `for`

    ```Bash
    for item in list; do
      # use $item
    done
    ```

    Example:

    ```Bash
    # loops.zsh script:
    #!/bin/zsh

    list=("apple" "banana" "cherry")

    echo "Grocery list:"
    for item in $list; do
      echo "$item!"
    done
    ```

    ```Bash
    loops.zsh
    # output:
    # Grocery list:
    # apple!
    # banana!
    # cherry!
    ```

    Example:

    ```Bash
    for ((i = 1; i <= 5; i++)); do
      echo "Iteration $i"
    done
    ```

    ```Bash
    loops.zsh
    # Iteration 1
    # Iteration 2
    # Iteration 3
    # Iteration 4
    # Iteration 5
    ```

2. `while`

    ```Bash
    while [ condition ]; do
      # do something
    done
    ```

### 4.6 Functions

### 4.7 Error Handling & Exit Codes



