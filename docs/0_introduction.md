# UNIX, Linux  & UNIX Shell 

!!! abstract "Lesson Objectives"

    - Quick overview on UNIX operating system and it's importance
    - Differences/similarities between UNIX vs. Linux
    - What is a shell and importance of UNIX shell/s for Bioinformatics (or for Scientific computing in general)
    - Types of Shell and intro to Bash Shell (we will be using the latter throughout the workshop)

<center>
![image](./images/unix_funnel.png){width="350"}
</center>

<!--- check -->

## The UNIX operating system

Unix is a multi-user operating system which allows more than one person to use the computer resources at a time. It was originally designed as a time-sharing system to serve several users simultaneous. Unix allows direct communication with the computer via a terminal, hence being very interactive and giving the user direct control over the computer resources. Unix also gives users the ability to share data and programs among one another.

Unix is a generic operating system which takes full advantage of all available hardware such as 32-bit processor chips, expanded memory, and large, fast hard drives. Since Unix is written in a machine-independent language (C/C++) it is portable to many different types of machines including PC's. Therefore, Unix can be adapted to meet special requirements.The UNIX operating system is made up of three parts; the kernel, the shell and the programs.


!!! quote "On Unix philosophy"

    “Although that philosophy can’t be written down in a single sentence, as its heart is the idea that the power of a system comes more from the relationships among programs than from the programs themselves. Many UNIX programs do quite trivial things in isolation, but, combined with other programs, become general and useful tools.” ***– Brian Kernighan & Rob Pike***

!!! info "The UNIX operating system is made up of three parts; the ^^kernel^^, the ^^shell^^ and the programs"

    Kernel − The kernel is the heart of the operating system. It interacts with the hardware and most of the tasks like memory management, task scheduling and file management.

    ^^**Shell**^^ − The shell is the utility that processes your requests (acts as an interface between the user and the kernel). When you type in a command at your terminal, the shell *interprets* (operating as in ^^*interpreter*^^) the command and calls the program that you want. The shell uses standard syntax for all commands. The shell recognizes a limited set of commands, and you must give commands to the shell in a way that it understands: Each shell command consists of a command name, followed by command options (if any are desired) and command arguments (if any are desired). The command name, options, and arguments, are separated by blank space. 



## UNIX vs. Linux

Linux is not Unix, but it is a "Unix-like" operating system. Linux system is derived from Unix and it is a continuation of the basis of Unix design. Linux distributions are the most famous and healthiest example of the direct Unix derivatives. BSD (Berkley Software Distribution) is also an example of a Unix derivative.

