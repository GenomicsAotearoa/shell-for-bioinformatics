# Inspecting and Manipulating Text Data with Unix Tools - Part 1 
 
!!! abstract "Lesson Objectives"

Many formats in bioinformatics are simple tabular plain-text files delimited by a character. The most common tabular plain-text file format used in bioinformatics is tab-delimited. Bioinformatics evolved to favor tab-delimited formats because of the convenience of working with these files using Unix tools.

!!! info "Tabular Plain-Text Data Formats" 

    Tabular plain-text data formats are used extensively in computing. The basic format is
    incredibly simple: each row (also known as a record) is kept on its own line, and each
    column (also known as a field) is separated by some delimiter. There are three flavors
    you will encounter: tab-delimited, comma-separated, and variable space-delimited.

## Inspecting data with `head` and `tail`

Although `cat` command is an easy way for us to open and view the content of a file, it is not very practical to do so for a file with thousands of lines as it will exhaust the shell "space". Instead, large files should be inspected first and then manipulated accordingly. The first round of inspection can be done with `head` and `tail` command which prints the first 10 lines and the last 10 lines (`-n 10`) of a a file, respectively. Let's use `head` and `tail` to inspect *Mus_musculus.GRCm38.75_chr1.bed* 

```bash
head Mus_musculus.GRCm38.75_chr1.bed
```

??? success "Output"

    ```bash
    1	3054233	3054733
    1	3054233	3054733
    1	3054233	3054733
    1	3102016	3102125
    1	3102016	3102125
    1	3102016	3102125
    1	3205901	3671498
    1	3205901	3216344
    1	3213609	3216344
    1	3205901	3207317
    ```

```bash
tail Mus_musculus.GRCm38.75_chr1.bed 
```
??? success "Output"

    ```bash
    1	195166217	195166390
    1	195165745	195165851
    1	195165748	195165851
    1	195165745	195165747
    1	195228278	195228398
    1	195228278	195228398
    1	195228278	195228398
    1	195240910	195241007
    1	195240910	195241007
    1	195240910	195241007
    ```
Changing the number of lines printed for either of those commands can be done by passing `-n <number_of_lines>` flag .i.e. Over-ride the `-n 10` default

Try those commands with `0n 4` to print top 4 lines and bottom 4 lines

```bash
head -n 4 Mus_musculus.GRCm38.75_chr1.bed 
```
```bash
tail -n 4 Mus_musculus.GRCm38.75_chr1.bed 
```

???+ question "Exercise 4.1"

    Sometimes it’s useful to see both the beginning and end of a file — for example, if we have a sorted BED file and we want to see the positions of the first feature and last feature. Can you figure out a way to use both `head` and `tail` in a single command to inspect first and last 2 lines of ***Mus_musculus.GRCm38.75_chr1.bed***?

    ??? success "solution"
        ```bash
        (head -n 2; tail -n 2) < Mus_musculus.GRCm38.75_chr1.bed
        ```

???+ question "Exercise 4.2"

    We can also use tail to remove the header of a file. Normally the `-n` argument specifies how many of the last lines of a file to include, but if `-n` is given a number `x` preceded with a `+` sign (e.g., `+x` ), tail will start from the x<sup>th</sup> line. So to chop off a header, we start from the second line with `-n+2`.  
    Use the `seq` command to generate a file containing the numbers 1 to 10, and then use the `tail` command to chop off the first line.

    ??? success "solution"
        ```
        seq 10 > nums.txt
        ```
        ```bash
        cat nums.txt
        ```
        ```bash
        tail -n+2 nums.txt
        ```

## Extract summary information with `wc`

The "wc" in the `wc` command which stands for "word count" - this command can count the numbers of **words, lines**, and **characters** in a file (take a note on the order).

