# Inspecting and Manipulating Text Data with Unix Tools

* Do not remove this line (it will not be displayed)
{:toc}


Many formats in bioinformatics are simple tabular plain-text files delimited by a character. The most common tabular plain-text file format used in bioinformatics is tab-delimited. Bioinformatics evolved to favor tab-delimited formats because of the convenience of working with these files using Unix tools.

>>**Tabular Plain-Text Data Formats** : 
Tabular plain-text data formats are used extensively in computing. The basic format is
incredibly simple: each row (also known as a record) is kept on its own line, and each
column (also known as a field) is separated by some delimiter. There are three flavors
you will encounter: tab-delimited, comma-separated, and variable space-delimited.

## Inspecting data with `head` and `tail`

Although `cat` command is an easy way for us to open and view the content of a file, it is not very practical to do so for a file with thousands of lines as it will exhaust the shell "space". Instead, large files should be inspected first and then manipulated accordingly. First round of inspection can be done with `head` and `tail` command which prints the first 10 lines and the last 10 lines (`-n 10`) of a a file, respectively. .i.e. Let's use `head` and `tail` to inspect *Mus_musculus.GRCm38.75_chr1.bed* 

```bash
$ head Mus_musculus.GRCm38.75_chr1.bed 
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
$ tail Mus_musculus.GRCm38.75_chr1.bed 
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
$ head -n 4 Mus_musculus.GRCm38.75_chr1.bed 
```
```bash
$ tail -n 4 Mus_musculus.GRCm38.75_chr1.bed 
```


### Exercise 4.1
{% capture e4dot1 %}

Sometimes it’s useful to see both the beginning and end of a file—for example, if we have a sorted BED file and we want to see the positions of the first feature and last feature. Can you figure out a way to use both `head` and `tail` on a single command to inspect first and last 2 lines of ***Mus_musculus.GRCm38.75_chr1.bed***

{% endcapture %}

{% include exercise.html title="e4dot1" content=e4dot1%}

### Exercise 4.2
{% capture e4dot2 %}

We can also use tail to remove the header of a file. Normally the -n argument specifies how many of the last lines of a file to include, but if -n is given a number x preceded with a + sign (e.g., +x ), tail will start from the x<sup>th</sup> line. So to chop off a header,we start from the second line with -n +2 . Here, we’ll use the command seq to generate a file of 3 numbers, and chop of the first line:

* Use `seq` command to generate a file of 10 number and then use `tail` command  to chop off the first line

{% endcapture %}

{% include exercise.html title="e4dot2" content=e4dot2%}

## Extract summary information with `wc` 

`wc` command which stands for "word count" can counts the number of **words, lines**, and **characters** in a file. (take a not on the order)

```bash
$ wc Mus_musculus.GRCm38.75_chr1.bed 

  81226  243678 1698545 Mus_musculus.GRCm38.75_chr1.bed
```
Often, we only need to call the number of lines which can be done by using `-l` flag .i.e. It can be used as a sanity check at times to make sure an output which should carry the same number of lines as per input OR a certain file format depends on another format without loosing overall data structure wasn't corrupted or over/under manipulated. 

>>**Question** - count the number of lines in *Mus_musculus.GRCm38.75_chr1.bed* and *Mus_musculus.GRCm38.75_chr1.gtf* . Anything out of the ordinary ? 

Although `wc -l` is the quickest way to count the number of lines in a file, it is not the most robust as it relies on the very bad assumption that "data is well formatted" 

For an example, If we are to create a file with 3 rows of data and then two empty lines, 

```bash
$ cat > fool_wc.bed
1 100
2 200
3 300


$
```
```bash
$ wc -l fool_wc.bed 
  5 fool_wc.bed
```
This is a good place bring in `grep` again which can be used to count the number of lines while excluding white-spaces (spaces, tabs or newlines)

```bash

$ grep -c "[^ \\n\\t]" fool_wc.bed 

  3
```
## Using  `cut` with column data and formatting tabular data with `column`
