# 3. Unix Shell Basics & Recap 

!!! abstract "Lesson Objectives 😶‍🌫️"

    - Navigate your file system using the command line.
    - Quick recap on commands used in routine tasks such copy, move, remove.

    
It is expected that you are already familiar with using the basics of the Unix Shell. As a quick refresher, some frequently used commands are listed below.

For more information about a command, use the Unix `man` (manual) command. For example, to get more information about the `mkdir` command, type:

```bash
man mkdir
```



!!! note "Key commands for navigating around the filesystem are:"

    - `ls` - list the contents of the current directory
    - `ls -l` - list the contents of the current directory in more detail
    - `pwd` - show the location of the current directory
    - `cd DIR` - change directory to directory DIR (DIR must be in your current directory - you should see its name when you type `ls` OR you need to specify either a full or relative path to DIR)
    - `cd -` - change back to the last directory you were in
    - `cd` (also `cd ~/`) - change to your home directory
    - `cd ..` - change to the directory one level above

!!! note "Other useful commands:"

    - `mv` - move files or directories
    - `cp` - copy files or directories
    - `rm` - delete files or directories
    - `mkdir` - create a new directory
    - `cat` - concatenate and print text files to screen
    - `more` - show contents of text files on screen
    - `less` - cooler version of `more`. Allows searching (use `/`)
    - `tree` - tree view of directory structure
    - `head` - view lines from the start of a file
    - `tail` - view lines from the end of a file
    - `grep` - find patterns within files




- - - 

<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/shell-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>