```bash
wc Mus_musculus.GRCm38.75_chr1.bed 

  81226  243678 1698545 Mus_musculus.GRCm38.75_chr1.bed
```
Often, we only need to list the number of lines, which can be done by using the `-l` flag. It can be used as a sanity check - for example, to make sure an output has the same number of lines as the input, OR to check that a certain file format which depends on another format without losing overall data structure wasn't corrupted or over/under manipulated. 

!!! question "Qu."

    Count the number of lines in *Mus_musculus.GRCm38.75_chr1.bed* and *Mus_musculus.GRCm38.75_chr1.gtf* . Anything out of the ordinary ? 

Although `wc -l` is the quickest way to count the number of lines in a file, it is not the most robust as it relies on the very bad assumption that "data is well formatted" 

For an example, if we are to create a file with 3 rows of data and then two empty lines by pressing **Enter** twice, 

```bash
cat > fool_wc.bed
```
>```bash
>  1 100
>  2 200
>  3 300
>```


**Ctrl+D** to end the edits started with `cat >`

```bash
wc -l fool_wc.bed 

  5 fool_wc.bed
```
This is a good place to bring in `grep` again which can be used to count the number of lines while excluding white-spaces (spaces, tabs (`t`) or newlines (`n`))

```bash

grep -c "[^ \n\t]" fool_wc.bed 

  3
```
## Using `cut` with column data and formatting tabular data with `column`

When working with plain-text tabular data formats like tab-delimited and CSV files, we often need to extract specific columns from the original file or stream. For example, suppose we wanted to extract only the start positions (the second column) of the ***Mus_musculus.GRCm38.75_chr1.bed*** file. The simplest way to do this is with `cut`.

```bash
cut -f 2 Mus_musculus.GRCm38.75_chr1.bed | head -n 3

3054233
3054233
3054233
```
>>`-f`  argument is how we specify which columns to keep. It can be used to specify a range as well

```bash
cut -f 2-3 Mus_musculus.GRCm38.75_chr1.bed | head -n 3

 3054233	3054733
 3054233	3054733
 3054233	3054733
```


Using `cut`, we can convert our GTF for ***Mus_musculus.GRCm38.75_chr1.gtf*** to a three-column tab-delimited file of genomic ranges (e.g., chromosome, start, and end position). We’ll chop off the metadata rows using the grep command covered earlier and then use cut to extract the first, fourth, and fifth columns (chromosome, start, end):

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | cut -f 1,4,5 | head -n 3
```
>```bash
>1	3054233	3054733
>1	3054233	3054733
>1	3054233	3054733
>```

Note that although our three-column file of genomic positions looks like a BED-formatted file, it’s not (due to subtle differences in genomic range formats).

`cut` also allows us to specify the column delimiter character. So, if we were to come across a CSV file containing chromosome names, start positions, and end positions, we could select columns from it, too:

As you may have noticed when working with tab-delimited files, it’s not always easy to see which elements belong to a particular column. For example:

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | cut -f 1-8 | head -n 3
```
>```bash    
>  1	pseudogene	gene	3054233	3054733	.	+	.
>  1	unprocessed_pseudogene	transcript	3054233	3054733	.	+	.
>  1	unprocessed_pseudogene	exon	3054233	3054733	.	+	.
>```

While tabs are a great delimiter in plain-text data files, our variable width data causes our columns to not stack up well. There’s a fix for this in Unix: program column `-t` (the `-t` option tells column to treat data as a table). The column -t command produces neat columns that are much easier to read:

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | cut -f 1-8 | column -t | head -n 3
```
>```bash
>   1  pseudogene                          gene         3054233    3054733    .  +  .
>   1  unprocessed_pseudogene              transcript   3054233    3054733    .  +  .
>   1  unprocessed_pseudogene              exon         3054233    3054733    .  +  .
>```

Note that you should only use `column -t` to visualize data in the terminal, not to reformat data to write to a file. Tab-delimited data is preferable to data delimited by a variable number of spaces, since it’s easier for programs to parse. Like `cut` , `column`’s default delimiter is the tab character (`\t` ). We can specify a different delimiter with the `-s` option. So, if we wanted to visualize the columns of the ***Mus_musculus.GRCm38.75_chr1_bed.csv*** file more easily, we could use:

```bash
column -s "," -t Mus_musculus.GRCm38.75_chr1.bed | head -n 3

    1	3054233	3054733
    1	3054233	3054733
    1	3054233	3054733
