# 7. Automating File-Processing with find and xargs

In this section, we’ll learn about a more powerful way to specify files matching some criteria using Unix `find`. We’ll also see how files printed by `find` can be passed to another tool called `xargs` to create powerful Unix-based processing workflows.

Suppose you have a program named `analyse_fastq` that takes multiple filenames through a standard process. If you wanted to run this program on all files with the suffix .fastq, you might run:

```bash
ls *.fastq | analyse_fastq
```

!!! failure "Fail"

    Your shell expands this wildcard to all matching files in the current directory, and `ls` prints these filenames. Unfortunately, this leads to a common complication that makes `ls` and wildcards a fragile solution. Suppose your directory contains a filename called ***treatment 03.fastq***. In this case, `ls` returns ***treatment 03.fastq*** along with other files. However, because files are separated by spaces, and this file contains a space, `analyse_fastq` will interpret *treatment 03.fastq* as two separate files, named ***treatment*** and ***03.fastq***. This problem crops up periodically in different ways, and it’s necessary to be aware of when writing file-processing pipelines. Note that this does not occur with file globbing in arguments—if `analyse_fastq` takes multiple files as arguments, your shell handles this properly:

```bash
analyse_fastq *.fastq
```
Here, shell automatically escapes the space in the filename ***treatment 03.fastq***, so `analyse_fastq` will correctly receive the arguments ***treatment-02.fastq***, ***treatment-03.fastq***,. The potential problem here is that there’s a limit to the number of files that can be specified as arguments. The limit is high, but you can reach it with NGS data. In this case you may get a meassage: `: cannot execute [Argument list too long]` 

!!! Success "Solution"

    Solution to both of the above problems is through `find` and `xargs`, as we will see in the following sections.

### Finding files with `find`

Basic syntax for `find` is 

```bash
find path expression
```
!!! note "find" 

    - Path specifies the starting directory for search. Expressions are how we describe which files we want to `find` to return
    - Unlike `ls`, `find`is recursive (it will search through the directory structure). In fact, running `find` on a directory (without other arguments) is a quick way to see it’s structure, e.g.,

    ```bash
    find /nesi/project/nesi02659/| head
    ```
    ```bash
    find: /nesi/project/nesi02659/
    /nesi/project/nesi02659/.jupyter
    /nesi/project/nesi02659/.jupyter/share
    /nesi/project/nesi02659/.jupyter/share/jupyter
    /nesi/project/nesi02659/.jupyter/share/jupyter/nbconvert
    /nesi/project/nesi02659/.jupyter/share/jupyter/nbconvert/templates
    ‘/nesi/project/nesi02659/.jupyter/share/jupyter/nbconvert/templates’: Permission denied
    /nesi/project/nesi02659/.jupyter/share/jupyter/kernels
    /nesi/project/nesi02659/.jupyter/share/jupyter/kernels/sismonr
    /nesi/project/nesi02659/.jupyter/share/jupyter/kernels/sismonr/kernel.json
    /nesi/project/nesi02659/.jupyter/share/jupyter/kernels/sismonr/logo-64x64.png
    ```

Argument `-maxdepth` limits the depth of the search: to search only within the current directory, use `find -maxdepth 1 .`

??? question "Exercise 6.1"

    1. Create a small directory system as below in your current working directory

        ```bash
        mkdir -p hihi_project/{data/raw,scripts,results}
        ```

        ```bash
        touch hihi_project/data/raw/hihi{A,B,C}_R{1,2}.fastq
        ```
    2.  Run `find hihi_project` and examine the output

        ??? success "Output"

            ```bash
            hihi_project/
            hihi_project/results
            hihi_project/data
            hihi_project/data/raw
            hihi_project/data/raw/hihiB_R1.fastq
            hihi_project/data/raw/hihiA_R1.fastq
            hihi_project/data/raw/hihiA_R2.fastq
            hihi_project/data/raw/hihiC_R1.fastq
            hihi_project/data/raw/hihiB_R2.fastq
            hihi_project/data/raw/hihiC_R2.fastq
            hihi_project/scripts
            ```

    3. Use find to print the names of all files matching the pattern “hihiB*fastq” (e.g., FASTQ files from sample “B”, both read pairs): 

        ```bash
        find hihi_project/data/raw/ -name "hihiB*fastq"
        ```
    4. This gives similar results to `ls hihiB*fastq`, as we’d expect. The primary difference is that find reports results separated by new lines and, by default, `find` is recursive. Because we only want to return `fastq` files (and not directories with that matching name), we might want to limit our results using the `-type` option: There are numerous different types you can search for; the most commonly used are `f`for files, `d` for directories, and `l` for links.

        ```bash
        find hihi_project/data/raw/ -name "hihiB*fastq" -type f
        ```
    5. By default, `find` connects different parts of an expression with logical **AND**. The find command in this case returns results where the name matches “hihiB*fastq” and is a file (type “f ”). `find` also allows explicitly connecting different parts of an expression with different operators.
    If we want to get the names of all `fastq` files from samples A or C, we’ll use the operator -or to chain expressions:

        ```bash
        find hihi_project/data/raw/ -name "hihiA*fastq" -or -name "hihiC*fastq" -type f
        ```
    6. Another way to select these files is with negation: Some bash versions will accept `"!"` as the flag for this where others will require `-not` 

        ```bash
        find hihi_project/data/raw/ -type f -not -name "hihiB*fastq"
        ```
    7. Suppose you were sharing this project folder with a colleague and a file named hihiB_R1-temp.fastq was created by them in *hihi_project/data/raw* but you want to ignore it in your file querying:

        ```bash
        find hihi_project/data/raw/ -type f -not -name "hihiB*fastq" -and -not -name "*-temp*"
        ```

### `find`s `-exec`: Running Commands on find’s Results

Find’s real strength in bioinformatics is that it allows you to run commands on every file that is returned by find, using -`exec` option.

Continuing from our last example, suppose that a collaborator created numerous temporary files. Let’s emulate this (in the *hihi_project/data/raw/*): (then `ls` ensure the `-temp.fastq` files were created)

```bash
touch hihi_project/data/raw/hihi{A,C}_R{1,2}-temp.fastq
```

Although we can delete these files with `rm *-temp.fastq`, using `rm` with a wildcard in a directory filled with important data files is too risky. Using `find`’s `-exec` is a much safer way to delete these files.

For example, let’s use `find -exec` and `rm` to delete these temporary files:

```bash
find hihi_project/data/raw/ -name "*-temp.fastq" -exec rm {} \;
```
Notice the (required!) semicolumn and curly brackets at the end of the command! . In one line, we’re able to pragmatically identify and execute a command on files that match a certain pattern. With `find` and `-exec`, a daunting task like processing a directory of 100,000 text files with a program is simple.

In general, `find -exec` is most appropriate for quick, simple tasks (like deleting files, changing permissions, etc.). For larger tasks, `xargs` is a better choice.

## `xargs`

`xargs` reads data from standard input (stdin) and executes the command (supplied to it as an argument) one or more times based on the input read. Any spaces, tabs, and newlines in the input are treated as delimiters, while blank lines are ignored. If no command is specified, `xargs` executes `echo`. (Notice, that echo by itself does not read from standard input!)

`xargs` allows us to take input from standard in, and use this input as arguments to another program, which allows us to build commands programmatically. Using `find` with `xargs` is much like `find -exec`, but with some added advantages that make xargs a better choice for larger tasks.

Let’s re-create our messy temporary file directory example again: .i.e Make sure to run `ls` after the `touch` command to verify the files were created. 

```bash
touch hihi_project/data/raw/hihi{A,C}_R{1,2}-temp.fastq
```

`xargs` works by taking input from standard in and splitting it by spaces, tabs, and newlines into arguments. Then, these arguments are passed to the command supplied. For example, to emulate the behavior of `find -exec` with `rm`, we use `xargs` with `rm`:

```bash
find hihi_project/data/raw -name "*-temp.fastq" | xargs rm
```
`xargs` passes all arguments received through standard in to the supplied program (rm in this example). This works well for programs like `rm`, `touch`, `mkdir`, and others that take multiple arguments. However, other programs only take a single argument at a time. We can set how many arguments are passed to each command call with `xargs`’s `-n` argument. For example, we could call rm four separate times (each on one file) with:

```bash
touch hihi_project/data/raw/zmays{A,C}_R{1,2}-temp.fastq
```
```bash
find hihi_project/data/raw -name "*-temp.fastq" | xargs -n 1 rm
```

One big benefit of `xargs` is that it separates the process that specifies the files to operate on (`find`) from applying a command to these files (through `xargs`). If we wanted to inspect a long list of files find returns before running rm on all files in this list, we could use:

```bash
touch hihi_project/data/raw/hihi{A,C}_R{1,2}-temp.fastq
```
```bash
find hihi_project/data/raw/ -name "*-temp.fastq" > ohno_filestodelete.txt
```
```bash
cat ohno_filestodelete.txt
```
```bash
cat ohno_filestodelete.txt  | xargs rm
```

!!! note "Using `xargs` with Replacement Strings to Apply Commands to Files"

    In addition to adding arguments at the end of the command, `xargs` can place them in predefined positions. This is done with the `-I` option and a placeholder string ({}). Suppose an imaginary program `fastq_stat` takes an input file through the option –in, gathers FASTQ statistics information, and then writes a summary to the file specified by the –out option. We may want our output filenames to be paired with our input filenames and have corresponding names. We can tackle this with `find`, `xargs`, and `basename`: