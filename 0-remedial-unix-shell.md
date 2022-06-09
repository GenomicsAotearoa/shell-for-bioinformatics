## Remedial unix shell

From: Chapter 3 of *Bioinformatics Data Skills* by Vince Buffalo.

### Streams and redirection
 
 - `cat` for single and multiple files: uses example of `.fasta` files.
 - Introduces redirection via `>` and `>>`
 - Redirection of standard error with `>` and `2>` (and `2>>`)
 - Covers inout redirectuion via '<', but then goes straight into the pipe.

 ### Pipes

 - Uses a double `grep` example 
 - Combines redirection (`>`) and pipes (`|`)
 - Adds in `tee` to create intermediate file

 ### Processes

 - Background processes via `&`, plus `ctrl-z` and `fg`

 ### Command substitution

 - Kind of cool example of counting entries in `fasta` file: 
    ```
    grep -c '^>' input.fasta

    echo "There are $(grep -c '^>' input.fasta) entries in my fasta file."`
    ```
 - Should cover `^` and `&` for regex.

### Other stuff to recap?

 - Navigation and organisation: `cd`, `mkdir`, `rm`
 - `ls` and switches (plus combos): `-l`, `-h`, `-t`, `-S`, `-r` etc
 