```

??? warning "Counting the number of columns of a *.gtf* with `grep` and `wc`"

    **GTF** Format has 9 columns 

    
    |1       |2      |3       |4     |5   |6     |7      |8     |9         |
    |:-------|:------|:-------|:-----|:---|:-----|:------|:-----|:---------|
    |seqname |source |feature |start |end |score |strand |frame |attribute |
    
    In theory, we should be able to use the following command to isolate the column headers and count the number
    
    ```bash
    grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | head -1 | wc -w
    ```
    However, this will return **16** and the problem is with the **attribute** column which is a *"A semicolon-separated list of tag-value pairs, providing additional information about each feature."*

    ```
    gene_id "ENSMUSG00000090025"; gene_name "Gm16088"; gene_source "havana"; gene_biotype "pseudogene";
    ```
    Therefore, we need to *translate** these values to a single "new line" with `tr` command and then count the number of lines than than the number of words
    
    ```bash
    grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | head -1 | tr '\t' '\n' | wc -l
    ```


## Sorting Plain-Text Data with `sort`

Very often we need to work with sorted plain-text data in bioinformatics. The two most common reasons to sort data are as follows:
- Certain operations are much more efficient when performed on sorted data.
- Sorting data is a prerequisite to finding all unique lines.

`sort`  is designed to work with plain-text data with columns. Create a test .bed file with few rows and use `sort` command without any arguments.

```bash
cat > test_sort.bed
```

??? abstract  "input"

    ```bash
    chr1	26	39
    chr1	32	47
    chr3	11	28
    chr1	40	49
    chr3	16	27
    chr1	9	28
    chr2	35	54
    chr1	10	19
    ```
```bash
sort test_sort.bed 
```
??? success "Output"

    ```bash
    chr1	10	19
    chr1	26	39
    chr1	32	47
    chr1	40	49
    chr1	9	28
    chr2	35	54
    chr3	11	28
    chr3	16	27
    ```
`sort` without any arguments simply sorts a file alphanumerically by line. Because chromosome is the first column, sorting by line effectively groups chromosomes together, as these are "ties" in the sorted order.

However, using `sort`’s default of sorting alphanumerically by line and doesn’t handle tabular data properly. There are two new features we need:

- The ability to sort by particular columns

- The ability to tell sort that certain columns are numeric values (and not alpha‐numeric text)

`sort` has a simple syntax to do this. Let’s look at how we’d sort example.bed by chromosome (first column), and start position (second column):

```bash
sort -k1,1 -k2,2n test_sort.bed
``` 
??? success "Output"

    ```bash
    chr1	9	28
    chr1	10	19
    chr1	26	39
    chr1	32	47
    chr1	40	49
    chr2	35	54
    chr3	11	28
    chr3	16	27
    ```

!!! quote ""

    Here, we specify the columns (and their order) we want to sort by as `-k` arguments. In technical terms, `-k` specifies the sorting keys and their order. Each `-k` argument takes a range of columns as `start,end`, so to sort by a single column we use `start,start`. In the preceding example, we first sorted by the first column (chromosome), as the first `-k` argument was `-k1,1` . Sorting by the first column alone leads to many ties in rows
    with the same chromosomes (e.g., “chr1” and “chr3”). Adding a second `-k` argument with a different column tells sort how to break these ties. In our example, `-k2,2n` tells sort to sort by the second column (start position), treating this column as numerical data (because there’s an `n` in `-k2,2n`).

    The end result is that rows are grouped by chromosome and sorted by start position.


??? question "Exercise 4.3"

    ***Mus_musculus.GRCm38.75_chr1_random.gtf*** file is ***Mus_musculus.GRCm38.75_chr1.gtf*** with permuted rows (and without a metadata
    header). Can you group rows by chromosome, and sort by position? If yes, append the output to a separate file.

    ??? success "solution"
        ```bash
        sort -k1,1 -k4,4n Mus_musculus.GRCm38.75_chr1_random.gtf > Mus_musculus.GRCm38.75_chr1_sorted.gtf
        ```

## Finding Unique Values with `uniq`

`uniq` takes lines from a file or standard input stream and outputs all lines with consecutive duplicates removed. While this is a relatively simple functionality, you will use `uniq` very frequently in command-line data processing.

```bash
cat letters.txt 
```

??? success "Output"

    ```bash
    A 
    A
    B
    C
    B
    C
    C
    C
    ```
```bash
uniq letters.txt 
```
```
A
B
C
B
C
```
As you can see, `uniq` does not return the unique values in letters.txt — it only removes consecutive duplicate lines (keeping one). If instead we did want to find all unique lines in a file, we would first sort all lines using `sort` so that all identical lines are grouped next to each other, and then run `uniq`.

```bash
sort letters.txt | uniq

