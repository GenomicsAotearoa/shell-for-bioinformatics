# 5. Preview : Automating File-Processing with `find` and `xargs`


??? truck-ramp "This section will be moved to "Advanced Shell for BioInformaitcs""

    In this section, we’ll learn about a more powerful way to specify files matching some criteria using Unix `find`. We’ll also see how files printed by `find` can be passed to another tool called `xargs` to create powerful Unix-based processing workflows.
    
    Suppose you have a program named `analyse_fastq` that takes multiple filenames through a standard process. If you wanted to run this program on all files with the suffix .fastq, you might run:
    
    ```bash
    ls *.fastq | analyse_fastq
    ```
    
    !!! failure "Fail"
    
        Your shell expands this wildcard to all matching files in the current directory, and `ls` prints these filenames. Unfortunately, this leads to a common complication that makes `ls` and wildcards a fragile solution. Suppose your directory contains a filename called ***treatment 03.fastq***. In this case, `ls` returns ***treatment 03.fastq*** along with other files. However, because files are separated by spaces, and this file contains a space, `analyse_fastq` will interpret *treatment 03.fastq* as two separate files, named ***treatment*** and ***03.fastq***. This problem crops up periodically in different ways, and it’s necessary to be aware of when writing file-processing pipelines. Note that this does not occur with file "globbing" in arguments—if `analyse_fastq` takes multiple files as arguments, your shell handles this properly:
    
    Note that this does not occur with file globbing in arguments—if `analyse_fastq` takes multiple files as arguments, your shell handles this properly:
    ```bash
    analyse_fastq *.fastq
    ```
    Here, shell automatically escapes the space in the filename ***treatment 03.fastq***, so `analyse_fastq` will correctly receive the arguments ***treatment-02.fastq***, ***treatment-03.fastq***,. The potential problem here is that there’s a limit to the number of files that can be specified as arguments. The limit is high, but you can reach it with NGS data. In this case you may get a meassage: `: cannot execute [Argument list too long]` 
    
    ??? quote-right "Globbing"
    
        Bash does not support native regular expressions like some other standard programming languages. The Bash shell feature that is used for matching or expanding specific types of patterns is called globbing. Globbing is mainly used to match filenames or searching for content in a file. Globbing uses wildcard characters to create the pattern. The most common wildcard characters that are used for creating globbing patterns are described below.
    
        1. Question mark – (`?`) 
    
            - `?` is used to match any single character. You can use ‘?’ for multiple times for matching multiple characters.
        2. Asterisk – (`*`)
           
            - `*` is used to match zero or more characters. If you have less information to search any file or information then you can use ‘*’ in globbing pattern.
    
        3. Square Bracket – (`[]`)
           
            - `[]` is used to match the character from the range. Some of the mostly used range declarations are mentioned below.
    
        4. Caret – (`^`)
    
            - You can use `^` with square bracket to define globbing pattern more specifically. `^` can be used inside or outside of square bracket. `^` is used outside the square bracket to search those contents of the file that starts with a given range of characters. `^`is used inside the square bracket to show all content of the file by highlighting the lines start with a given range of characters . 
    
    
    
    !!! Success "Solution"
    
        Solution to both of the above problems is through `find` and `xargs`, as we will see in the following sections.
    
    ### Finding files with `find`
    
    Basic syntax for `find` is 
    
    ```bash
    find path expression
    ```
    !!! mg-glass-location "find" 
    
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
    
        - Try the same command with `-maxdepth 1` .i.e.
        ```bash
        find /nesi/project/nesi02659/ -maxdepth 1 | head
        ```
    
    ??? question "Exercise 6.1"
    
        1. Create a small directory system as below in your current working directory
    
            ```bash
            mkdir -p genome-project/{data/raw,scripts,results}
            ```
    
            ```bash
            touch genome-project/data/raw/bird{A,B,C}_R{1,2}.fastq
            ```
        2.  Run `find genome-project` and examine the output
    
            ??? success "Output"
    
                ```bash
                genome-project/
                genome-project/results
                genome-project/data
                genome-project/data/raw
                genome-project/data/raw/birdB_R1.fastq
                genome-project/data/raw/birdA_R1.fastq
                genome-project/data/raw/birdA_R2.fastq
                genome-project/data/raw/birdC_R1.fastq
                genome-project/data/raw/birdB_R2.fastq
                genome-project/data/raw/birdC_R2.fastq
                genome-project/scripts
                ```
    
        3. Use find to print the names of all files matching the pattern “birdB*fastq” (e.g., FASTQ files from sample “B”, both read pairs): 
    
            ```bash
            find genome-project/data/raw/ -name "birdB*fastq"
            ```
        4. This gives similar results to `ls birdB*fastq`, as we’d expect. The primary difference is that find reports results separated by new lines and, by default, `find` is recursive. Because we only want to return `fastq` files (and not directories with that matching name), we might want to limit our results using the `-type` option: There are numerous different types you can search for; the most commonly used are `f`for files, `d` for directories, and `l` for links.
    
            ```bash
            find genome-project/data/raw/ -name "birdB*fastq" -type f
            ```
        5. By default, `find` connects different parts of an expression with logical **AND**. The find command in this case returns results where the name matches “birdB*fastq” and is a file (type “f ”). `find` also allows explicitly connecting different parts of an expression with different operators.
        If we want to get the names of all `fastq` files from samples A or C, we’ll use the operator -or to chain expressions:
    
            ```bash
            find genome-project/data/raw/ -name "birdA*fastq" -or -name "birdC*fastq" -type f
            ```
        6. Another way to select these files is with negation: Some bash versions will accept `"!"` as the flag for this where others will require `-not` 
    
            ```bash
            find genome-project/data/raw/ -type f -not -name "birdB*fastq"
            ```
        7. Suppose you were sharing this project folder with a colleague and a file named birdB_R1-temp.fastq was created by them in *genome-project/data/raw* but you want to ignore it in your file querying:
    
            ```bash
            find genome-project/data/raw/ -type f -not -name "birdB*fastq" -and -not -name "*-temp*"
            ```
    
    ### `find`s `-exec`: Running Commands on find’s Results
    
    `find`’s real strength in bioinformatics is that it allows you to run commands on every file that is returned by find, using -`exec` option.
    
    Continuing from our last example, suppose that a collaborator created numerous temporary files. Let’s emulate this (in the *genome-project/data/raw/*): (then `ls` ensure the `-temp.fastq` files were created)
    
    !!! terminal "code"
    
        ```bash
        touch genome-project/data/raw/bird{A,C}_R{1,2}-temp.fastq
        ```
    
    Although we can delete these files with `rm *-temp.fastq`, using `rm` with a wildcard in a directory filled with important data files is too risky. Using `find`’s `-exec` is a much safer way to delete these files.
    
    For example, let’s use `find -exec` and `rm` to delete these temporary files:
    
    !!! terminal "code"
    
        ```bash
        find genome-project/data/raw/ -name "*-temp.fastq" -exec rm {} \;
        ```
    Notice the (required!) semicolumn and curly brackets at the end of the command! . In one line, we’re able to pragmatically identify and execute a command on files that match a certain pattern. With `find` and `-exec`, a daunting task like processing a directory of 100,000 text files with a program is simple.
    
    In general, `find -exec` is most appropriate for quick, simple tasks (like deleting files, changing permissions, etc.). For larger tasks, `xargs` is a better choice.
    
    ## `xargs`
    
    `xargs` reads data from standard input (stdin) and executes the command (supplied to it as an argument) one or more times based on the input read. Any spaces, tabs, and newlines in the input are treated as delimiters, while blank lines are ignored. If no command is specified, `xargs` executes `echo`. (Notice, that echo by itself does not read from standard input!)
    
    `xargs` allows us to take input from standard in, and use this input as arguments to another program, which allows us to build commands programmatically. Using `find` with `xargs` is much like `find -exec`, but with some added advantages that make `xargs` a better choice for larger tasks.
    
    Let’s re-create our `-temp.fastq` files: .i.e Make sure to run `ls` after the `touch` command to verify the files were created. 
    
    !!! terminal "code"
    
        ```bash
        touch genome-project/data/raw/bird{A,C}_R{1,2}-temp.fastq
        ```
    
    `xargs` works by taking input from standard in and splitting it by spaces, tabs, and newlines into arguments. Then, these arguments are passed to the command supplied. For example, to emulate the behavior of `find -exec` with `rm`, we use `xargs` with `rm`:
    
    !!! terminal "code"
        
        ```bash
        find genome-project/data/raw -name "*-temp.fastq" | xargs rm
        ```
    
    One big benefit of `xargs` is that it separates the process that specifies the files to operate on (`find`) from applying a command to these files (through `xargs`). If we wanted to inspect a long list of files find returns before running `rm` on all files in this list, we could use:
    
    !!! terminal "code"
    
        ```bash
        touch genome-project/data/raw/bird{A,C}_R{1,2}-temp.fastq
        ```
        ```bash
        find genome-project/data/raw/ -name "*-temp.fastq" > ohno_filestodelete.txt
        ```
        ```bash
        cat ohno_filestodelete.txt
        ```
        ```bash
        cat ohno_filestodelete.txt  | xargs rm
        ```
    
    !!! note "Using `xargs` with Replacement Strings to Apply Commands to Files"
    
        In addition to adding arguments at the end of the command, `xargs` can place them in predefined positions. This is done with the `-I` option and a placeholder string ({}). Suppose an imaginary program `fastq_stat` takes an input file through the option –in, gathers FASTQ statistics information, and then writes a summary to the file specified by the –out option. We may want our output filenames to be paired with our input filenames and have corresponding names. We can tackle this with `find`, `xargs`, and `basename`: