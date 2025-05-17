
Questions:

- What is a directory?



## Navigating the File System

`pwd`: show current directory

`ls`: list files in directory

`cd folderName`: change directory

`cd ..`: go up one level

`cd ~` or `cd`: go to home directory

`mkdir folderName`: create directory

`touch file.txt`: create a file

`rm file.txt`: delete a file

`open file.txt`: open a file

`code file.txt`: open a file via VS Code

## Aliases

An alias is a custom shortcut for a command (or series of commands). It saves time and reduces typing.

`alias`: view existing aliases

:bulb: to make an alias permanent, they must be added to your *zshrc* file. Save and reload: `source ~/.zshrc`

Examples:

`alias ..='cd ..'`

`alias ...='cd ../..'`

`alias c='clear'`

`alias h='history'`

`alias gs='git status'`

`alias ga='git add .'`

`alias gc='git commit -m'`

## Functions

Functions can take arguments, run multiple commands, and include logic (conditionals or loops)

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

## Tips

- Tab auto-completion

- Wildcard matching: `ls *.txt` lists all `.txt` files

- Arrow keys to scroll through command history

- `control`+`C`: to cancel out and get back to a fresh prompt

- `control`+`A`/`E`: jump to the beginnging/end of a line

- `control`+`U`: delete a line

- `control`+`W`: delete the last word
