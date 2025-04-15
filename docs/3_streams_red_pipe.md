# 2. Streams, Redirection and Pipe

!!! clipboard-list "Lesson Objectives"

    - To be able to redirect streams of data in Unix.
    - Solve problems by piping several Unix commands.
    - Command substitution 

Bioinformatics data is often text-based and large. This is why Unix’s philosophy of handling text streams is useful in bioinformatics: text streams allow us to do processing on a stream of data rather than holding it all in memory. Handling and redirecting the streams of data is an essential skill in Unix.

<figure markdown>
  ![image](images/redirection_st.png){ width="590" }
</figure>



By default, both standard error and standard output of most unix programs go to your terminal screen. We can change this behavior (redirect the streams to a file) by using `>` or `>>` operators. The operator `>` redirects standard output to a file and overwrites any existing contents of the file, whereas `>>` appends to the file. If there isn’t an existing file, both operators will create it before redirecting output to it. 

### Output redirection

The `shell4b_data` directory contains the following fasta files:

```
tb1.fasta
tb1-protein.fasta
tga1-protein.fasta
```

We can use the `cat` command to view these files either one at a time:

??? backward "Recap - `cat` command to view the content of *a* file"
    !!! terminal "code"
    
        ```bash
        cat tb1-protein.fasta
        ```
        ??? success "output"
    
            ```bash
            >teosinte-branched-1 protein
            LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
            FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
            GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
            FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
            GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
            VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
            NLSHHSSLSMNMPCAAA
            ```
    
    !!! terminal "code"
    
        ```bash
        cat tga1-protein.fasta 
        ```
        ??? success "output"
    
            ```bash
            >teosinte-glume-architecture-1 protein
            DSDCALSLLSAPANSSGIDVSRMVRPTEHVPMAQQPVVPGLQFGSASWFP
            RPQASTGGSFVPSCPAAVEGEQQLNAVLGPNDSEVSMNYGGMFHVGGGSG
            GGEGSSDGGT
            ```

OR all at once with `cat *.fasta`

We can also redirect the output to create a new file containing the sequence for both proteins:

!!! terminal "code"

    ```bash
    cat tb1-protein.fasta tga1-protein.fasta > zea-proteins.fasta
    ```

Now we have a new file called `zea-proteins.fasta`. Let's check the contents:

!!! terminal "code"

    ```bash
    cat zea-proteins.fasta
    ``` 
    ??? success "output"   
        ```bash
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

!!! terminal "code"

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

There are two different types of output there: <span style="color:blue">***standard output***</span> (the contents of the `tb1-protein.fasta` file) and <span style="color:red">***standard error***</span> (the error message relating to the missing `mik.fasta` file). If we use the `>` operator to redirect the output, the standard output is captured, but the standard error is not - it is still printed to the screen.  Let's check:

!!! terminal "code"
     ``` bash
     cat tb1-protein.fasta mik.fasta > test.fasta
     
       cat: mik.fasta: No such file or directory
     ```

The new file has been created and contains the standard output (contents of the file `tb1-protein.fasta`):

!!! terminal "code"
    ```bash
    cat test.fasta
    ```
    ??? success "output"

        ```bash
        >teosinte-branched-1 protein
        LGVPSVKHMFPFCDSSSPMDLPLYQQLQLSPSSPKTDQSSSFYCYPCSPP
        FAAADASFPLSYQIGSAAAADATPPQAVINSPDLPVQALMDHAPAPATEL
        GACASGAEGSGASLDRAAAAARKDRHSKICTAGGMRDRRMRLSLDVARKF
        FALQDMLGFDKASKTVQWLLNTSKSAIQEIMADDASSECVEDGSSSLSVD
        GKHNPAEQLGGGGDQKPKGNCRGEGKKPAKASKAAATPKPPRKSANNAHQ
        VPDKETRAKARERARERTKEKHRMRWVKLASAIDVEAAAASVPSDRPSSN
        NLSHHSSLSMNMPCAAA
        ```

If we want to capture the standard error we use the (slightly unweildy) `2>` operator:

!!! terminal "code"

    ```bash
    cat tb1-protein.fasta mik.fasta > test.fasta 2> stderror.txt
    ```

!!! user-secret "Descriptors"

    File descriptor `2` represents standard error (other special file descriptors include `0` for standard input and `1` for standard output).

Check the contents:

!!! terminal "code"
    
    ```bash
    cat stderror.txt
    
      cat: mik.fasta: No such file or directory
    ```
!!! warning "Reminder :`>` vs `>>`"

    Note that `>` will overwrite an existing file. We can use `>>` to add to a file instead of overwriting it:

!!! terminal "code"

    ```bash
    cat tga1-protein.fasta >> test.fasta
    ```
    ```bash
    cat test.fasta 
    ```
    ??? success "output"

        ```bash
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

!!! terminal "code"

    ```bash 
    grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"
    ```

!!! magnifying-glass "Let's see what each piece does" 

    `grep -v "^>" tb1.fasta`
 
    - `-v`: Inverts the match, selecting non-matching lines
    - `"^>"`: The pattern to match. ^ means start of line, > is the literal character

    `grep --color -i "[^ATCG]"`

    There are a few things going on here:

    - `--color`: tells `grep` to highlight any matches
    - `-i`: tells `grep` to ignore the case (i.e., will match lower or upper case)
    - `[^ATCG]`: when `^` is used *inside square brackets* it has a different function - *inverts* the pattern, so that `grep` finds any letters that are *not* A, T, C or G. In other words,  `[^...]` means "any character not in this set"

    ??? clipboard-question "Why is this useful"

        - Identifying non-standard nucleotides in a DNA sequence
        - Spotting potential sequencing errors
        - Quality control of DNA sequence data

