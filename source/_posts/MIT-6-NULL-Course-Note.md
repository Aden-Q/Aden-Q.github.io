---
title: MIT 6.NULL - Course Notes
date: 2023-11-14 01:49:35
tags:
categories:
  - MOOC
---


This introductory course aims to enhance the proficiency of CS students with their tools. It covers a broad range of topics without delving too deeply into each, including command-line usage, shell programming, editors, version control, debugging, profiling, build systems, testing, etc. While some of these concepts may not seem immediately useful during a student's campus life, they prove to be highly valuable in the real industry.

While I won't cover every single detail of the course, I'll highlight some key takeaways that I personally find very helpful.

## Shell Tools and Scripting

### Special Variables for the Bash Shell:

- `$0`: Name of the script/program
- `$1` to `$9`: Arguments to the script
- `$@`: All the arguments
- `$#`: Number of arguments
- `$?`: Return code of the previous commad
- `$$`: PID for the current script
- `!!`: Entire last command
- `$_`: Last argument from the last command

### Finding Files

+ `find`: a program coming along with all Linux distrbutions
+ `fd`: a simple, fast, and user-friendly alternatively to `find`
+ `locate`: uses a database to index and search

### Search Context
+ `grep`: provided by most Linux distributions
+ `ack`: an alternative to `grep`
+ `ag`: an alternative to `grep`
+ `rg`: ripgrep, a fast alternative to `grep`

### Search in History and History-Based Autosuggestions

`fzf` is a general-purpose fuzzy finder that can be used to search from any input stream. As an example, we can use `fzf` to search the output from `history` interactively by running:

```bash
$ history | fzf
```

Another very cool history-related trick is **history-based autosuggestions**. This feature dynamically autocompletes the current shell command with the most recent command in the history sharing a common prefix. It can be enabled in **zsh** with a plugin.

![](https://raw.githubusercontent.com/Aden-Q/blogImages/main/img/20231114010908.png)

### Directory Navigation

Tools like `fasd` and `autojump` can be used to find frequent and recent files/directories.

More complex tools like `tree`, `broot`, `nn`, `ranger` can be used to get an overview of a directory structure.

### File Editing

`sed`, `awk` are useful tools to edit files/input streams. Both have their own programming languages and can be used along with **regex**, a powerful tool for pattern matching. Even if we do not use shell, it's crucial to be familiar with regex for a developer.

### Misc

The `xargs` command can be used to execute a command using STDIN as arguments. We might need to use this tool sometimes because some commands take both STDIN (i.e. any input stream) and command line arguments, but some commands like `tar` and `rm` only take input from arguments. As an example:

```bash
$ ls | xargs rm
```

This command will delete the files in the current directory. Of course there is a better way to do the same thing. This is just an example for illustration purposes.

## Editors

I personally use **VSCode** for both my side projects and work. While it comes with a vibrant remote development toolset, including features like SSH, tunneling, remote containers, and VSCode server, it may not cover all circumstances. There are situations where a command-line editor like **Vim** is still beneficial.

## Command-line Environment

### Job Control









### Terminal Multiplexers

`tmux` is a powerful tool used for terminal multiplexing. It can maintain persistent sessions and connections to remote machines over SSH.

`tmux` has three core concepts:
+ Sessions: a session is an independent workspace with one or more windows
  - `tmux` starts a new session
  - `tmux new -s NAME` starts it with that name
  - `tmux ls` lists the current sessions
  - Within `tmux` typing `<C-b> d` detaches the current session
  - `tmux a` attaches the last session. You can use -t flag to specify which session to attach to
- Windows: equivalent to tabs in editors or browsers
    - `<C-b> c` creates a new window. To close it you can just terminate the shells doing `<C-d>`
    - `<C-b> N` go to the N th window. Note windows are numbered
    - `<C-b> p` goes to the previous window
    - `<C-b> n` goes to the next window
    - `<C-b> ,` renames the current window
    - `<C-b> w` lists current windows
- Panes: like vim splits, panes let you have multiple shells in the same visual display
    - `<C-b> "` splits the current pane horizontally
    - `<C-b> %` splits the current pane vertically
    - `<C-b> <arrow key>` moves to the pane in the specified direction
    - `<C-b> z` toggles zoom for the current pane
    - `<C-b> [` starts scrollback. You can then press `<space>` to start a selection and <enter> to copy that selection
    - `<C-b> <space>` cycles through pane arrangements

Note that we can make those shortcuts even "shorter" or more intuitive by customizing them in `.tmux.conf`. Please check [my config](https://github.com/Aden-Q/dotfiles/blob/main/tmux.conf) as an example.

### Alias

Most shells support aliasing. A shell alias is a short form for another command that your shell will replace automatically for you. For instance:

```bash
$ alias ga="git add"
$ alias gc="git commit"
$ alias gp="git push"
```

To make the alias persistent, we need to add them to the shell configuration.

### Dotfiles
Many programs are configured using plain-text files know as *dotfiles*. Some examples are:

+ `bash`: `~/.bashrc`, `~/.bash_profile`
+ `git`: `~/.gitconfig`
+ `vim`: `~/.vimrc`, `~/.vim` folder
+ `ssh`: `~/.ssh/confg`
+ `tmux`: `~/.tmux.conf`
+ `.netrc`: a magic file 

Organizing dotfiles can help us easily migrate to a new machine/environment. A typical way to organize them is using a GitHub repo. [Here](https://github.com/Aden-Q/dotfiles) is the repo holding all my dotfiles.

### Remote Development with SSH


## System Logs
A few useful Linux programs can be used to access system logs. `systemd`, `journalctl `, `dmesg` kernel logs

## Profiling

### Resource Monitoring Tools


## Metaprogramming

### Build Systems



### Dependency and Semantic Versioning
Recently during my work, I also encountered the concetp of [semantic vesioning](https://www.conventionalcommits.org/en/v1.0.0/) and [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/). Those two concepts are important in the sense to allow automate version bump, and changelogs.

Semantic versioning is important in the life cycle of project development in a few aspects:









