<center>
# **Intermediate-Advanced Shell for Bioinformatics**
</center>

<br>
<p align="center"><img src="images/dna_tux.png" alt="drawing" width="300"/></p> 
<br>

<!--- check -->

| **Lesson**                                         | **Overview** | 
|:---------------------------------------------------|:-------------|
|1. [UNIX,Linux & UNIX Shell](./0_introduction.md)|Introduction to UNIX operating system, Linux and UNIX Shell|
|2. [Download and verify data](./1_download_data.md)| Downloading data with `wget`/`curl` and check the transferred data’s integrity with check‐sums|
|3. [UNIX Shell Basics & Recap](./2_unixshellbasics.md)|Navigating Files & Directories and a review of  commands used in routine tasks|
|4. [Inspecting and Manipulating Text Data with UNIX Tools - Part 1](./4_inspectmanipluate.md)| Inspect file/s with utilities such as `head`,`less`. Extracting and formatting tabular data. Magical `grep`. Substitute matching patterns with `sed`|
|5. [Inspecting and Manipulating Text Data with UNIX Tools - Part 2](./5_inspectmanipulate2.md)| Text processing with `awk` and `bioawk`|
|6. [Automating File-Processing with find and xargs](./6_automate_fileprocessing_find_xargs.md)
|7. [Puzzles](./puzzles.md) 🧩                                      | 

- - - 


# Attribution Notice

* This workshop material is heavily inspired by : 
    1. Buffalo, V (2015). ***Bioinformatics Data Skills***.O'Reilly Media, Inc
    2. The Carpentries. ***The Unix Shell*** . https://swcarpentry.github.io/shell-novice/
    3. The Carpentries. ***Introduction to Command Line for Genomics***. https://datacarpentry.org/shell-genomics/
    4. Rosalind Project. https://rosalind.info/about/

- - - 

# License 



Genomics Aotearoa / New Zealand eScience Infrastructure "Intermediate-Advanced Shell for Bioinformatics" is licensed under the **GNU General Public License v3.0, 29 June 2007** . ([Follow this link for more information](https://github.com/GenomicsAotearoa/shell-for-bioinformatics/blob/main/LICENSE))

- - - 

# Setup

If possible, we do recommend using the **Remote** option over **Local**  ( Especially for *Windows* hosts). This will eliminate  the need to install any additional applications

-  **Remote** option will require an exiting NeSI Account

### Remote

??? info "Log into NeSI Mahuika Jupyter Service"

    1. Follow [https://jupyter.nesi.org.nz/hub/login](https://jupyter.nesi.org.nz/hub/login)
    2. <p>Enter NeSI username, HPC password and 6 digit second factor token<br>![image](./images/jupyter_login_labels_updated.png)</p>
    3. <p>Choose server options as below
    <br>>>make sure to choose the correct project code `nesi02659`, number of CPUs `CPUs=4`, memory `8 GB` prior to pressing ![images](./images/start_button.png){width="40"}  button.

    <br>![images](./images/jupyter_server2022.png)

### Local  :warning:



??? info "Local host setup - Windows, MacOS & Linux"

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