Let's run the code:

!!! terminal "code"

    ```bash
    grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]"
    ```

    ??? success  "Output"

        CCCCAAAGACGGACCAATCCAGCAGCTTCTACTGCTA<span style="color:red">Y</span>CCATGCTCCCCTCCCTTCGCCGCCGCCGACGC


### Combining pipes and redirection

!!! terminal-2 "redirect the standard output of above `grep..` command to `non-atcg.txt`"

    ```bash
    grep -v "^>" tb1.fasta | grep --color -i "[^ATCG]" > non-atcg.txt
    ```
!!! terminal "code"

    ```bash
    cat non-atcg.txt 
    ```

since we are redirecting to a text file, the `--color` by itself will not record the colour information. We can achieve this by invoking `always` flag for `--color`.i.e..

!!! terminal "code"

    ```bash
    grep -v "^>" tb1.fasta | grep --color=always -i "[^ATCG]" > non-atcg.txt
    ```
### Using tee to capture intermediate outputs

!!! terminal "code"

    ```bash
    grep -v "^>" tb1.fasta | tee intermediate-out.txt | grep --color=always -i "[^ATCG]" > non-atcg.txt
    ```

The file `intermediate-out.txt` will contain the output from `grep -v "^>" tb1.fasta`, but `tee` also passes that output through the pipe to the next `grep` command.

??? truck-ramp "Preview - This is to be covered in "Advanced Shell for Bioinformatics"<br>*Pipes and Chains and Long running processes  : Exit Status (Programmatically Tell Whether Your Command Worked)*"


    How do you know when they complete? How do you know if they successfully finished without an error? Unix programs exit with an exit status, which indicates whether a program terminated without a problem or with an error. By Unix standards, an exit status of `0` indicates the process ran successfully, and any **nonzero** status indicates some sort of error has occurred (and hopefully the program prints an understandable error message, too). The exit status isn’t printed to the terminal, but your shell will set its value to a shell variable named   `$?`. We can use the `echo` command to look at this variable’s value after running a command:
    
    ```bash
    program input.txt > results.txt; echo $?
    ```
    Exit statuses are useful because they allow us to programmatically chain commands together in the shell. A subsequent command in a chain is run conditionally on the last command’s exit status. The shell provides two operators that implement this: one operator that runs the subsequent command only if the first command completed successfully (`&&`), and one operator that runs the next command only if the first completed unsuccessfully (`||`).
    
    For example, the sequence `program1 input.txt > intermediate-results.txt && program2 intermediate-results.txt > results.txt` will execute the second command only if previous commands have completed with a successful zero exit status.
    
    By contrast, `program1 input.txt > intermediate-results.txt || echo "warning: an error occurred"` will print the message if error has occurred.
    
    !!! quote "" 
        When a script ends with an **exit** that has no parameter, the exit status of the script is the exit status of the last command executed in the script (previous to the **exit**).
    
    !!! clipboard-question "Exit Status : using `&&` and `||`"
        To test your understanding of `&&` and `||`, we’ll use two Unix commands that do nothing but return either exit success (true) or exit failure (false). Predict and check the outcome of the following commands:
    
        ```bash
        true
        echo $?
        false
        echo $?
        true && echo "first command was a success"
        true || echo "first command was not a success" 
        false || echo "first command was not a success"
        false && echo "first command was a success"
        ```
        !!! tip "hint"
            The `$?` variable represents the exit status of the previous command.
    
        ??? success "Answer"
    
            ```bash
            ~$ true 
            ~$ echo $?
            0
            ~$ false
            ~$ echo $?
            1
            ~$ true && echo "first command was a success"
            first command was a success
    
            ~$ true || echo "first command was not a success"
            ~$ false || echo "first command was not a success"
            first command was not a success
    
            ~$ false && echo "first command was a success"
            ~$ 
            ```


### Command Substitution

Unix users like to have the Unix shell do the work for them. This is why shell expansions like wildcards and brace expansion exist. Another type of useful shell expansion is command substitution. Command substitution runs a Unix command inline and returns the output as a string that can be used in another command. This opens up a lot of useful possibilities. For example, if you want to include the results from executing a command into a text, you can type:

!!! terminal-2 "Which is better ?"
    ```bash
    grep -c '^@' SRR097977.fastq
    ```
    <center>**OR**</center>

    ```bash
    echo "There are $(grep -c '^@' SRR097977.fastq) entries in my FASTA file."
    ```
    !!! info ""

    - `echo`: This is a command that prints text to the standard output.
    - The text in quotes is what will be printed, with a substitution: "There are ... entries in my FASTA file."
    - `$(...)`: This is command substitution. It runs the command inside the parentheses and replaces itself with the output of that command.
    - `grep -c '^@' SRR097977.fastq`: This is the command inside the substitution:
        - `-c`: An option that tells grep to count matching lines instead of printing them
        - `'^@'`: The pattern to search for. In this case, it's looking for lines that start with '@'

    - So, this command will:
        - Count how many lines in SRR097977.fastq start with '@'
        - Substitute that number into the echo statement
        - Print the resulting message!

Another example of using command substitution would be creating dated directories:

!!! terminal "code"

    ```bash
    mkdir results-$(date +%F)
    ```
    !!! clipboard-question ""

        * `%F` - full date; same as `%Y-%m-%d`
        



