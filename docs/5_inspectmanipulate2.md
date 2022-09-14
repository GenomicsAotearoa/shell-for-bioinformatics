# 6. Inspecting and Manipulating Text Data with Unix Tools - Part 2

!!! abstract "Lesson Objectives"
     
    * insertion, deletion, search and replace(substitution) with `sed`
    * Use specialised language `awk` to do a variety of text-processing tasks
    * Quick overview of `bioawk` (an extension of `awk` to process common biological data formats)

<center>
![image](./images/awk_image.png){width="200"}
</center>

## sed

The `s`treamline `ed`itor or `sed` command is a stream editor that reads one or more text files, makes changes or edits according to editing script, and writes the results to standard output. First, we will discuss sed command with respect to search and replace function. 

### Find and Replace

Most common use of `sed` is to substitute text, matching a pattern. The syntax for doing this in `sed` is as follows:

```bash
sed 'OPERATION/REGEXP/REPLACEMENT/FLAGS' FILENAME
```

!!! info ""


    - Here, `/` is the delimiter (you can also use `_` (underscore), `|` (pipe) or `:` (colon) as delimiter as well)
    - `OPERATION` specifies the action to be performed (sometimes if a condition is satisfied). 
         - The most common and widely used operation is `s` which does the **substitution** operation 
         - Other useful operators include `y` for **transformation**, `i` for **insertion**, `d` for **deletion** etc.).
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

To print a specific line you can use the address function. Note that by default, `sed` will stream the entire file, so when you are interested in specific lines only, you will have to suppress this feature using the option `-n` 

!!! info ""
    `-n`, `--quiet`, `--silent` = suppress automatic printing of pattern space

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

    ??? pied-piper "POSIX Regular and Exetended Regular Expressions"

         **POSIX Basic Regular Expressions** 

         * POSIX or “Portable Operating System Interface for uniX” is a collection of standards that define some of the functionality that a (UNIX) operating system should support. One of these standards defines two flavors of regular expressions. Commands involving regular expressions, such as grep and egrep, implement these flavors on POSIX-compliant UNIX systems. Several database systems also use POSIX regular expressions.
        
            The Basic Regular Expressions or BRE flavor standardizes a flavor similar to the one used by the traditional UNIX grep command. This is pretty much the oldest regular expression flavor still in use today. One thing that sets this flavor apart is that most meta-characters require a backslash to give the metacharacter its flavor. Most other flavors, including POSIX ERE, use a backslash to suppress the meaning of metacharacters. Using a backslash to escape a character that is never a metacharacter is an error.

        **POSIX Extended Regular Expressions**

        * The Extended Regular Expressions or ERE flavor standardizes a flavor similar to the one used by the UNIX egrep command. “Extended” is relative to the original UNIX grep, which only had bracket expressions, dot, caret, dollar and star. An ERE support these just like a BRE. Most modern regex flavors are extensions of the ERE flavor. By today’s standard, the POSIX ERE flavor is rather bare bones. The POSIX standard was defined in 1986, and regular expressions have come a long way since then.

            The developers of egrep did not try to maintain compatibility with grep, creating a separate tool instead. Thus egrep, and POSIX ERE, add additional metacharacters without backslashes. You can use backslashes to suppress the meaning of all metacharacters, just like in modern regex flavors. Escaping a character that is not a meta-character is an error.


Output is not really what we are after,

>```bash
>1	pseudogene	gene	3054233	3054733	.	+	.	gene_id "ENSMUSG00000090025"; gene_name "Gm16088"; gene_source "havana"; gene_biotype "pseudogene";
>ENSMUST00000160944
>ENSMUST00000160944
>```

