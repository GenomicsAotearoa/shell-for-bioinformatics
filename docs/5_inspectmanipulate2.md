# Inspecting and Manipulating Text Data with Unix Tools - Part 2

* Do not remove this line (it will not be displayed)
{:toc}

## Aho, Weinberger, Kernighan = AWK

Awk is a scripting language used for manipulating data and generating reports. The awk command programming language requires no compiling and allows the user to use variables, numeric functions, string functions, and logical operators. 

Awk is a utility that enables a programmer to write tiny but effective programs in the form of statements that define text patterns that are to be searched for in each line of a document and the action that is to be taken when a match is found within a line. Awk is mostly used for pattern scanning and processing. It searches one or more files to see if they contain lines that matches with the specified patterns and then perform the associated actions. 

WHAT CAN WE DO WITH AWK? 

1. AWK Operations: 
   - Scans a file line by line 
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

**Syntax**:

```bash
awk options 'selection_criteria {action}' input-file >  output-file
```
**Options** 

>>`-f program-file` : Reads the AWK program source from the file program-file, instead of from the first command line argument.
>>
>>`-F fs`            : Use fs for the input field separator

Default behaviour of `awk` is to print every line of data from the specified file. .i.e. mimics `cat`

```bash
$ awk '{print}' example.bed 
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
$ awk '/chr1/ {print}' example.bed 
chr1	26	39
chr1	32	47
chr1	40	49
chr1	9	28
chr1	10	19
```

`awk` can be used to mimic functionality of `cut` 

```bash
$ awk '{print $2 "\t" $3}' example.bed 
26	39
32	47
11	28
40	49
16	27
9	28
35	54
10	19
```
Here, we’re making use of Awk’s string concatenation. Two strings are concatenated if they are placed next to each other with no argument. So for each record, `$2"\t"$3` concatenates the second field, a tab character, and the third field. However, this is an instance where using `cut` is much simpler as the equivalent of above is `cut -f2,3 example.bed` 

Let’s look at how we can incorporate simple pattern matching. Suppose we wanted to write a filter that only output lines where the length of the feature (end
position - start position) was greater than 18. Awk supports arithmetic with the standard operators + , - , * , / , % (remainder), and ^ (exponentiation). We can subtract within a pattern to calculate the length of a feature, and filter on that expression:

```bash
$ awk '$3 - $2 > 18' example.bed 
chr1	9	28
chr2	35	54
```
---

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

---

We can also chain patterns, by using logical operators `&&` (AND), `||` (OR), and `!` (NOT). For example, if we wanted all lines on chromosome 1 with a length greater than 10:

```bash
$ awk '$1 ~ /chr1/ && $3 - $2 > 10' example.bed 
chr1	26	39
chr1	32	47
chr1	9	28
```

**Built-In Variables and special patterns In Awk**

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
$ awk 'NR >= 3 && NR <=5' example.bed 

    chr3	11	28
    chr1	40	49
    chr3	16	27
```

* suppose we wanted to calculate the mean feature length in example.bed. We would have to take the sum feature lengths, and then divide by the total number of records. We can do this with:

```bash
$ awk 'BEGIN{s = 0}; {s += ($3-$2)}; END{ print "mean: " s/NR};' example.bed 

  mean: 14
```
>>In this example, we’ve initialized a variable `s` to **0** in `BEGIN` (variables you define do not need a dollar sign). Then, for each record we increment s by the length of the feature. At the end of the records, we print this sum `s` divided by the number of records `NR` , giving the mean.

* `awk` makes it easy to convert between bioinformatics files like BED and GTF. For example, we could generate a three-column BED file from ***Mus_muscu‐
lus.GRCm38.75_chr1.gtf*** as follows:

```bash
$ awk '!/^#/ { print $1 "\t" $4-1 "\t" $5}' Mus_musculus.GRCm38.75_chr1.gtf | head -n 3

1	3054232	3054733
1	3054232	3054733
1	3054232	3054733
```
* `awk` also has a very useful data structure known as an associative array. Associative arrays behave like Python’s dictionaries or hashes in other languages. We can create an associative array by simply assigning a value to a key. For example, suppose we wanted to count the number of features (third column) belonging to the gene “Lypla1.” We could do this by incrementing their values in an associative array:

```bash
$ awk '/Lypla1/ {feature[$3] += 1}; END {for (k in feature) print k "\t" feature[k]}' Mus_musculus.GRCm38.75_chr1.gtf 

CDS	56
transcript	9
start_codon	5
gene	1
exon	69
UTR	24
```
- - - 
<p align="center"><b><a class="btn" href="https://genomicsaotearoa.github.io/bash-for-bioinformatics/" style="background: var(--bs-dark);font-weight:bold">Back to homepage</a></b></p>
