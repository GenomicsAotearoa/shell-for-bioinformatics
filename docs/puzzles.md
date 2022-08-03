# Puzzles


??? question "Transcribing DNA into RNA (tdir)"

    An RNA string is a string formed from the alphabet containing 'A', 'C', 'G', and 'U'.

    Given a DNA string $t$ corresponding to a coding strand, its transcribed RNA string u is formed by replacing all occurrences of 'T' in $t$ with 'U' in $u$

    **Given:** A DNA string $t$ having length at most 1000 nt.

    **Return:** The transcribed RNA string of $t$


??? question "Complementing a Strand of DNA (csod)"

    In DNA strings, symbols 'A' and 'T' are complements of each other, as are 'C' and 'G'.

    The reverse complement of a DNA string $s$ is the string $s^{c}$ formed by reversing the symbols of $s$ then taking the complement of each symbol (e.g., the reverse complement of "GTCA" is "TGAC").

    **Given:** A DNA string s of length at most 1000 bp.

    **Return:** The reverse complement $s^{c}$ of $s$.


??? question "Counting DNA Nucleotides (dnct"

    A string is simply an ordered collection of symbols selected from some alphabet and formed into a word; the length of a string is the number of symbols that it contains.

    An example of a length 21 DNA string (whose alphabet contains the symbols 'A', 'C', 'G', and 'T') is "ATGCTTCAGAAAGGTCTTACG."
 
    **Given:** A DNA string $s$ of length at most 1000 nt.

    **Return:** Four integers (separated by spaces) counting the respective number of times that the symbols 'A', 'C', 'G', and 'T' occur in s


??? question "Maximum Matchings and RNA Secondary Structures"

    !!! Figure inline end

        ![image](./images/matching_%26_rna.png)

    The graph theoretical analogue of the quandary stated in the introduction above is that if we have an RNA string s that does not have the same number of occurrences of 'C' as 'G' and the same number of occurrences of 'A' as 'U', then the bonding graph of s cannot possibly possess a perfect matching among its basepair edges. For example, see ***Figure 1***; in fact, most bonding graphs will not contain a perfect matching.

    

    In light of this fact, we define a maximum matching in a graph as a matching containing as many edges as possible. See ***Figure 2*** for three maximum matchings in graphs.

    A maximum matching of basepair edges will correspond to a way of forming as many base pairs as possible in an RNA string, as shown in ***Figure 3***.

    **Given:** An RNA string $s$ of length at most 100.

    **Return:** The total possible number of maximum matchings of basepair edges in the bonding graph of $s$

??? question "Computing GC Content"

    The GC-content of a DNA string is given by the percentage of symbols in the string that are 'C' or 'G'. For example, the GC-content of "AGCTATAG" is 37.5%. Note that the reverse complement of any DNA string has the same GC-content.

    DNA strings must be labeled when they are consolidated into a database. A commonly used method of string labeling is called FASTA format. In this format, the string is introduced by a line that begins with '>', followed by some labeling information. Subsequent lines contain the string itself; the first line to begin with '>' indicates the label of the next string.

    In our implementation, a string in FASTA format will be labeled by the ID "GAotearoa_xxxx", where "xxxx" denotes a four-digit code between 0000 and 9999.

    **Given:** At most 10 DNA strings in FASTA format (of length at most 1 kbp each).

    **Return:** The ID of the string having the highest GC-content, followed by the GC-content of that string. Rosalind allows for a default error of 0.001 in all decimal answers unless otherwise stated; please see the note on absolute error below.


??? question "Counting Disease Carriers"

    To model the Hardy-Weinberg principle, assume that we have a population of $N$ diploid individuals. If an allele is in genetic equilibrium, then because mating is random, we may view the 2$N$ chromosomes as receiving their alleles uniformly. In other words, if there are m dominant alleles, then the probability of a selected chromosome exhibiting the dominant allele is simply $p=m/2N$

    Because the first assumption of genetic equilibrium states that the population is so large as to be ignored, we will assume that $N$ is infinite, so that we only need to concern ourselves with the value of $p$

    **Given:** An array $A$ for which $A[k]$ represents the proportion of homozygous recessive individuals for the $k$-th Mendelian factor in a diploid population. Assume that the population is in genetic equilibrium for all factors.

    **Return:** An array $B$ having the same length as $A$ in which $B[k]$ represents the probability that a randomly selected individual carries at least one copy of the recessive allele for the $k$-th factor.