The is due to `sed` default behaviour where it prints every line, making replacements to matching lines. .i.e Some lines of the last column of Mus_musculus.GRCm38.75_chr1.gtf don't contain `transcript_id`. So, `sed` prints the entire line rather than captured group. One way to solve this would be to use `grep` `transcript_id` before sed to only work with lines containing the string `transcript_id` . However, `sed` offers a cleaner way. First, disable sed from outputting all lines with `-n` ( can use `--quiet` or `--silent` as well .i.e. *suppress automatic printing of pattern space*). Then, by appending `p` (*Print the current pattern space*) after the last slash `sed` will print all lines it’s made a replacement on. The following is an illustration of `-n` used with `p`:

```bash
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf | head -n 3 | sed -E -n 's/.*transcript_id "([^"]+)".*/\1/p'
```
- - - 

!!! abstract "In preparation for next lesson, rename *example.bed.old* back to  *example.bed*"

    ```bash
    mv example.bed.old example.bed
    ```

## Aho, Weinberger, Kernighan = AWK

Awk is a scripting language used for manipulating data and generating reports. The awk command programming language requires no compiling and allows the user to use variables, numeric functions, string functions, and logical operators. 

Awk is a utility that enables a programmer to write tiny but effective programs. These take the form of statements that define text patterns that are to be searched for in each line of a document, and the action that is to be taken when a match is found within a line. Awk is mostly used for pattern scanning and processing. It searches one or more files to see if they contain lines that match with the specified patterns and then perform the associated actions. 

???+ "WHAT CAN WE DO WITH AWK?" 

      1. AWK Operations: 
         -  Scans a file line by line 
         -  Splits each input line into fields 
         -  Compares input line/fields to pattern 
         -  Performs action(s) on matched lines 

      2. Useful For: 
         -  Transform data files 
         -  Produce formatted reports 

      3. Programming Constructs: 
         - Format output lines 
         - Arithmetic and string operations 
         - Conditionals and loops 

!!! quote ""

    There are two key parts for understanding the Awk language: how Awk processes records, and pattern-action pairs. The rest of the language is quite simple.

    * Awk processes input data a record (line) at a time. Each record is composed of fields (column entries) that Awk automatically separates. Awk assigns the entire record to the variable $0, field one’s value to $1, field two’s value to $2, etc.

    * We build Awk programs using one or more of the following structures: `pattern { action }`. Each pattern is an expression or regular expression pattern. In Awk lingo, these are pattern-action pairs and we can chain multiple pattern-action pairs together (separated by semicolons). If we omit the pattern, Awk will run the action on all records. If we omit the action but specify a pattern, Awk will print all records that match the pattern.



**Syntax**:

```bash
awk options 'selection_criteria {action}' input-file >  output-file
```
!!! Options 

      `-f program-file` : Reads the AWK program source from the file program-file, instead of from the first command line argument.

      `-F fs`            : Use fs for the input field separator

Default behaviour of `awk` is to print every line of data from the specified file. .i.e. mimics `cat`


```bash
awk '{print}' example.bed 
```
??? success "Output"

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
Print lines which match the given pattern

```bash
awk '/chr1/{print}' example.bed
```
>```bash
>chr1	26	39
>chr1	32	47
>chr1	40	49
>chr1	9	28
>chr1	10	19
>```

`awk` can be used to mimic functionality of `cut` 

```bash
awk '{print $2 "\t" $3}' example.bed 
```

??? success "Output"

    ```bash
    26	39
    32	47
    11	28
    40	49
    16	27
     9	28
    35	54
    10	19
    ```
Here, we’re making use of Awk’s string concatenation. Two strings are concatenated if they are placed next to each other with no argument. So for each record, `$2"\t"$3` concatenates the second field, a tab character, and the third field. However, this is an instance where using `cut` is much simpler as the equivalent of above is `cut -f 2,3 example.bed` 

Let’s look at how we can incorporate simple pattern matching. Suppose we wanted to write a filter that only output lines where the length of the feature (end
position - start position) was greater than 18. Awk supports arithmetic with the standard operators + , - , * , / , % (remainder), and ^ (exponentiation). We can subtract within a pattern to calculate the length of a feature, and filter on that expression:

