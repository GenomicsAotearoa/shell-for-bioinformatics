# Setup & Download Data

<center>
## Setup
</center>

### Remote

### Local



???+ info "Local host setup - Windows, MacOS & Linux"

    === "Windows Hosts"

        * Install either 
          - Git for Windows from [https://git-scm.com/download/win](https://git-scm.com/download/win) **OR**
          - MobaXterm Home (*Portable* or *Installer* edition) from [https://mobaxterm.mobatek.net/download-home-edition.html](https://mobaxterm.mobatek.net/download-home-edition.html)
              * Portable edition does not require administrative privileges 

    === "MacOS"

          * Native terminal client is sufficient.
          * It might not comes with `wget` download data via command line (can be installed with `$ brew install wget`)
          * However, it is not required as we provide a direct link to download data in .zip format 

    === "Linux"

          * Native terminal client is sufficient.

    !!! warning "`bioawk` install on all hosts"

        One of the tools used in this workshop is `bioawk` which is not native to any terminal clients. Installing it on MacOS and Linux can be done with `$ brew install bioawk` & `$ sudo apt install bioawk`, respectively.Windows hosts might have to do it via `conda` according to [these instructions](https://anaconda.org/bioconda/bioawk). However, this will require a prior install of **Anaconda** Or **Miniconda** 

<center>
## Download data
</center>

!!! info "Download via Web Browser"

    * Data can be downloaded directly from this [link](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.tar.gz) which will download **bashfbio_v1.0_data.tar.gz** to *Downloads* directory
    * If the above link fails, try [this alternative .zip](https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.zip)

!!! info "Download with `wget` from a terminal client"

    ```bash
    $ wget -c https://github.com/GenomicsAotearoa/shell-for-bioinformatics/releases/download/v1.0/data_shell4b.tar.gz
    ```
    OR

    ```bash
    $ wget -c https://github.com/GenomicsAotearoa/bash-for-bioinformatics/releases/download/v1.0rc1/data_shell4b.zip
    ```

- - - 
<p style="text-align:left;">
  <b><a class="btn" href="https://genomicsaotearoa.github.io/bash-for-bioinformatics/" style="background: var(--bs-green);font-weight:bold">&laquo; Home </a></b> 
  <span style="float:right;">
    <b><a class="btn" href="https://genomicsaotearoa.github.io/bash-for-bioinformatics/workshop_material/1_introduction.html" style="background: var(--bs-green);font-weight:bold">1 - Unix Shell and Bash &raquo;</a></b>
  </span>
</p>
