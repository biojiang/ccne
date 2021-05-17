# ccne: Carbapenemase-encoding gene Copy Number Estimator
# Introduction
Carbapenemase-encoding gene Copy Number Estimator (ccne) is a tool to estimate the copy number of carbapenemase-encoding gene. It uses housekeeping gene as the reference and compares the count of reads that mapped to carbapenemase-encoding genes with the count of reads that mapped to the reference gene. 
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
  --dbdir [X]        CCNE database root folders (default '/data/project/MultiKPCCNV/ccne/data')
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
# Licence
* GPL V3

# Author
* Jiang Jianping
* Institute of Antibiotics, Huashan hospital, Fudan University
* jiangjianping@fudan.edu.cn
