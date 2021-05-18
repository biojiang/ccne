# ccne: Carbapenemase-encoding gene Copy Number Estimator
# Introduction
Carbapenemase-encoding gene Copy Number Estimator (ccne) is a tool to estimate the copy number of carbapenemase-encoding gene. It uses housekeeping gene as the reference and compares the count of reads that mapped to carbapenemase-encoding genes with the count of reads that mapped to the reference gene. 
# Quick start
```
$ ccne --carb KPC-2 --sp Kp --in File.list --out result.txt
All finished! Enjoy!

$ ls
File.list result.txt SRR14561347_1.fastq.gz SRR14561347_2.fastq.gz

$ head File.list
SRR14561347	./SRR14561347_1.fastq.gz	./SRR14561347_2.fastq.gz

$ head result.txt
ID	rpoB reads coverage	KPC-2 reads coverage	ratio
SRR14561347	767.177023498695	2398.69106881406	3.12664612643751
```
# Installation
## Source
Install the latest version direct from Github. 
```
$ cd $HOME

$ git clone https://github.com/biojiang/ccne.git

$ $HOME/ccne/ccne --help
```
## Check installation
Check the ccne version:
```
$ ccne --version
```
Check dependencies:<br/>
The ccne will check the dependencies automatically each time before running.

# Usage
```
Name:
  Ccne 1.0.0 by Jianping Jiang <jiangjianping@fudan.edu.cn>
Synopsis:
  Carbapenemase-encoding gene copy number estimator
Usage:
  ccne --carb KPC-2 --sp Kp --in File.list --out result.txt
General:
  --help             This help
  --version          Print version and exit
  --quiet            No screen output (default OFF)
Setup:
  --dbdir [X]        CCNE database root folders (default '/ccne/data')
  --listdb           List all configured databases
  --listsp           List all configured species and housekeeping genes
Input:
  --carb [X]         Carbapenemase-encoding gene name, such as KPC-2, NDM-1, etc. Please refer to --listdb (required)
  --sp [X]           Species name[Kp|Ec|Ab|Pa](required)
  --ref [X]          Reference gene defalut(Kp:rpoB Ab:rpoB Ec:polB Pa:ppsA), please refer to --listsp
  --in [X]           Input file name (required)
Outputs:
  --out [X]          Output file name (required)
Computation:
  --flank [N]        The flanking length of sequence to be excluded (default '100')
  --cpus [N]         Number of CPUs to use (default '1')

```
# Running
## Input Requirements
* Sequence read file(s) in FASTQ format (can be .gz compressed) format
* Carbapenemase-encoding gene name
* The reference single copy housekeeping gene name
## Output File
The ccne will output the result to the file with the name the user provided.
## Columns in the output file
Name|Description
---|:--
ID|The sample ID user provided in the input file
rpoB reads coverage|The estimated reads coverage of the input reference housekeeping gene
KPC-2 reads coverage|The estimated reads coverage of the input carbapenemase-encoding gene
ratio|Divide the reads coverage of carbapenemase-encoding gene into that of housekeeping gene
## Tutorial
1. Fetch the reads files (SRR14561347) in fastq format from NCBI SRA database. (SRR14561347 generated from a *Klebsiella pneumoinae* clinical isolate with triple KPC-2 encoding genes on the plasmid)
```
$ fasterq-dump --split-3 SRR14561347
```
2. Copy input file templete from templete folder.
```
$ cp ./templete/templete.list File.list
```
3. Modify the input file.
```
$ head File.list
SRR14561347	./SRR14561347_1.fastq.gz	./SRR14561347_2.fastq.gz
```
4. Run ccne.
```
$ ccne --carb KPC-2 --sp Kp --in File.list --out result.txt
```
5. Check the result.
```
$ head result.txt
ID	rpoB reads coverage	KPC-2 reads coverage	ratio
SRR14561347	767.177023498695	2398.69106881406	3.12664612643751
```
# Dependencies
* **bwa**</br>
Used for reads mapping</br>
*Li H. and Durbin R. (2009) Fast and accurate short read alignment with Burrows-Wheeler transform. Bioinformatics, 25:1754-1760.* [PMID: [19451168](http://www.ncbi.nlm.nih.gov/pubmed/19451168)]
* **samtools**</br>
Used for fetching mapped reads and sorting them by locus</br>
*Li H., Handsaker B. et al, 2009, The Sequence alignment/map (SAM) format and SAMtools, Bioinformatics, 25(16):2078-9.* [PMID:[19505943](http://www.ncbi.nlm.nih.gov/pubmed/19505943)]
* **bedtools**</br>
Used for getting bed files</br>
*Quinlan R A. and Hall M I., 2010, BEDTools: a flexible suite of utilities for comparing genomic features, Bioinformatics, 26(6):841-2.* [PMID:[20110278](https://pubmed.ncbi.nlm.nih.gov/20110278)]
# Test environment
Ubuntu 5.4.0 with perl v5.26.2 (Theoretically compatible with other generic Linux version but not tested)
# Licence
* [GPL V3](https://github.com/biojiang/ccne/blob/main/LICENSE)

# Author
* Jiang Jianping
* Institute of Antibiotics, Huashan hospital, Fudan University
* jiangjianping@fudan.edu.cn