```bash
awk '$3 - $2 > 18' example.bed
``` 
>```bash
>chr1	9	28
>chr2	35	54
>```

- - -

??? info "`awk` Comparison and Logical operations"

    |Comparison |  Description                                |
    |:----------|:--------------------------------------------|
    |`a == b`     |a is equal to b                              |
    |`a != b`     |a is not equal to b                          |
    |`a < b`      |a is less than b                             |
    |`a > b`      |a is greater than b                          |
    |`a <= b`     |a is less than or equal to b                 |
    |`a >= b`     |a is greater than or equal to b              |
    |`a ~ b`      |a matches regular expression pattern b       |
    |`a !~ b`     |a does not match regular expression pattern b|
    |`a && b`     |logical a and b                              |
    |`a \|\| b`     |logical or a and b                           |
    |`!a`         |not a (logical negation)                     |


- - -

We can also chain patterns, by using logical operators `&&` (AND), `||` (OR), and `!` (NOT). For example, if we wanted all lines on chromosome 1 with a length greater than 10:

```bash
awk '$1 ~ /chr1/ && $3 - $2 > 10' example.bed 
```
>```bash
>chr1	26	39
>chr1	32	47
>chr1	9	28
>```
!!! info ""

    - First pattern, `$1 ~ /chr1` specifies the regular expression (All Regular expressions are in slashes).  We are matching the first field, `$1` against the regular expression `chr1`. 
    - Tilde `~` means **match**.
    - To do the inverse of **match**, we can use `!~` OR `!($1 ~ /chr1/`



!!! note "Built-In Variables and special patterns In Awk"

    * Awk’s built-in variables include the field variables `$1`, `$2`, `$3`, and so on (`$0` is the entire line) — that break a line of text into individual words or pieces called fields. 

        - `NR`: keeps a current count of the number of input records. Remember that records are usually lines. Awk command performs the pattern/action statements once for each record in a file. 
        - `NF`: keeps a count of the number of fields within the current input record. 
        - `FS`: contains the field separator character which is used to divide fields on the input line. The default is “white space”, meaning space and tab characters. FS can be reassigned to another character (typically in BEGIN) to change the field separator. 
        - `RS`: stores the current record separator character. Since, by default, an input line is the input record, the default record separator character is a newline. 
        - `OFS`: stores the output field separator, which separates the fields when Awk prints them. The default is a blank space. Whenever print has several parameters separated with commas, it will print the value of OFS in between each parameter. 
        - `ORS`: stores the output record separator, which separates the output lines when Awk prints them. The default is a newline character. print automatically outputs the contents of ORS at the end of whatever it is given to print. 

    * Also, there are two special patterns `BEGIN` & `END`

        - `BEGIN` - specifies what to do before the first record is read in. Useful to initialise and set up variables
        - `END` - what to do after the last record's processing is complete. Useful to print data summaries ad the end of file processing


**Examples**

* We can use `NR` to extract ranges of lines, too; for example, if we wanted to extract all lines between 3 and 5 (inclusive):

```bash
awk 'NR >= 3 && NR <=5' example.bed
``` 
>```bash
>    chr3	11	28
>    chr1	40	49
>    chr3	16	27
>```

* suppose we wanted to calculate the mean feature length in example.bed. We would have to take the sum feature lengths, and then divide by the total number of records. We can do this with:

```bash
awk 'BEGIN{s = 0}; {s += ($3-$2)}; END{ print "mean: " s/NR};' example.bed 

  mean: 14
```

!!! info 

      In this example, we’ve initialized a variable `s` to **0** in `BEGIN` (variables you define do not need a dollar sign). Then, for each record we increment `s` by the length of the feature. At the end of the records, we print this sum `s` divided by the number of records `NR` , giving the mean.

* `awk` makes it easy to convert between bioinformatics files like BED and GTF. For example, we could generate a three-column BED file from ***Mus_muscu‐lus.GRCm38.75_chr1.gtf*** as follows:

