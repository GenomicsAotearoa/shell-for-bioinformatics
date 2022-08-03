# Puzzles



### How to compare your solution with the provided solution


```bash
#!/bin/bash

if cmp --silent -- "provided_answer.txt" "my_answer.txt"; then
echo "contents are identical"
else 
echo "files differ"
fi
```


??? question "Transcribing DNA into RNA (tdir)"

    An RNA string is a string formed from the alphabet containing 'A', 'C', 'G', and 'U'.

    Given a DNA string $t$ corresponding to a coding strand, its transcribed RNA string u is formed by replacing all occurrences of 'T' in $t$ with 'U' in $u$

    **Given:** A DNA string $t$ having length at most 1000 nt.

    **Return:** The transcribed RNA string of $t$

    ??? success "Solution"

        === "bash"

            ```bash
            cat tdir_data.txt | tr T U
            ```
            ```bash
            tr T U < tdir_data.txt
            ```

        === "R"

- - - 

??? question "Complementing a Strand of DNA (csod)"

    In DNA strings, symbols 'A' and 'T' are complements of each other, as are 'C' and 'G'.

    The reverse complement of a DNA string $s$ is the string $s^{c}$ formed by reversing the symbols of $s$ then taking the complement of each symbol (e.g., the reverse complement of "GTCA" is "TGAC").

    **Given:** A DNA string s of length at most 1000 bp.

    **Return:** The reverse complement $s^{c}$ of $s$.

    ??? success "Solution"

        === "bash"
            
            There are multiple ways to do this 
            ```bash
            rev csod_data.txt | tr ATCG TAGC
            ```
            ```bash
            rev csod_data.txt | sed 'y/ATCG/TAGC/'
            ```
            ```bash
            cat csod_data.txt | tr 'ACGT' 'TGCA' | rev
            ``` 

            * For multi-line sequences
            ```bash
            tr -d "\n" < data.txt| rev  | tr ATCG TAGC
            ```

            * For FASTA files
            ```bash
            grep -v "^>" r.fasta | tr -d "\n"  | rev  | tr ATCG TAGC
            ```
    

    


        

        === "R"


??? question "Counting DNA Nucleotides (dnct)"

    A string is simply an ordered collection of symbols selected from some alphabet and formed into a word; the length of a string is the number of symbols that it contains.

    An example of a length 21 DNA string (whose alphabet contains the symbols 'A', 'C', 'G', and 'T') is "ATGCTTCAGAAAGGTCTTACG."
 
    **Given:** A DNA string $s$ of length at most 1000 nt.

    **Return:** Four integers (separated by spaces) counting the respective number of times that the symbols 'A', 'C', 'G', and 'T' occur in s

    ??? success "Solution"

        === "bash"
            ```bash
            awk '{a=gsub("A","");c=gsub("C","");g=gsub("G","");t=gsub("T","")} END {print a,c,g,t}' /path/to/file x=$(cat dnct_data.txt); for i in A C G T; do y=${x//[^$i]}; echo -n "${#y} "; done
            ```



??? question "Maximum Matchings and RNA Secondary Structures (mmrs)"

    !!! Figure inline end

        ![image](./images/matching_%26_rna.png)

    The graph theoretical analogue of the quandary stated in the introduction above is that if we have an RNA string s that does not have the same number of occurrences of 'C' as 'G' and the same number of occurrences of 'A' as 'U', then the bonding graph of s cannot possibly possess a perfect matching among its basepair edges. For example, see ***Figure 1***; in fact, most bonding graphs will not contain a perfect matching.

    

    In light of this fact, we define a maximum matching in a graph as a matching containing as many edges as possible. See ***Figure 2*** for three maximum matchings in graphs.

    A maximum matching of basepair edges will correspond to a way of forming as many base pairs as possible in an RNA string, as shown in ***Figure 3***.

    **Given:** An RNA string $s$ of length at most 100.

    **Return:** The total possible number of maximum matchings of basepair edges in the bonding graph of $s$

    ??? success "Solution"

        === "bash"
            ```bash
            #!/bin/bash

            dna="$(cat mmrs_data.txt | tail -n +2 | tr -d '\n')"

            count () {
                echo -n "${dna//[^$1]/}" | wc -c
            }

            matches () {
            local n1=$(count $1)
            local n2=$(count $2)
            if test $n2 -gt $n1; then
                local tmp=$n1
                n1=$n2
                n2=$tmp
            fi
            seq -s \* $((n1 - n2 + 1)) $n1
            }

            echo "$(matches A U) * $(matches C G)" | bc 
            ```
        === "R"
          

??? question "Computing GC Content (cgcc)"

    The GC-content of a DNA string is given by the percentage of symbols in the string that are 'C' or 'G'. For example, the GC-content of "AGCTATAG" is 37.5%. Note that the reverse complement of any DNA string has the same GC-content.

    DNA strings must be labeled when they are consolidated into a database. A commonly used method of string labeling is called FASTA format. In this format, the string is introduced by a line that begins with '>', followed by some labeling information. Subsequent lines contain the string itself; the first line to begin with '>' indicates the label of the next string.

    In our implementation, a string in FASTA format will be labeled by the ID "GAotearoa_xxxx", where "xxxx" denotes a four-digit code between 0000 and 9999.

    **Given:** At most 10 DNA strings in FASTA format (of length at most 1 kbp each).

    **Return:** The ID of the string having the highest GC-content, followed by the GC-content of that string. Rosalind allows for a default error of 0.001 in all decimal answers unless otherwise stated; please see the note on absolute error below.

    ??? success "Solution"

        === "bash"
            ```bash
            #/bin/bash

            awk -v RS=">" -v FS="\n" 'BEGIN {max_id=""; max_gc="";} \
            $0 !="" { \
            gc=0; \
            l=0; \
            for(i=2;i<=NF;i++) { \
                gc += gsub(/[GC]/, ".", $i); \
                l += length($i);} \
                if(max_gc < gc/l*100) {max_id=$1; max_gc=gc/l*100} \
            } \
            END {print max_id"\n"max_gc;}' cgcc.txt
            ```

??? question "Counting Disease Carriers (cdcr)"

    To model the Hardy-Weinberg principle, assume that we have a population of $N$ diploid individuals. If an allele is in genetic equilibrium, then because mating is random, we may view the 2$N$ chromosomes as receiving their alleles uniformly. In other words, if there are m dominant alleles, then the probability of a selected chromosome exhibiting the dominant allele is simply $p=m/2N$

    Because the first assumption of genetic equilibrium states that the population is so large as to be ignored, we will assume that $N$ is infinite, so that we only need to concern ourselves with the value of $p$

    **Given:** An array $A$ for which $A[k]$ represents the proportion of homozygous recessive individuals for the $k$-th Mendelian factor in a diploid population. Assume that the population is in genetic equilibrium for all factors.

    **Return:** An array $B$ having the same length as $A$ in which $B[k]$ represents the probability that a randomly selected individual carries at least one copy of the recessive allele for the $k$-th factor.

    ??? success "Solution"

        === "bash"
            ```bash
            cat cdcr_data.txt | tr \  '\n' | sed 's/.*/2 * sqrt(&) - &/' | bc -l
            ```
