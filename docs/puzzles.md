# Puzzles

!!! abstract  "Info"

    * Download data (and answers  😊)

    ```bash
    wget -c puzzles_da.tar.gz https://github.com/GenomicsAotearoa/shell-for-bioinformatics/releases/download/v2.0/puzzles_da.tar.gz -O - | tar -xz
    ```

    * Review the content of the **puzzles_da** directory
        - There are two directories, **data** and **answers**
        - Each filename has a unique ID which corresponds to the puzzle/question (Described below)

    * We recommend appending the solution to a file and compare the content of it with the corresponding answer in **answers** directory

 
    * How to compare your solution with the provided solution  (and here is another new command `cmp`)


    ```bash
    #!/bin/bash

    if cmp --silent -- "provided_answer.txt" "my_answer.txt"; then
        printf "\U1F60A SUCCESS \n"
    else 
        printf "\U1F97A Almost there...Try again please\n" 
    fi
    ```


??? question "Transcribing DNA into RNA (tdir)"

    An RNA string is a string formed from the alphabet containing 'A', 'C', 'G', and 'U'.

    Given a DNA string $t$ corresponding to a coding strand, its transcribed RNA string $u$ is formed by replacing all occurrences of 'T' in $t$ with 'U' in $u$

    **Given:** A DNA string $t$ having length at most 1000 nt.

    **Return:** The transcribed RNA string of $t$

    ??? success "Solution"

        === "bash"
            There are multiple ways to do this

            ```bash
            cat tdir_data.txt | tr T U
            ```
            ```bash
            awk '{gsub(/T/,"U");print}' tdir_data.txt
            ``` 
            ```bash
            sed 's/T/U/g' tdir_data.txt
            ```           

        === "R"
            ```r
            gsub("T","U",readLines("tdir_data.txt"))
            ```

        === "Python"
            ```py
            with open('tdir_data.txt', 'r') as f1:
                    dna = f1.read()
                    rna = dna.replace("T", "U")
                    print(rna)
            ```
        === "Julia"
            ```julia
            seq = open("tdir_data.txt") do file
                read(file, String)
            end

            seq = replace(seq, "T" => "U")
            println(seq)
            ```


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
            
        === "Julia"
        ```jl
        f = read(open("csod_data.txt"), String)

        dict = Dict("A"=>"T", "C"=>"G", "T"=>"A", "G"=>"C")

        for i in reverse(f[1:end-1])
            print(dict[string(i)])
        end

        println()
        ```

- - - 

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
- - - 

??? question "Maximum Matchings and RNA Secondary Structures (mmrs)"

    !!! Figure inline end

        ![image](./images/matching_%26_rna.png)

    The graph theoretical analogue of the quandary stated in the introduction above is that if we have an RNA string $s$ that does not have the same number of occurrences of 'C' as 'G' and the same number of occurrences of 'A' as 'U', then the bonding graph of $s$ cannot possibly possess a perfect matching among its basepair edges. For example, see ***Figure 1***; in fact, most bonding graphs will not contain a perfect matching.

    

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
        === "Python"
            ```py
            from Bio import SeqIO
            from scipy.special import perm

            with open('mmrs_data.txt', encoding='utf-8') as handle:
                rna = SeqIO.read(handle, 'fasta').seq

            au, cg = (rna.count('A'), rna.count('U')), (rna.count('C'), rna.count('G'))
            print(perm(max(au), min(au), exact=True) * perm(max(cg), min(cg), exact=True))
            ```
          
- - - 

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
            END {print max_id"\n"max_gc;}' cgcc_data.txt
            ```
        === "Perl"
            ```perl
            perl -ne 'BEGIN{$/="\n>";$gc=-1}chomp;s/^>//;($id,$seq)=(split(/\n/,$_,2));$seq=~s/\n//g;$cgc=100*(($seq=~y/GC//)/length($seq));if($cgc>$gc){$gc=$cgc;$gcid=$id};END{print "$gcid\n$gc\n"}' data/cgcc_data.txt
            ```
        === "R (with seqinr)"
            ```r
            library(seqinr)
            fasta <- read.fasta("cgcc_data.txt")
            gc_content <- apply(matrix(names(fasta)), 1, function(x){GC(fasta[[x]])})
            most_gc <- which(gc_content==max(gc_content))

            #Result
            rbind(names(fasta)[most_gc], paste(signif(gc_content[most_gc], 4) * 100, "%", sep=""))
            ```
        === "Python"
            ```py
            from Bio import SeqIO
            from Bio.Seq import Seq
            from Bio.Alphabet import IUPAC
            from Bio.SeqUtils import GC
            max_seq = Seq('tata', IUPAC.unambiguous_dna)
            max_gc = ()
            for seq_record in SeqIO.parse('cgcc_data.txt','fasta'):
                if GC(seq_record.seq) > GC(max_seq):
                    max_id = seq_record.id
                    max_seq = seq_record.seq
                    max_gc = GC(seq_record.seq)
            print(max_id)
            print (max_gc)
            ```

        === "Python (🤓 version)"
            ```py
            from Bio import SeqIO
            from Bio.SeqUtils import GC

            input_file = 'cgcc_data.txt'
            seq_dict = {
                seq_record.id: GC(seq_record.seq)
                for seq_record in SeqIO.parse(input_file, "fasta")
            }

            max_gc = max(seq_dict, key=seq_dict.get) # find key of the dicionary's max value
            print(max_gc)
            print(seq_dict[max_gc])
            ```


- - - 

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
