
??? question "The Second Nucleic Acid"

    An RNA string is a string formed from the alphabet containing 'A', 'C', 'G', and 'U'.

    Given a DNA string *t* corresponding to a coding strand, its transcribed RNA string u is formed by replacing all occurrences of 'T' in *t* with 'U' in u

    **Given:** A DNA string *t* having length at most 1000 nt.

    **Return:** The transcribed RNA string of *t*


??? question "The secondary and tertiary structures of DNA"

    In DNA strings, symbols 'A' and 'T' are complements of each other, as are 'C' and 'G'.

    The reverse complement of a DNA string ***s*** is the string ***s^c^*** formed by reversing the symbols of *s* then taking the complement of each symbol (e.g., the reverse complement of "GTCA" is "TGAC").

    **Given:** A DNA string s of length at most 1000 bp.

    **Return:** The reverse complement ***s^c^*** of ***s***.


??? question "Counting DNA Nucleotides"

    A string is simply an ordered collection of symbols selected from some alphabet and formed into a word; the length of a string is the number of symbols that it contains.

    An example of a length 21 DNA string (whose alphabet contains the symbols 'A', 'C', 'G', and 'T') is "ATGCTTCAGAAAGGTCTTACG."
 
    **Given:** A DNA string ***s*** of length at most 1000 nt.

    **Return:** Four integers (separated by spaces) counting the respective number of times that the symbols 'A', 'C', 'G', and 'T' occur in s


??? question "Maximum Matchings and RNA Secondary Structures"

    !!! Figure inline end

        ![image](./images/matching_%26_rna.png)

    The graph theoretical analogue of the quandary stated in the introduction above is that if we have an RNA string s that does not have the same number of occurrences of 'C' as 'G' and the same number of occurrences of 'A' as 'U', then the bonding graph of s cannot possibly possess a perfect matching among its basepair edges. For example, see ***Figure 1***; in fact, most bonding graphs will not contain a perfect matching.

    

    In light of this fact, we define a maximum matching in a graph as a matching containing as many edges as possible. See ***Figure 2*** for three maximum matchings in graphs.

    A maximum matching of basepair edges will correspond to a way of forming as many base pairs as possible in an RNA string, as shown in ***Figure 3***.

    **Given:** An RNA string ***s*** of length at most 100.

    **Return:** The total possible number of maximum matchings of basepair edges in the bonding graph of ***s***
