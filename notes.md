
Questions:

- What is a directory?

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

:bulb: to make an alias permanent, they must be added to your *zshrc* file. Save and reload: `source ~/.zshrc`

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



### 4.2 Variables

### 4.3 Arguments

Arguments let you pass input into your script when you run it.

Key variables:

| Command         | Description                    |
|-----------------|-------------------------------|
| `pwd`           | Show current directory         |
| `ls`            | List files in directory        |
| `cd folderName` | Change directory              |
| `mkdir`         | Create directory              |
| `touch`         | Create a file                 |
| `rm`            | Delete a file                 |
| `open`          | Open a file                   |
| `code`          | Open a file in VS Code        |

### 4.4 Conditions


### 4.5 Loops

### 4.6 Functions

### 4.7 Error Handling & Exit Codes