```bash
awk '!/^#/ { print $1 "\t" $4-1 "\t" $5}' Mus_musculus.GRCm38.75_chr1.gtf | head -n 3
```
>```bash
>1	3054232	3054733
>1	3054232	3054733
>1	3054232	3054733
>```

!!! danger "Note"
    Note that we subtract 1 from the start position to convert to **BED** format. This is because **BED** uses zero-indexing while GTF uses 1-indexing; This is a subtle detail, certainly one that’s been missed many times. In the midst of an analysis, it’s easy to miss these small details.


* `awk` also has a very useful data structure known as an associative array. Associative arrays behave like Python’s dictionaries or hashes in other languages. We can create an associative array by simply assigning a value to a key. For example, suppose we wanted to count the number of features (third column) belonging to the gene “Lypla1.” We could do this by incrementing their values in an associative array:

```bash
awk '/Lypla1/ {feature[$3] += 1}; END {for (k in feature) print k "\t" feature[k]}' Mus_musculus.GRCm38.75_chr1.gtf 
```
>```bash
>exon	69
>CDS	56
>UTR	24
>gene	1
>start_codon	5
>stop_codon	5
>transcript	9
>```

It’s worth noting that there’s an entirely Unix way to count features of a particular gene: `grep` , `cut` , `sort` , and `uniq -c`

```bash
grep "Lypla1" Mus_musculus.GRCm38.75_chr1.gtf | cut -f 3 | sort | uniq -c
```
However, if we needed to also filter on column-specific information (e.g., strand), an approach using just base Unix tools would be quite messy. With Awk, adding an additional filter would be trivial: we’d just use `&&` to add another expression in the pattern.

## `bioawk` 

`bioawk` is an extension of `awk`, adding the support of several common biological data formats, including optionally gzip'ed BED, GFF, SAM, VCF, FASTA/Q and TAB-delimited formats with column names. It also adds a few built-in functions and a command line option to use TAB as the input/output delimiter. When the new functionality is not used, `bioawk` is intended to behave exactly the same as the original BWK awk.

