# Download Data

!!! abstract "Lesson Objectives"

    - To be able to download data with command line utilities
    - Inspect data (checksum verification)

!!! info "Download with `wget` from a terminal client"

    ```bash
    $ wget -c https://github.com/GenomicsAotearoa/shell-for-bioinformatics/releases/download/v1.0/data_shell4b.tar.gz
    ```
    OR

    ```bash
    $ wget -c https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.zip
    ```

- - - 

!!! info "Download via Web Browser"

    * Data can be downloaded directly from this [link](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.tar.gz) which will download **bashfbio_v1.0_data.tar.gz** to *Downloads* directory
    * If the above link fails, try [this alternative .zip](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.zip)


## Data Integrity

Data we download  is the starting point of all future analyses and conclusions. So it’s important to explicitly check the transferred data’s integrity with check‐sums. Checksums are very compressed summaries of data, computed in a way that even if just one bit of the data is changed, the checksum will be different. As such data integrity checks are also helpful in keeping track of data versions. Checksums facilitate reproducibility, as we can link a particular analysis and set of results to an exact version of data summarized by the data’s checksum value.

!!! book-lock "SHA and MD5 Checksums"

Two most common checksum algorithms are MD5 and SHA ( There are we will be referring to **SHA256**)

* **MD5** : cryptographic hash function algorithm that takes the message as input of any length and changes it into a fixed-length message of 16 bytes. MD5 algorithm stands for the message-digest algorithm. 

* **SHA**  : SHA-256 is a more secure and newer cryptographic hash function that was launched in 2000 as a new version of SHA functions and was adopted as *Federal Information Processing Standards* (FIPS) in 2002. It is allowed to use a hash generator tool to produce a SHA256 hash for any string or input value. Also, it generates 256 hash values, and the internal state size is 256 bit and the original message size is up to 264-1 bits.

To create checksums, we can pass arbitrary strings to the program `md5sum` (or `sha256sum`) through standard in 

```bash

echo "shell for Bioinformatics" | md5sum
echo "shell for BioInformatics" | md5sum
```
```bash
198638c380be53bf3f6ff70d5626ae44  -
afa4dbcc56b540e24558085fdc10342f  -
```