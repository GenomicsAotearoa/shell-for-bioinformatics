# Inspecting and Manipulating Text Data with Unix Tools - Part 2

* Do not remove this line (it will not be displayed)
{:toc}

## Aho, Weinberger,Kernighan = `awk` 

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
|a == b     |a is equal to b                              |
|a != b     |a is not equal to b                          |
|a < b      |a is less than b                             |
|a > b      |a is greater than b                          |
|a <= b     |a is less than or equal to b                 |
|a >= b     |a is greater than or equal to b              |
|a ~ b      |a matches regular expression pattern b       |
|a !~ b     |a does not match regular expression pattern b|
|a && b     |logical and a and b                          |
|a || b     |logical or a and b                           |
|!a         |not a (logical negation)                     |
---

We can also chain patterns, by using logical operators `&&` (AND), `||` (OR), and `!` (NOT). For example, if we wanted all lines on chromosome 1 with a length greater than 10:

```bash
$ awk '$1 ~ /chr1/ && $3 - $2 > 10' example.bed 
chr1	26	39
chr1	32	47
chr1	9	28
```