A
B
C
```
`uniq` with `-c` shows the counts of occurrences next to the unique lines.

```bash
uniq -c letters.txt 

      2 A
      1 B
      1 C
      1 B
      3 C
```
```bash
sort letters.txt | uniq -c

      2 A
      2 B
      4 C
```
Combined with other Unix tools like `cut`, `grep` and `sort`, `uniq` can be used to summarize columns of tabular data:

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | cut -f3 | sort | uniq -c
```
??? success "Output"

    ```bash
    25901 CDS
    36128 exon
    2027 gene
    2290 start_codon
    2299 stop_codon
    4993 transcript
    7588 UTR
    ```
Count in order from most frequent to last

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | cut -f3 | sort | uniq -c | sort -rn

  36128 exon
  25901 CDS
   7588 UTR
   4993 transcript
   2299 stop_codon
   2290 start_codon
   2027 gene
```
!!! abstract ""
 
    * `n` and `r` represents *numerical sort* and *reverse* order (Or descending as the default as ascending) 

## sed

The `s`treamline `ed`itor or `sed` command is a stream editor that reads one or more text files, makes changes or edits according to editing script, and writes the results to standard output. First, we will discuss sed command with respect to search and replace function. 

### Find and Replace

Most common use of `sed` is to substitute text, matching a pattern. The syntax for doing this in `sed` is as follows:

```bash
sed 'OPERATION/REGEXP/REPLACEMENT/FLAGS' FILENAME
```

!!! info ""


    - Here, `/` is the delimiter (you can also use `_` (underscore), `|` (pipe) or `:` (colon) as delimiter as well)
    - `OPERATION` specifies the action to be performed (sometimes if a condition is satisfied). The most common and widely used operation is `s` which does the substitution operation (other useful operators include `y` for transformation, `i` for insertion, `d` for deletion etc.).
    - `REGEXP` and `REPLACEMENT` specify search term and the substitution term respectively for the operation that is being performed.
    - `FLAGS` are additional parameters that control the operation. Some common `FLAGS` include:
        - `g`	replace all the instances of `REGEXP` with `REPLACEMENT` (globally)
        - `N` where N is any number, to replace Nth instance of the `REGEXP` with `REPLACEMENT`
        - `p` if substitution was made, then prints the new pattern space
        - `i` ignores case for matching `REGEXP`
        - `w` file If substitution was made, write out the result to the given file
        - `d` when specified without `REPLACEMENT`, deletes the found `REGEXP`

* Some find and replace examples

Find and replace all `chr` to `chromosome` in the example.bed file and append the the edit to a new file names example_chromosome.bed

```bash
sed 's/chr/chromosome/g' example.bed > example_chromosome.bed
```
Find and replace `chr` to `chromosome`, only if you also find **40** in the line

```bash
sed '/40/s/chr/chromosome/g' example.bed > example_40.bed
```
Find and replace directly on the input, but save an old version too

```bash
sed -i.old 's/chr/chromosome/g' example.bed
```
!!! info ""
    `-i` to edit files in-place instead of printing to standard output

* Print specific lines of the file

To print a specific line you can use the address function. Note that by default, `sed` will stream the entire file, so when you are interested in specific lines only, you will have to suppress this feature using the option `-n`.

print 5th line of example.bed

```bash
sed -n '5p' example.bed
```

We can provide any number of additional lines to print using `-e` option. Let's print line 2 and 5, 

```bash
sed -n -e '2p' -e '5p' example.bed
```

It also accepts range, using `,`. Let's print line 2-6,

```bash
sed -n '2,6p' example.bed
```
Also, we can create specific pattern, like multiples of a number using `~`. Let's print every tenth line of Mus_musculus.GRCm38.75_chr1.bed starting from 10, 20, 30.. to end of the file

```bash
sed -n '10~10p' Mus_musculus.GRCm38.75_chr1.bed
```

???+ question "Exercise 4.4"

    Can you use the above `~` trick to extract all the **odd** numbered lines from Mus_musculus.GRCm38.75_chr1.bed and append the output to a new file **odd_sed.bed**

One of the powerful feature is that we can combine these ranges or multiples in any fashion. Example: fastq files have header on first line and sequence in second, next two lines will have the quality and a blank extra line (four lines make one read). Sometimes  we only need the sequence and header

```bash
sed -n '1~4p;2~4p' SRR097977.fastq
```
!!! hint "Sanity Check"

    It's not a bad practice validate some of these commands by comparing the output from another command. For an example, above `sed -n '1~4p;2~4p' SRR097977.fastq` should print exactly half the number of lines in the file as it is removing two lines per read. Do a quick sanity check with `sed -n '1~4p;2~4p' SRR097977.fastq  | wc -l` & `cat SRR097977.fastq | wc -l`


We can use the above trick to convert the .fastq to .fasta

```bash
sed -n '1~4p;2~4p' SRR097977.fastq  | sed 's/^@/>/g' > SRR097977.fasta
```
Let's wrap up `sed` with one more use case (a slightly complicated looking one). Let's say that we want capture all the transcript names from the last column (9th column) from .gtf file. We can write something similar to: 

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | head -n 3 | sed -E 's/.*transcript_id "([^"]+)".*/\1/'
```
!!! info ""
    `-E` option to enable POSIX Extended Regular Expressions (ERE)

Output is not really what we are after,

```bash
1	pseudogene	gene	3054233	3054733	.	+	.	gene_id "ENSMUSG00000090025"; gene_name "Gm16088"; gene_source "havana"; gene_biotype "pseudogene";
ENSMUST00000160944
ENSMUST00000160944
```

The is due to `sed` default behaviour where it prints every line, making replacements to matching lines. .i.e Some lines of the last column of Mus_musculus.GRCm38.75_chr1.gtf don't contain `transcript_id`. So, `sed` prints the entire line rather than captured group. One way to solve this would be to use `grep` `transcript_id` before sed to only work with lines containing the string `transcript_id` . However, `sed` offers a cleaner way. First, disable sed from outputting all lines with `-n` ( can use `--quiet` or `--silent` as well .i.e. *suppress automatic printing of pattern space*). Then, by appending `p` (*Print the current pattern space*) after the last slash `sed` will print all lines it’s made a replacement on. The following is an illustration of `-n` used with `p`:

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | head -n 3 | sed -E -n 's/.*transcript_id "([^"]+)".*/\1/p'
```
- - - 

!!! abstract "In preparation for next lesson, rename *example.bed.old* back to  *example.bed*"

    ```bash
    mv example.bed.old example.bed
    ```

<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/shell-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>