??? "Unix-like & a bit more on Linux"

    A Unix-like OS (also called as UN*X or *nix) is the one that works in a way similar to Unix systems, however, it is not necessary that they conform to Single UNIX Specification (SUS) or similar POSIX (Portable Operating System Interface) standard.

    SUS is a standard which is required to be met for any OS to qualify for using ‘UNIX’ trademark. This trademark is granted by [‘The Open Group’](https://www.opengroup.org/)

    Few Examples of currently registered UNIX systems include macOS, Solaris, and AIX. If we consider the POSIX system, then Linux can be regarded as Unix-like OS.

    Linux is just the **kernel** and not the complete OS. This Linux kernel is generally packaged in Linux distributions which thereby makes it a complete OS.

    Linux distribution (also called as a **distro** in short) is an operating system that is created from a collection of software built upon the Linux Kernel and is a package management system. A standard Linux distribution consists of a Linux kernel, GNU system, GNU utilities, libraries, compiler, additional software, documentation, a window system, window manager and a desktop environment. Most of the software included in a Linux distribution is free and open source. They may include some proprietary software like binary blobs which is essential for a few device drivers.


## UNIX Shell for Bioinformatics

A shell is a computer program that presents a command line interface which allows you to control your computer using commands entered with a keyboard instead of controlling graphical user interfaces (GUIs) with a mouse/keyboard/touchscreen combination.

There are many reasons to learn about the shell:

* Many bioinformatics tools can only be used through a command line interface. Many more have features and parameter options which are not available in the GUI. BLAST is an example. Many of the advanced functions are only accessible to users who know how to use a shell.
* The shell makes your work less boring. In bioinformatics you often need to repeat tasks with a large number of files. With the shell, you can automate those repetitive tasks and leave you free to do more exciting things.
* The shell makes your work less error-prone. When humans do the same thing a hundred different times (or even ten times), they’re likely to make a mistake. Your computer can do the same thing a thousand times with no mistakes.
* The shell makes your work more reproducible. When you carry out your work in the command-line (rather than a GUI), your computer keeps a record of every step that you’ve carried out, which you can use to re-do your work when you need to. It also gives you a way to communicate unambiguously what you’ve done, so that others can inspect or apply your process to new data.
* Many bioinformatic tasks require large amounts of computing power and can’t realistically be run on your own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed through a shell.

## Different Types of Shells 

Being able to interact with the kernel makes shells a powerful tool. Without the ability to interact with the kernel, a user cannot access the utilities offered by their machine’s operating system.

Let’s take a look at some of  the major shells that are available for the Linux environment

!!! info "Types of Shells"

    === "Bourne Shell (sh)"

        Developed at AT&T Bell Labs by Steve ***Bourne***, the Bourne shell is regarded as the first UNIX shell ever. It is denoted as sh. It gained popularity due to its compact nature and high speeds of operation.

    === "C Shell (csh)"

        The C shell was created at the University of California by Bill Joy. It is denoted as csh. It was developed to include useful programming features like in-built support for arithmetic operations and a syntax similar to the C programming language.

        Further, it incorporated command history which was missing in different types of shells in Linux like the Bourne shell. Another prominent feature of a C shell is “aliases”.

        The complete path-name for the C shell is `/bin/csh`. By default, it uses the prompt `hostname#` for the root user and `hostname%` for the non-root users.

    === "Korn Shell (ksh)"

        The Korn shell was developed at AT&T Bell Labs by David Korn, to improve the Bourne shell. It is denoted as ksh. The Korn shell is essentially a superset of the Bourne shell.

        Besides supporting everything that would be supported by the Bourne shell, it provides users with new functionalities. It allows in-built support for arithmetic operations while offering interactive features which are similar to the C shell.

        The Korn shell runs scripts made for the Bourne shell, while offering string, array and function manipulation similar to the C programming language. It also supports scripts which were written for the C shell. Further, it is faster than most different types of shells 

    === "Z Shell (zsh)"

        zsh is a shell designed for interactive use, although it is also a powerful scripting language. Many of the useful features of bash, ksh, and tcsh were incorporated into zsh; many original features were added.

    === "Fish Shell (fish)"

        Fish is a fully-equipped command line shell (like bash or zsh) that is smart and user-friendly. Fish supports powerful features like syntax highlighting, autosuggestions, and tab completions that just work, with nothing to learn or configure.

        If you want to make your command line more productive, more useful, and more fun, without learning a bunch of arcane syntax and configuration options, then fish might be just what you’re looking for!

### Type of Shell for this workshop: GNU Bourne-Again shell (bash)


The GNU Bourne-Again shell was designed to be compatible with the Bourne shell. It incorporates useful features from different types of shells in Linux such as Korn shell and C shell.

- The shell's name **bash** is an acronym for "Bourne Again Shell", a pun on the name of the Bourne shell that it replaces and the notion of being "born again"

First released in 1989, it has been used as the default login shell for most Linux distributions. Bash was also the default shell in all versions of Apple macOS prior to the 2019 release of macOS Catalina, which changed the default shell to zsh, although Bash remains available as an alternative shell

The Bash command syntax is a superset of the Bourne shell command syntax. Bash supports brace expansion, command line completion (Programmable Completion),basic debugging and signal handling (using `trap`) among other features. Bash can execute the vast majority of Bourne shell scripts without modification, with the exception of Bourne shell scripts stumbling into fringe syntax behavior interpreted differently in Bash or attempting to run a system command matching a newer Bash builtin, etc. 

- - - 

<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/shell-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>