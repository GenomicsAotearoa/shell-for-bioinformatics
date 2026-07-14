
 <p align="center"><img src="./images/ga_reannz.png" alt="drawing" width="300"/></p> 

# <p align="center"><img src="./images/logo-2023-sept.png" alt="drawing" width="370"/></p> 


<!--- check -->

| **Lesson**                                         | **Overview** | 
|:---------------------------------------------------|:-------------|
|[Download and verify data](./2_download_data.md)|Downloading data with `wget`/`curl` and check the transferred data’s integrity with check‐sums|
|[Streams, Redirection and Pipe](./3_streams_red_pipe.md)|Combining pipes and redirection, Using "Exit" statuses|
|[Inspecting and Manipulating Text Data with UNIX Tools - Part 1](./4_inspectmanipulate.md)| Inspect file/s with utilities such as `head`,`less`. Extracting and formatting tabular data. Magical `grep`. |
|[Inspecting and Manipulating Text Data with UNIX Tools - Part 2](./5_inspectmanipulate2.md)|Substitute matching patterns with `sed`.|
|[Inspecting and Manipulating Text Data with UNIX Tools - Part 3](.//6_inspectmanipulate3.md)| Text processing with `awk` and `bioawk`|
|[Puzzles](./puzzles.md) 🧩 | Can you use shell scripts to solve these "real" life challenges in molecular biology ?|
|[Supplementary - 1](./supplementary/supplementary_1.md) | Recap - Unix , Linux and Unix shell|
|[Supplementary - 2](./supplementary/supplementary_2.md) | Recap - Shell basics and commands  |
|[Supplementary - 3](./supplementary/supplementary_3.md)| Escaping, Special Characters|
|[Supplementary - 4](./supplementary/supplementary_4.md)| Automating File-Processing with find and xargs. Search files by pattern with `find` and use `xargs` to execute a command for those objects matching the pattern|


- - - 


!!! copyright "Attribution Notice"

    * This workshop material is heavily inspired by : 
        1. Buffalo, V (2015). ***Bioinformatics Data Skills***.O'Reilly Media, Inc
        2. The Carpentries. ***The Unix Shell*** . https://swcarpentry.github.io/shell-novice/
        3. The Carpentries. ***Introduction to Command Line for Genomics***. https://datacarpentry.org/shell-genomics/
        4. Rosalind Project. https://rosalind.info/about/
    
- - - 

!!! key "License" 

    Genomics Aotearoa / The Research Education Advanced Network New Zealand (REANNZ)  "Intermediate Shell for Bioinformatics" is licensed under the **GNU General Public License v3.0, 29 June 2007** . ([Follow this link for more information](https://github.com/GenomicsAotearoa/shell-for-bioinformatics/blob/main/LICENSE))
    
- - - 

!!! screwdriver-wrench "Setup"

    - If possible, we do recommend using the **Remote** option over **Local**  ( Especially for *Windows* hosts). This will eliminate  the need to install any additional applications

    ### Remote
    
    ??? jupyter "Workshop will be running on REANNZ Training environment. Access details will be provided on the day of the workshop"

    
    ### Local  :warning:
    
    
    
    ??? circle-info "Local host setup - Windows, MacOS & Linux"
    
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
    
            One of the tools used in this workshop is `bioawk` which is not a native Linu/UNIX utility. Installing it on MacOS and Linux can be done with `$ brew install bioawk` & `$ sudo apt install bioawk`, respectively. Windows hosts might have to do it via `conda` according to [these instructions](https://anaconda.org/bioconda/bioawk). However, this will require a prior install of **Anaconda** Or **Miniconda** 
    
    
    