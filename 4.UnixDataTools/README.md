## Unix Data Tools

### `head` and `tail` for inspecting text files

Use `head` to view the first few lines (default is 10 lines) of the file `Mus_musculus.GRCm38.75_chr1.bed`

```
head Mus_musculus.GRCm38.75_chr1.bed
```

```
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

We can choose the number of lines to display using the `-n` switch:

```
head -n 3 Mus_musculus.GRCm38.75_chr1.bed
```

```
1	3054233	3054733
1	3054233	3054733
1	3054233	3054733
```

Use `tail` to view the last three lines of the file `Mus_musculus.GRCm38.75_chr1.bed`:

```
tail -n 3 Mus_musculus.GRCm38.75_chr1.bed
```

```
1	195240910	195241007
1	195240910	195241007
1	195240910	195241007
```

View the start *and* the end of a file with a single command:

```
(head -n 2; tail -n 2) < Mus_musculus.GRCm38.75_chr1.bed
```

```
1	3054233	3054733
1	3054233	3054733
1	195240910	195241007
1	195240910	195241007
```

Controlling output with `head` (and a pipe):

```
grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf | head -n 1
```

```
1	protein_coding	gene	6206197	6276648	.	+	.	gene_id "ENSMUSG00000025907"; gene_name "Rb1cc1"; gene_source "ensembl_havana"; gene_biotype "protein_coding";
```

Note that using `head` speeds things up, as (in the above example), the `grep` command can stop as soon as `head` has one line of output.

It takes longer if you run the command without `head` (use `time` to check):

```
time grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf
```

```
grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf  0.56s user 0.02s system 94% cpu 0.611 total
```

With `head`:

```
time (grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf | head -n 1)
```

```
( grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf | head )  0.01s user 0.01s system 77% cpu 0.021 total
```

Definitely good to remember if you are testing things out when you are creating a workflow.

### `less`(and `more`)

`less` and `more` (stick with `less`, it has *more* features)

Interactive program for inspecting files.

```
less Mus_musculus.GRCm38.75_chr1.gtf
```

Type `q` to exit.

Other shortcuts:

| Shortcut | Action|
|----------|-------|
| space | Next page |
| b | Previous page |
| g | First line | 
| G | Last line | 
| j | Down (one line) | 
| k | Up (one line) | 
| /<pattern> | Search down (forward) for string <pattern>|
| ?<pattern> | Search up (backward) for string <pattern> |
| n | Repeat last search downward (forward) | 
| N | Repeat last search upward (backward) | 


Try searching for the gene `Sgk3` in the file `Mus_musculus.GRCm38.75_chr1.gtf` using `less`.


### Summary information for text files

The `wc` command returns word, line, character and byte counts for a file.

```
wc Mus_musculus.GRCm38.75_chr1.bed
```

```
81226  243678 1698545 Mus_musculus.GRCm38.75_chr1.bed
```

Can select which output you want using: 

 - `-c`: bytes
 - `-l`: lines
 - `-m`: characters
 - `-w`: words

For example, to check how many lines there are in the file `Mus_musculus.GRCm38.75_chr1.bed`:

```
wc -l Mus_musculus.GRCm38.75_chr1.bed
```

```
81226 Mus_musculus.GRCm38.75_chr1.bed
```

You can also check multiple files at once:

```
wc -l Mus_musculus.GRCm38.75_chr1.bed Mus_musculus.GRCm38.75_chr1.gtf
   81226 Mus_musculus.GRCm38.75_chr1.bed
   81231 Mus_musculus.GRCm38.75_chr1.gtf
  162457 total
```

Why are there five lines difference between the files? Use `head` to check:

```
head -n 6 Mus_musculus.GRCm38.75_chr1.gtf
```

```
#!genome-build GRCm38.p2
#!genome-version GRCm38
#!genome-date 2012-01
#!genome-build-accession NCBI:GCA_000001635.4
#!genebuild-last-updated 2013-09
1	pseudogene	gene	3054233	3054733	.	+	.	gene_id "ENSMUSG00000090025"; gene_name "Gm16088"; gene_source "havana"; gene_biotype "pseudogene";
```

Not a definitive answer, but `Mus_musculus.GRCm38.75_chr1.gtf` has a five line header.

### Column data

Coming soon...

And then: `grep`, `hexdump`, `sort`, `uniq`, `join`, `awk`, `sed`, regex...