The original `awk` requires a YACC-compatible parser generator (e.g. Byacc or Bison). `bioawk` further depends on [zlib](http://zlib.net/) so as to work with gzip'd files.

??? "YACC" 

      A parser generator is a program that takes as input a specification of a syntax, and produces as output a procedure for recognizing that language. Historically, they are also called compiler-compilers.
      YACC (yet another compiler-compiler) is an LALR(LookAhead, Left-to-right, Rightmost derivation producer with 1 lookahead token) parser generator. YACC was originally designed for being complemented by **Lex**.

      - **Lex** (A Lexical Analyzer Generator) helps write programs whose control flow is directed by instances of regular expressions in the input stream. It is well suited for editor-script type transformations and for segmenting input in preparation for a parsing routine. 

!!! info "`bioawk` features"

      - It can automatically recognize some popular formats and will parse different features associated with those formats. The format option is passed to `bioawk` using `-c arg` flag. Here `arg` can be bed, sam, vcf, gff or fastx (for both fastq and FASTA). It can also deal with other types of table formats using the `-c header` option. When `header` is specified, the field names will used for variable names, thus greatly expanding the utility.`
      - There are several built-in functions (other than the standard `awk` built-ins), that are specific to biological file formats. When a format is read with `bioawk`, the fields get automatically parsed. You can apply several functions on these variables to get the desired output. Let’s say, we read fasta format, now we have `$name` and `$seq` that holds sequence name and sequence respectively. You can use the `print` function (`awk` built-in) to print `$name` and `$seq`. You can also use `bioawk` built-in with the `print` function to get length, reverse complement etc by using `'{print length($seq)}'`. Other functions include `reverse`, `revcomp`, `trimq`, `and`, `or`, `xor` etc.

??? "Variables for each format"

    For the `-c` you can either specify bed, sam, vcf, gff, fastx or header. `bioawk` will parse these variables for the respective format. If `-c` header is specified, the field names (first line) will be used as variables (spaces and special characters will be changed to under_score).

    <center>

    |**bed** 	|**sam** 	|**vcf** 	|**gff** 	|**fastx**|
    |:----------|:----------|:----------|:----------|:--------|
    |chrom 	|qname 	|chrom 	|seqname 	|name     |
    |start 	|flag 	|pos 	      |source 	|seq      |
    |end 	      |rname 	|id 	      |feature 	|qual     |
    |name 	|pos 	      |ref 	      |start 	|comment  | 
    |score 	|mapq 	|alt 	      |end 	      |         |
    |strand 	|cigar 	|qual       |score 	|         |
    |thickstart |rnext 	|filter 	|filter 	|         |
    |thickend 	|pnext 	|info 	|strand 	|         |
    |rgb 	      |tlen 	|group 	| 	      |         |
    |blockcount |seq 	      |attribute 	|  	      |         |
    |blocksizes |qual 	| 	  	|           |         |
    |blockstarts| 	  	|           |           |         |

    </center>

**bioawk** is not a default linux/unix utility. .i.e. Has to be installed. This is available as a module on NeSI HPC platforms which can be loaded with 

```bash
module load bioawk/1.0
```
 The basic idea of Bioawk is that we specify what bioinformatics format we’re working with, and Bioawk will automatically set variables for each field (just as regular Awk sets the columns of a tabular text file to $1, $1, $2, etc.). For Bioawk to set these fields, specify the format of the input file or stream with -c. Let’s look at Bioawk’s supported input formats and what variables these formats set:

```bash
bioawk -c help bed
```
>```bash
>ed:
>	1:chrom 2:start 3:end 4:name 5:score 6:strand 7:thickstart 8:thickend 9:rgb 10:blockcount 11:blocksizes 12:blockstarts 
>sam:
>	1:qname 2:flag 3:rname 4:pos 5:mapq 6:cigar 7:rnext 8:pnext 9:tlen 10:seq 11:qual 
>vcf:
>	1:chrom 2:pos 3:id 4:ref 5:alt 6:qual 7:filter 8:info 
>gff:
>	1:seqname 2:source 3:feature 4:start 5:end 6:score 7:filter 8:strand 9:group 10:attribute 
>fastx:
>	1:name 2:seq 3:qual 4:comment 
>```

As an example of how this works, let’s read in example.bed and append a column with the length of the feature (end position - start position) for all protein coding genes:

```bash
bioawk -c gff '$3 ~ /gene/ && $2 ~ /protein_coding/ {print $seqname,$end-$start}' ../data/Mus_musculus.GRCm38.75_chr1.gtf | head -n 4
```
>```
>1	465597
>1	16807
>1	5485
>1	12533
>```

Bioawk is also quite useful for processing FASTA/FASTQ files. For example, we could use it to turn a FASTQ file into a FASTA file:

```bash
bioawk -c fastx '{print ">"$name"\n"$seq}' SRR097977.fastq | head -n 4
```
>```
>>SRR097977.1
>TATTCTGCCATAATGAAATTCGCCACTTGTTAGTGT
>>SRR097977.2
>GGTTACTCTTTTAACCTTGATGTTTCGACGCTGTAT
>```

!!! info ""
    Note that Bioawk detects whether to parse input as FASTQ or FASTA when we use `-c fastx`.


Bioawk can also serve as a method of counting the number of FASTQ/FASTA entries:

```bash
bioawk -c fastx 'END{print NR}' SRR097977.fastq 
```
Bioawk’s function `revcomp()` can be used to reverse complement a sequence:

```bash
bioawk -c fastx '{print ">"$name"\n"revcomp($seq)}' SRR097977.fastq | head -n 4
```
>```
>>SRR097977.1
>ACACTAACAAGTGGCGAATTTCATTATGGCAGAATA
>>SRR097977.2
>ATACAGCGTCGAAACATCAAGGTTAAAAGAGTAACC
>```


- - - 
<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/shell-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>