# Download & Verify Data

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

!!! info "Download via Web Browser (use locally)"

    * Data can be downloaded directly from this [link](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.tar.gz) which will download **bashfbio_v1.0_data.tar.gz** to *Downloads* directory
    * If the above link fails, try [this alternative .zip](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.zip)

## Decompress downloaded tar.gz OR .zip

A **TAR.GZ** file is a combination of two different packaging algorithms. The first is *tar*, short for tape archive. It’s an old utility, invented mainly for accurate data transfer to devices without their own file systems. A tar file (or tarball) contains files in a sequential format, along with metadata about the directory structure and other technical parameters.

It is useful to note that tar doesn’t compress the files in question, only packages them. Indeed, sometimes the resulting tarball can be of greater size due to padding. So that’s where **Gzip** comes in. **Gzip** (denoted by a .gz file extension) is a compressed file format used to archive data to take up smaller space. Gzip uses the same compression algorithm as the more commonly known zip but can only be used on a single file. In short, Gzip compresses all the individual files and tar packages them in a single archive.

!!! abstract "Decomressing the tar.gz file can be done  with the built-in `tar` utility" 

    ```bash
    tar -xvzf shell4b_data.tar.gz
    ```

    - `x`: Extract an archive.
    - `z`: Compress the archive with gzip.
    - `v`: Display progress in the terminal while creating the archive, also known as “verbose” mode. The `v`is always **optional** in these commands, but it’s helpful. 
    - `f`: Allows you to specify the filename of the archive.



ZIP files have capability to store multiple files using different compression techniques while at the same time supports storing a file without any compression. Each file is stored/compressed individually which helps to extract them, or add new ones, without applying compression or decompression to the entire archive.

Each file in a ZIP archive is represented as an individual entry where each entry consists of a Local File Header followed by the compressed file data.The Directory at the end of archive holds the references to all these file entries. ZIP file readers should avoid reading the local file headers and all sort of file listing should be read from the Directory. This Directory is the only source for valid file entries in the archive as files can be appended towards the end of the archive as well. That is why if a reader reads local headers of a ZIP archive from the beginning, it may read invalid (deleted) entries as well those are not part of the Directory being deleted from archive.

The order of the file entries in the central directory need not coincide with the order of file entries in the archive.

Decompressing .zip files can be done with `unzip` command

## Data Integrity

Data we download  is the starting point of all future analyses and conclusions. So it’s important to explicitly check the transferred data’s integrity with check‐sums. Checksums are very compressed summaries of data, computed in a way that even if just one bit of the data is changed, the checksum will be different. As such data integrity checks are also helpful in keeping track of data versions. Checksums facilitate reproducibility, as we can link a particular analysis and set of results to an exact version of data summarized by the data’s checksum value.

!!! book-lock "SHA and MD5 Checksums"

    Two most common checksum algorithms are MD5 and SHA ( There are we will be referring to **SHA256**)

    * **MD5** : cryptographic hash function algorithm that takes the message as input of any length and changes it into a fixed-length message of 16 bytes. MD5 algorithm stands for the message-digest algorithm. 

    * **SHA**  : SHA-256 is a more secure and newer cryptographic hash function that was launched in 2000 as a new version of SHA functions and was adopted as *Federal Information Processing Standards* (FIPS) in 2002. It is allowed to use a hash generator tool to produce a SHA256 hash for any string or input value. Also, it generates 256 hash values, and the internal state size is 256 bit and the original message size is up to 264-1 bits.

    To create checksums, we can pass arbitrary strings to the program `md5sum` (or `sha256sum`) through **standard** in 

    ```bash

    echo "shell for Bioinformatics" | md5sum
    echo "shell for BioInformatics" | md5sum
    ```
    ```bash
    198638c380be53bf3f6ff70d5626ae44  -
    afa4dbcc56b540e24558085fdc10342f  -
    ```
    Checksums are reported in hexadecimal format, where each digit can be one of 16 characters: digits 0 through 9, and the letters a, b, c, d, e, and f. The trailing dash indicates this is the MD5 checksum of input from **standard** in. Checksums with **file** input can be done with `md5usm filename` .i.e.

    ```bash
    md5sum tb1.fasta

    f44dca62012017196b545a2dd2d2906d  tb1.fasta
    ```
    Because it can get rather tedious to check each checksum individually, both `md5sum` and `sha256sum` has a convenient solution: Let's say that we want to verify checksum for all of the .fasta files in current directory. This can be done by creating and validating against a file containing the checksums of all the .fasta files. 

    * `fasta_checksums.md5` contains the checksums for three .fasta files which were verified prior to uploading to github repo. This was generated by using the command `md5sum *.fasta > fasta_checksums.md5`
    * Run `md5sum -c fasta_checksums.md5` and see whether all the .fasta files pass the verification
        - `-c` option represent **check** 