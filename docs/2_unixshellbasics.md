# Unix Shell Basics & Recap

!!! abstract "Lesson 

It is expected that you are already familiar with using the basics of the Unix Shell.  As a quick refresher, some frequently used commands are listed below.

For more information about a command, use the Unix `man` command. For example, to get more information about the `mkdir` command, type:

```bash
$ man mkdir
```



!!! note "Key commands for navigating around the filesystem are:"

    - `ls` - list the contents of the current directory
    - `ls -l` - list the contents of the current directory in more detail.
    - `pwd` - show the location of the current directory
    - `cd DIR` - change directory to directory DIR (DIR must be in your current directory - you should see its name when you type `ls` OR you need to specify either a full or relative path to DIR)
    - `cd -` - change back to the last directory you were in.
    - `cd` (also `cd ~/`)- change to your home directory
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


### Output redirection

The `data_shell4b` directory contains the following fasta files:

```
tb1.fasta
tb1-protein.fasta
tga1-protein.fasta
```

We can use the `cat` command to view these files either one at a time:

```bash
$ cat tb1-protein.fasta

>teosinte-branched-1 protein
LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
NLSHHSSLSMNMPCAAA
```
```bash
$ cat tga1-protein.fasta 

>teosinte-glume-architecture-1 protein
DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
GGEGSSDGGT
```
OR all at once with `cat *.fasta`

We can also redirect the output to create a new file containing the sequence for both proteins:

```
$ cat tb1-protein.fasta tga1-protein.fasta > zea-proteins.fasta
```

Now we have a new file called `zea-proteins.fasta`. Let's check the contents:

```bash
$ cat zea-proteins.fasta 

>teosinte-branched-1 protein
LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
NLSHHSSLSMNMPCAAA
>teosinte-glume-architecture-1 protein
DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
GGEGSSDGGT
```

Capturing error messages

```bash
$ cat tb1-protein.fasta mik.fasta

>teosinte-branched-1 protein
LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
NLSHHSSLSMNMPCAAA
cat: mik.fasta: No such file or directory
```

There are two different types of output there: "standard outout" (the contents of the `tb1-protein.fasta` file) and *standard error* (the error message relatign to the missign `mik.fasta` file). If we use the `>` operator to redirect the outout, the standard output is catured, bu the standard error is not - it is still printed to the screen.  Let's check:

``` bash
$ cat tb1-protein.fasta mik.fasta > test.fasta

  cat: mik.fasta: No such file or directory
```

The new file has been created, and contans the standard outout (contents of the file `tb1-protein.fasta`):

```bash
$ cat test.fasta

>teosinte-branched-1 protein
LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
NLSHHSSLSMNMPCAAA
```

If we want to capture the standard error, we use the (slightly unweildy) `2>` operator:

```bash
$ cat tb1-protein.fasta mik.fasta > test.fasta 2> stderror.txt
```

!!! Descriptors

    File descriptor `2` represents standard error. (other special file descriptors include `0` for standard input and `1` for standard output).

Check the contents:

```bash
$ cat stderror.txt

  cat: mik.fasta: No such file or directory
```

Note that `>` will overwrite an existing file. We can use `>>` to add to a file instead of overwriting it:

```bash
$ cat tga1-protein.fasta >> test.fasta

$ cat test.fasta 

>teosinte-branched-1 protein
LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
NLSHHSSLSMNMPCAAA
>teosinte-glume-architecture-1 protein
DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
GGEGSSDGGT
```

### The Unix pipe

The pipe operator (`|`) passes the output from one command to another command as input.  The following is an example of using a pipe with the `grep` command.

Steps:

1. Remove the header information for the sequence (line starts with ">")
2. Highlight any characters in the sequence that *are not* A, T, C or G.

We will use grep to carry out the first step, and then use the pipe operator to pass the output to a second grep command to carry out the second step.

Here is the full command:

```bash 
$ grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"
```

!!! info "Let's see what each piece does" 

    `grep -v "^>" tb1.fasta`
 
    This tells `grep` to search for all lines in the file `tb1.fasta`  that *do not* contain a ">" at the start (`^` is a special character that denotes "at the start of the line - we'll learn more about this later).

    `grep --color -i "[^ATCG]"`

    There are a few things going on here:

    - `--color`: tells `grep` to highlight any matches
    - `-i`: tells `grep` to ignore the case (i.e., will match lower or upper case)
    - `[^ATCG]`: when `^` is used *inside square brackets* it has a different function - *inverts* the pattern, so that `grep` finds any letters that are *not* A, T, C or G.

Let's run the code:

```bash
$ grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"

  CCCCAAAGACGGACCAATCCAGCAGCTTCTACTGCTAYCCATGCTCCCCTCCCTTCGCCGCCGCCGACGC
```

**NB: colours not supported in Markdown (I tried using HTML)**

What if we had just run the code for step 2 on the `tb1.fasta` file?

```
$ grep --color -i "[^ATCG]" tb1.fasta
```

### Combining pipes and redirection

```bash
$ grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]" > non-atcg.txt
```

```bash
$ cat non-atcg.txt 
```

since we are redirecting to a text file, the `--color` by itself will not record the colour information. We can achieve this by invoking `always` flag for `--color`.i.e..

```bash
$ grep -v "^>" tb1.fasta | grep --color=always -i "[^ATCG]" > non-atcg.txt
```
### Using tee to capture intermediate outputs

```bash
$ grep -v "^>" tb1.fasta | tee intermediate-file.txt | grep --color=always -i "[^ATCG]" > non-atcg.txt
```

The file `intermediate-file.txt` will contain the output from `grep -v "^>" tb1.fasta`, but `tee` also passes that output through the pipe to the next `grep` command.


- - - 

<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/shell-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>