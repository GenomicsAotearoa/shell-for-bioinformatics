# 4. Streams, Redirection and Pipe

!!! abstract "Lesson Objectives"

    - To be able to redirect streams of data in Unix.
    - To be able to solve problems by piping several Unix commands.

Bioinformatics data is often text-based and large. This is why Unix’s philosophy of handling text streams is useful in bioinformatics: text streams allow us to do processing on a stream of data rather than holding it all in memory. Handling and redirecting the streams of data is essential skill in Unix.


By default, both standard error and standard output of most unix programs go to your terminal screen. We can change this behavior (redirect the streams to a file) by using `>` or >> operators. The operator > redirects standard output to a file and overwrites any existing contents of the file, whereas >> appends to the file. If there isn’t an existing file, both operators will create it before redirecting output to it. For example, to concatenate two FASTA files, we use cat command, but redirect the output to a file:

### Output redirection

The `shell4b_data` directory contains the following fasta files:

```
tb1.fasta
tb1-protein.fasta
tga1-protein.fasta
```

We can use the `cat` command to view these files either one at a time:

```bash
cat tb1-protein.fasta
```
>```bash
>>teosinte-branched-1 protein
>LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
>FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
>GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
>FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
>GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
>VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
>NLSHHSSLSMNMPCAAA
>```

```bash
cat tga1-protein.fasta 
```
>```bash
>>teosinte-glume-architecture-1 protein
>DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
>RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
>GGEGSSDGGT
>```

OR all at once with `cat *.fasta`

We can also redirect the output to create a new file containing the sequence for both proteins:

```
cat tb1-protein.fasta tga1-protein.fasta > zea-proteins.fasta
```

Now we have a new file called `zea-proteins.fasta`. Let's check the contents:

```bash
cat zea-proteins.fasta
``` 
>```bash
>>teosinte-branched-1 protein
>LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
>FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
>GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
>FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
>GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
>VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
>NLSHHSSLSMNMPCAAA
>>teosinte-glume-architecture-1 protein
>DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
>RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
>GGEGSSDGGT
>```

Capturing error messages

```bash
cat tb1-protein.fasta mik.fasta
```
>```bash
>>teosinte-branched-1 protein
>LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
>FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
>GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
>FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
>GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
>VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
>NLSHHSSLSMNMPCAAA
>cat: mik.fasta: No such file or directory
>```

There are two different types of output there: "standard output" (the contents of the `tb1-protein.fasta` file) and *standard error* (the error message relating to the missing `mik.fasta` file). If we use the `>` operator to redirect the output, the standard output is captured, but the standard error is not - it is still printed to the screen.  Let's check:

``` bash
cat tb1-protein.fasta mik.fasta > test.fasta

  cat: mik.fasta: No such file or directory
```

The new file has been created and contains the standard output (contents of the file `tb1-protein.fasta`):

```bash
cat test.fasta
```
>```bash
>>teosinte-branched-1 protein
>LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
>FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
>GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
>FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
>GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
>VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
>NLSHHSSLSMNMPCAAA
>```

If we want to capture the standard error we use the (slightly unweildy) `2>` operator:

```bash
cat tb1-protein.fasta mik.fasta > test.fasta 2> stderror.txt
```

!!! Descriptors

    File descriptor `2` represents standard error (other special file descriptors include `0` for standard input and `1` for standard output).

Check the contents:

```bash
cat stderror.txt

  cat: mik.fasta: No such file or directory
```

Note that `>` will overwrite an existing file. We can use `>>` to add to a file instead of overwriting it:

```bash
cat tga1-protein.fasta >> test.fasta
```
```bash
cat test.fasta 
```
>```bash
>>teosinte-branched-1 protein
>LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
>FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
>GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
>FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
>GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
>VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
>NLSHHSSLSMNMPCAAA
>>teosinte-glume-architecture-1 protein
>DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
>RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
>GGEGSSDGGT
>```

### The Unix pipe

The pipe operator (`|`) passes the output from one command to another command as input.  The following is an example of using a pipe with the `grep` command.

Steps:

1. Remove the header information for the sequence (line starts with ">")
2. Highlight any characters in the sequence that *are not* A, T, C or G.

We will use grep to carry out the first step, and then use the pipe operator to pass the output to a second grep command to carry out the second step.

Here is the full command:

```bash 
grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"
```

!!! info "Let's see what each piece does" 

    `grep -v "^>" tb1.fasta`
 
    The -v tells `grep` to search for all lines in the file `tb1.fasta` that *do not* contain a ">" at the start (`^` is a special character that denotes "at the start of the line - we'll learn more about this later).

    `grep --color -i "[^ATCG]"`

    There are a few things going on here:

    - `--color`: tells `grep` to highlight any matches
    - `-i`: tells `grep` to ignore the case (i.e., will match lower or upper case)
    - `[^ATCG]`: when `^` is used *inside square brackets* it has a different function - *inverts* the pattern, so that `grep` finds any letters that are *not* A, T, C or G.

Let's run the code:

```bash
grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"
```

!!! quote ""

    CCCCAAAGACGGACCAATCCAGCAGCTTCTACTGCTA<span style="color:red">Y</span>CCATGCTCCCCTCCCTTCGCCGCCGCCGACGC


What if we had just run the code for step 2 on the `tb1.fasta` file?

```
grep --color -i "[^ATCG]" tb1.fasta
```

### Combining pipes and redirection

```bash
grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]" > non-atcg.txt
```

```bash
cat non-atcg.txt 
```

since we are redirecting to a text file, the `--color` by itself will not record the colour information. We can achieve this by invoking `always` flag for `--color`.i.e..

```bash
grep -v "^>" tb1.fasta | grep --color=always -i "[^ATCG]" > non-atcg.txt
```
### Using tee to capture intermediate outputs

```bash
grep -v "^>" tb1.fasta | tee intermediate-file.txt | grep --color=always -i "[^ATCG]" > non-atcg.txt
```

The file `intermediate-file.txt` will contain the output from `grep -v "^>" tb1.fasta`, but `tee` also passes that output through the pipe to the next `grep` command.

### Pipes and Chains and Long running processes  : Exit Status (Programmatically Tell Whether Your Command Worked)

!!! info ""

    How do you know when they complete? How do you know if they successfully finished without an error? Unix programs exit with an exit status, which indicates whether a program terminated without a problem or with an error. By Unix standards, an exit status of 0 indicates the process ran successfully, and any nonzero status indicates some sort of error has occurred (and hopefully the program prints an understandable error message, too). The exit status isn’t printed to the terminal, but your shell will set its value to a shell variable named   `$?`. We can use the `echo` command to look at this variable’s value after running a command: 

    ```bash
    program input.txt > results.txt; echo $?
    ```
    By contrast, `program1 input.txt > intermediate-results.txt || echo "warning: an error occurred"` will print the message if error has occurred.

    !!! quote "" 

        When a script ends with an **exit** that has no parameter, the exit status of the script is the exit status of the last command executed in the script (previous to the **exit**).