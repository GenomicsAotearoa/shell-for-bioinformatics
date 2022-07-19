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

