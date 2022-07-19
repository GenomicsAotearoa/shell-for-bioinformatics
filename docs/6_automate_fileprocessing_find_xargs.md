# Automating File-Processing with find and xargs

In this section, we’ll learn about a more powerful way to specify files matching some criteria using Unix `find`. We’ll also see how files printed by `find` can be passed to another tool called `xargs` to create powerful Unix-based processing workflows.

Suppose you have a program named *analyse_fastq* that takes multiple filenames through standard in to process. If you wanted to run this program on all files with the suffix .fastq, you might run:

```bash
ls *.fastq | analyse_fastq
```

Your shell expands this wildcard to all matching files in the current directory, and `ls` prints these filenames. Unfortunately, this leads to a common complication that makes `ls` and wildcards a fragile solution. Suppose your directory contains a filename called **treatment 03.fastq**. In this case, `ls` returns *treatment 03.fastq* along with other files. However, because files are separated by spaces, and this file contains a space, *analyse_fastq* will interpret *treatment 03.fastq* as two separate files, named *treatment* and *03.fastq*. This problem crops up periodically in different ways, and it’s necessary to be aware of when writing file-processing pipelines. Note that this does not occur with file globbing in arguments—if *analyse_fastq* takes multiple files as arguments, your shell handles this properly: