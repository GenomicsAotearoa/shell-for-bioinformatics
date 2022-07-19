# Automating File-Processing with find and xargs

In this section, we’ll learn about a more powerful way to specify files matching some criteria using Unix `find`. We’ll also see how files printed by `find` can be passed to another tool called `xargs` to create powerful Unix-based processing workflows.

Suppose you have a program named `analyse_fastq` that takes multiple filenames through standard in to process. If you wanted to run this program on all files with the suffix .fastq, you might run:

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

???+ question "Exercise 6.1"

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
    4. This gives similar results to `ls hihiB*fastq`, as we’d expect. The primary difference is that find reports results separated by newlines and, by default, `find` is recursive. Because we only want to return `fastq` files (and not directories with that matching name), we might want to limit our results using the `-type` option: There are numerous different types you can search for; the most commonly used are `f`for files, `d` for directories, and `l` for links.

        ```bash
        find hihi_project/data/raw/ -name "hihiB*fastq" -type f
        ```
    5. By default, `find` connects different parts of an expression with logical **AND**. The find command in this case returns results where the name matches “hihiB*fastq” and is a file (type “f ”). `find` also allows explicitly connecting different parts of an expression with different operators.
    If we want to get the names of all `fastq` files from samples A or C, we’ll use the operator -or to chain expressions:

        ```bash
        find hihi_project/data/raw/ -name "hihiA*fastq" -or -name "hihiC*fastq" -type f
        ```
    6. 