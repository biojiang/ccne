# ccne: Carbapenemase-encoding gene Copy Number Estimator
# Introduction
**C**arbapenemase-encoding gene **C**opy **N**umber **E**stimator (ccne) is a tool to estimate the copy number of AMR genes. It uses housekeeping gene as the reference and compares the count of reads that mapped to AMR genes with the count of reads that mapped to the reference gene. 
# Quick start
```
$ ccne --amr KPC-2 --sp Kpn --in File.list --out result.txt
All finished! Enjoy!

$ ls
File.list result.txt SRR14561347_1.fastq.gz SRR14561347_2.fastq.gz

$ head File.list
SRR14561347	./SRR14561347_1.fastq.gz	./SRR14561347_2.fastq.gz

$ head result.txt
ID	rpoB reads depth  SD of rpoB reads depth  KPC-2 reads depth SD of KPC-2 reads depth Ratio Estimated KPC-2 copy number
SRR14561347 766.040208488459  64.5952660470369  2418.11451247166  112.252829680359  3.15664176067605  3
```
# Installation
## Source
Install the latest version direct from Github. 
```
$ cd $HOME

$ git clone https://github.com/biojiang/ccne.git

$ $HOME/ccne/bin/ccne --help
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
  ccne --amr KPC-2 --sp Kpn --in File.list --out result.txt
General:
  --help             This help
  --version          Print version and exit
  --quiet            No screen output (default OFF)
Setup:
  --dbdir [X]        CCNE database root folders (default '$CCNE_bin/db')
  --listdb           List all configured AMRs
  --listsp           List all configured species and housekeeping genes
  --fmtdb            Format all the bwa index
Input:
  --amr [X]          AMR gene name, such as KPC-2, NDM-1, etc or AMR ID. Please refer to --listdb (required)
  --sp [X]           Species name[Kpn|Eco|Aba|Pae|Pls|...]. Please refer to --listsp. (required)
  --ref [X]          Reference gene defalut(such as Kpn:rpoB Aba:rpoB Eco:polB Pae:pps), please refer to --listsp. Note: When --sp is set to Pls, this parameter should be set to a replicon type.
  --in [X]           Input file name (required)
Outputs:
  --out [X]          Output file name (required)
Computation:
  --flank [N]        The flanking length of sequence to be excluded (default '0')
  --cpus [N]         Number of CPUs to use (default '1')
  --multiref         Use the reads depth of all the available sequences (default OFF)

```
# Running
## Input Requirements
* Sequence read file(s) in FASTQ format (can be .gz compressed) format
* AMR gene name (refer to --listdb)
* The species code ( refer to --listsp or species code table)
## Output File
The ccne will output the result to the file with the name the user provided.
## Columns in the output file
Name|Description
|:---|:--
|ID|The sample ID user provided in the input file
|rpoB reads coverage|The estimated reads coverage of the input reference housekeeping gene
|SD of rpoB reads coverage|The standard deviation of rpoB reads coverage
|KPC-2 reads coverage|The estimated reads coverage of the input carbapenemase-encoding gene
|SD of KPC-2 reads coverage|The standard deviation of KPC-2 reads coverage
|Ratio|Divide the reads coverage of AMR gene into that of housekeeping gene
|Estimated KPC-2 copy number|Rounding of ratio
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
$ ccne --amr KPC-2 --sp Kpn --in File.list --out result.txt
```
5. Check the result.
```
$ head result.txt
ID	rpoB reads depth  SD of rpoB reads depth  KPC-2 reads depth SD of KPC-2 reads depth Ratio Estimated KPC-2 copy number
SRR14561347 766.040208488459  64.5952660470369  2418.11451247166  112.252829680359  3.15664176067605  3
```
## AMR genes (2412 genes) in ccne
|Antimicrobial agents|Gene name
|:---|:--
|Aminoglycosides|AAC(2'), AAC(3), AAC(6'), ANT(3''), APH(2''), ANT(2''), ANT(4'), ANT(6), ANT(9), APH(3''), APH(3'), APH(4), APH(6), APH(7''), APH(9), ApmA, ArmA, KamB, NpmA, RmtA, RmtB, RmtC, RmtD, RmtE, RmtF, RmtG, RmtH, SAT, Sgm, Sta, StrA, StrB
|β-lactams|ACC, ACI, ACT, ADC, AER, AIM, AmpC1, AQU, ARL, AST-1, BAT-1, BcI, BcII, BCL-1, BEL, BES, BIC, BIL, BJP, BKC, Bla1, Bla2, blaA, blaB21, blaF, BPU-1, BRO, BUT-1, CAM-1, CARB, CAU, CblA, CBP, CepA, CepS, CfiA, CfxA, CGA, CGB, CIA, CKO, CME, CMH, CMY, CphA, CPS, CTX-M, DES, DHA, DIM, EBR, ERP, ESP, FAR, FEZ, FIM, FONA, FOX, FPH, FRI, FTU, GES, GIM, GOB, HERA, HMB, IMI, IMP, IND, JOHN-1, KHM, KPC, L1, LAP, LCR, LHK, LRA, MIR, MOX, MSI, MUS, NDM, NPS, OCH, OXA, OXY, PC1, PDC, PEDO, PER, PNGM, R39, RCP, RHO, RM3, ROB, RSA, SCO, SDA-A, SED, SFB, SFH, SHV-OKP-LEN, SIM, SLB, SMB, SME, SPG, SPM, SRT, TEM, THIN-B, TLA, TMB, TRU, TUS, VCC, VEB, VIM, SFO-1
|Colistin|ICR-Mo, Mcr1, Mcr2, Mcr3, Mcr4, Mcr5, Mcr6, Mcr7, Mcr8, Mcr9
|Fosfomycin|Fom, FosA, FosB, FosC, FosD, FosK, FosX
|Fluoroquinolone|CrpP, NorA, QepA, Qnr, QnrA, QnrB, QnrC, QnrD, QnrE, QnrS, QnrVC
|Glycopeptide|VanA, VanB, VanC, VanD, VanE, VanF, VanG, VanHA, VanHB, VanHD, VanHE, VanHM, VanHO, VanI, VanJ, VanL, VanM, VanN, VanO, VanRA, VanRB, VanRC, VanRD, VanRE, VanRF, VanRG, VanRI, VanRL, VanRM, VanRN, VanRO, VanSA, VanSB, VanSC, VanSD, VanSE, VanSF, VanSG, VanSL, VanSM, VanSN, VanSO, VanTC, VanTE, VanTG, VanTmL, VanTN, VanTrL, VanUG, VanVB, VanWB, VanWG, VanWI, VanXA, VanXB, VanXD, VanXE, VanXI, VanXM, VanXO, VanXYC, VanXYE, VanXYG, VanXYL, VanXYN, VanYA, VanYB, VanYD, VanYF, VanYG1, VanYM, VanZA, VanZF
|Macrolides|MefB, Mel, CarA, CfrA, CfrB, CfrC, clbB, clbC, clcD, Emt, EreA, EreB, EreD, Erm, gimA, LinG, LnuA, LnuB, LnuC, LnuD, LnuF, LnuG, LnuP, LsaA, LsaB, LsaC, LsaE, MgtA, MphA, MphB, MphC, MphE, MphF, MphG, MphH, MphI, MphJ, MphK, MphL, MphM, MphN, MphO, MsrA, MsrC, MsrE, MyrA, OleB, OleC, OleD, OleI, OptrA, PoxtA, SrmB, TirC, TvaA, VatA, VatB, VatC, VatD, VatE, VatF, VatH, VatI, VgaA, VgaB, VgaC, VgaD, VgaE, VgbA, VgbB, VgbC, VmlR
|Phenicols|Cat, CmlA, CmlB, CmlR, CmlV, Cmr, CmrA, FloR, PexA, CatBx
|Rifampin|Arr, IRI, RphB
|Sulfonamide|Sul
|Tetracycline|OtrA, OtrB, OtrC, Tcr3, Tet-30, Tet-31, Tet-32, Tet-33, Tet-35, Tet-36, Tet-37, Tet-38, Tet-39, Tet-40, Tet-41, Tet-42, Tet-43, Tet-44, Tet-45, Tet-48, Tet-49, Tet-50, Tet-51, Tet-52, Tet-53, Tet-54, Tet-55, Tet-56, Tet-59, TetA, TetB, TetC, TetD, TetE, TetG, TetH, TetJ, TetK, TetL, TetM, TetO, TetQ, TetS, TetT, TetU, TetV, TetW, TetX, TetY, TetZ
|Tigecycline|TetX, tmexC, tmexD, toprJ
|Trimethoprim|Dfr

Details refer to [CARD AMR genes](https://github.com/katholt/Kleborate/blob/master/kleborate/data/CARD_AMR_clustered.csv) in Kleborate

## Supported species
|Class|Group/Family|Genus|Species
|:---|:---|:---|:---
|Aerobic gram-positive cocci|||
||Staphlococcaceae|*Staphylococcus*|*Staphylococcus aureus, Staphylococcus epidermidis, Staphylococcus haemolyticus, Stapylococcus hominis, Staphylococcus lugdunensis, Staphylococcus pseudintermedius*
||Streptococcaceae|*Streptococcus*|*Streptococcus agalactiae, Streptococcus canis, Streptococcus dysgalactiae, Streptococcus gallolyticus, Streptococcus oralis, Streptococcus pneumoniae, Streptococcus pyogenes, Streptococcus suis, Streptococcus thermophilus, Streptococcus uberis, Streptococcus equi*
||Enterococcaceae|*Enterococcus*|*Enterococcus faecalis, Enterococcus faecium*
|Aerobic gram-positive bacillus|||
||Listeriaceae|*Listeria*|*Listeria monocytogenes*
||Lactobacillaceae|*Lactobacillus*|*Lactobacillus salivarius*
||Bacillaceae|*Bacillus*|*Bacillus cereus, Bacillus licheniformis, Bacillus subtilis*
||Corynebacteriaceae|*Corynebacterium*|*Corynebacterium diphtheriae*
||Streptomycetaceae|*Streptomyces*|*Streptomyces spp.*
||Mycobacteriaceae|*Mycobacterium*|*Mycobacterium abscessus, Mycobacterium massiliense*
|Aerobic gram-negative cocci|||
||Neisseriaceae|*Neisseria*|*Neisseria spp.*
||Moraxellaceae|*Moraxella*|*Moraxella catarrhalis*
|Aerobic gram-negative bacillus|||
||Enterobacteriaceae|*Escherichia*|*Escherichia spp.*
|||*Shigella*|*Shigella spp.*
|||*Salmonella*|*Salmonella enterica*
|||*Yersinia*|*Yersinia spp., Yersinia pseudotuberculosis, Yersinia ruckeri*
|||*Klebsiella*|*Klebsiella oxytoca, Klebsiella pneumoniae, Klebsiella aerogenes*
|||*Enterobacter*|*Enterobacter cloacae*
|||*Citrobacter*|*Citrobacter freundii*
|||*Serratia*|*Serratia marcescens*
|||*Proteus*|*Proteus mirabilis*
|||*Edwardsiella*|*Edwardsiella tarda*
|Aerobic gram-negative bacillus (Non-Enterobacteriaceae)|||
||Aeromonadaceae|*Aeromonas*|*Aeromonas spp.*
||Vibrionaceae|*Vibrio*|*Vibrio cholerae, Vibrio spp., Vibrio parahaemolyticus, Vibrio tapetis, Vibrio vulnificus*
|Aerobic gram-negative bacillus (Sugar unfermented)|||
||Pseudomonadaceae|*Pseudomonae*|*Pseudomonas aeruginosa, Pseudomonas fluorescens*
||Moraxellaceae|*Acinetobacter*|*Acinetobacter baumannii*
||Flavobacteriaceae|*Flavobacterium*|*Flavobacterium psychrophilum*
||Burkholderiaceae|*Burkholderia*|*Burkholderia cepacia, Burkholderia pseudomallei*
||Xanthomonadaceae|*Stenotrophomonas*|*Stenotrophomonas maltophilia*
||Alcaligenaceae|*Achromobacter*|*Achromobacter spp.*
|Other aerobic gram-negative bacteria|||
||Pasteurellaceae|*Hemophilus*|*Haemophilus influenzae, Haemophilus parasuis, Haematopinus suis*
||Neisseriaceae|*Kingella*|*Kingella kingae*
||Alcaligenaceae|*Bordetella*|*Bordetella pertussis*
||Pasteurellaceae|*Pasteurella*|*Pasteurella multocida*
||Bartonellaceae|*Bartonella*|*Bartonella henselae*
|Anaerobic gram-positive bacillus (Non-Spore-Forming)|||
||Propionibacteriaceae|*Propionibacterium*|*Propionibacterium acnes* 
|Anaerobic gram-positive bacillus (Spore-Forming)|||
||Clostridiaceae|*Clostridium*|*Clostridium botulinum, Clostridium difficile, Clostridium septicum*
|Anaerobic gram-negativ bacillus|||
||Porphyromonasaceae|*Porphyromonas*|*Porphyromonas gingivalis*
|Gram-negative Campylobacter and Helicobacter|||
||Campylobacteraceae|*Campylobacter*|*Campylobacter coli, Campylobacter jejuni, Campylobacter concisus, Campylobacter fetus, Campylobacter helveticus, Campylobacter hyointestinalis, Campylobacter insulaenigrae, Campylobacter lanienae, Campylobacter lari, Campylobacter sputorum, Campylobacter upsaliensis*
|||*Arcobacter*|*Arcobacter spp.*
||Helicobacteraceae|*Helicobacte*|*Helicobacter cinaedi, Helicobacter pylori*
|Spirochetes|||
||Leptospiraceae|*Leptospira*|*Leptospira*
|||*Borrelia*|*Borrelia spp.*
|||*Brachyspira*|*Brachyspira hampsonii, Brachyspira hyodysenteriae, Brachyspira intermedia, Brachyspira pilosicoli, Brachyspira*
|Mycoplasma, Chlamydia etc.|||
||Mycoplasmataceae|*Mycoplasma*|*Mycoplasma agalactiae, Mycoplasma bovis, Mycoplasma hyorhinis*
||Chlamydiaceae|*Chlamydia*|*Chlamydia spp.*
||Rickettsiaceae|*Orientia*|*Orientia tsutsugamushi*


## Species codes and default housekeeping genes
Codes in ccne|Species|Default housekeeping genes
|:---|:---|:---
**Aba**|*Acinetobacter baumannii*|*rpoB*
Ach|*Achromobacter spp.*|*rpoB*
Aer|*Aeromonas spp.*|*gltA*
Arc|*Arcobacter spp.*|*gltA*
Aap|*Anaplasma aphagocytophilum*|*atpA*
**Bcc**|*Burkholderia cepacia*|*atpD*
Bce|*Bacillus cereus*|*glp*
Bha|*Brachyspira hampsonii*|*adh*
Bhe|*Bartonella henselae*|*rpoB*
Bhy|*Brachyspira hyodysenteriae*|*adh*
Bin|*Brachyspira intermedia*|*adh*
Bli|*Bacillus licheniformis*|*rpoB*
Bpe|*Bordetella pertussis*|*adk*
Bor|*Borrelia spp.*|*clpA*
Bpi|*Brachyspira pilosicoli*|*adh*
Bps|*Burkholderia pseudomallei*|*ace*
Bra|*Brachyspira spp.*|*adh*
Bsu|*Bacillus subtilis*|*glpF*
Cco|*Campylobacter coli*|*aspA*
Cje|*Campylobacter jejuni*|*aspA*
Cbo|*Clostridium botulinum*|*rpoB*
Ccn|*Campylobacter concisus*|*aspA*
Cdi|*Clostridium difficile*|*adk*
Pdi|*Peptoclostridium difficile*|*adk*
Cdp|*Corynebacterium diphtheriae*|*rpoB*
Cfe|*Campylobacter fetus*|*aspA*
Cfr|*Citrobacter freundii*|*mdh*
Che|*Campylobacter helveticus*|*aspA*
Chl|*Chlamydia spp.*|*enoA*
Chy|*Campylobacter hyointestinalis*|*aspA*
Cin|*Campylobacter insulaenigrae*|*aspA*
Cla|*Campylobacter lanienae*|*aspA*
Clr|*Campylobacter lari*|*adk*
Cma|*Carnobacterium maltaromaticum*|*dapE*
Cro|*Cronobacter spp.*|*atpD*
Cse|*Clostridium septicum*|*ddl*
Csp|*Campylobacter sputorum*|*aspA*
Cup|*Campylobacter upsaliensis*|*adk*
**Ecl**|*Enterobacter cloacae*|*rpoB*
**Eco**|*Escherichia spp.*|*rpoB*
Shi|*Shigella spp.*|*rpoB*
Eta|*Edwardsiella tarda*|*adk*
**Efa**|*Enterococcus faecalis*|*aroE*
**Efm**|*Enterococcus faecium*|*adk*
Fps|*Flavobacterium psychrophilum*|*atpA*
Hci|*Helicobacter cinaedi*|*aroE*
**Hin**|*Haemophilus influenzae*|*adk*
Hpa|*Haemophilus parasuis*|*rpoB*
Hpy|*Helicobacter pylori*|*atpA*
Hsu|*Haematopinus suis*|*atpA*
Kki|*Kingella kingae*|*abcZ*
**Kox**|*Klebsiella oxytoca*|*rpoB*
**Kpn**|*Klebsiella pneumoniae*|*rpoB*
**Kae**|*Klebsiella aerogenes*|*rpoB*
Lep|*Leptospira spp.*|*adk*
Lmo|*Listeria monocytogenes*|*abcZ*
Lsa|*Lactobacillus salivarius*|*nrdB*
Mab|*Mycobacterium abscessus*|*rpoB*
Mag|*Mycoplasma agalactiae*|*dnaA*
Mbo|*Mycoplasma bovis*|*adh1*
Mca|*Moraxella catarrhalis*|*abcZ*
Mha|*Mannheimia haemolytica*|*adk*
Mhy|*Mycoplasma hyorhinis*|*rpoB*
Mma|*Mycobacterium massiliense*|*rpoB*
Mpl|*Melissococcus plutonius*|*argE*
Nei|*Neisseria spp.*|*abcZ*
Orh|*Ornithobacterium rhinotracheale*|*mdh*
Ots|*Orientia tsutsugamushi*|*mdh*
Pac|*Propionibacterium acnes*|*aroE*
**Pae**|*Pseudomonas aeruginosa*|*ppsA*
Pfl|*Pseudomonas fluorescens*|*glnS*
Pgi|*Porphyromonas gingivalis*|*ftsQ*
Pla|*Paenibacillus larvae*|*rpoB*
Pmu|*Pasteurella multocida*|*adk*
Ppe|*Pediococcus pentosaceus*|*dalR*
Ran|*Riemerella anatipestifer*|*rpoB*
**Sag**|*Streptococcus agalactiae*|*adhP*
**Pau**|*Staphylococcus aureus*|*arcC*
Sca|*Streptococcus canis*|*gki*
Sdy|*Streptococcus dysgalactiae*|*atoB*
Sen|*Salmonella enterica*|*aroC*
**Sep**|*Staphylococcus epidermidis*|*arcC*
Sga|*Streptococcus gallolyticus*|*aroE*
Sha|*Staphylococcus haemolyticus*|*arcC*
**Sho**|*Stapylococcus hominis*|*arcC*
Sin|*Sinorhizobium spp.*|*asd*
Slu|*Staphylococcus lugdunensis*|*aroE*
**Sma**|*Stenotrophomonas maltophilia*|*atpD*
Sor|*Streptococcus oralis*|*aroE*
**Spn**|*Streptococcus pneumoniae*|*aroE*
Sps|*Staphylococcus pseudintermedius*|*ack*
**Spy**|*Streptococcus pyogenes*|*gki*
Ssu|*Streptococcus suis*|*aroA*
Sth|*Streptococcus thermophilus*|*rpoB*
Str|*Streptomyces spp.*|*atpD*
Sub|*Streptococcus uberis*|*arcC*
Seq|*Streptococcus equi*|*arcC*
Tay|*Taylorella spp.*|*adk*
Ten|*Tenacibaculum*|*atpA*
Vch|*Vibrio cholerae*|*adk*
Vib|*Vibrio spp.*|*atpA*
Vpa|*Vibrio parahaemolyticus*|*dnaE*
Vta|*Vibrio tapetis*|*atpA*
Vvu|*Vibrio vulnificus*|*dtdS*
Wol|*Wolbachia spp.*|*coxA*
Xfa|*Xylella fastidiosa*|*cysG*
Yer|*Yersinia spp.*|*aarF*
Yps|*Yersinia pseudotuberculosis*|*adk*
Yru|*Yersinia ruckeri*|*glnA*
**Smr**|*Serratia marcescens*|*rpoC*
**Pmi**|*Proteus mirabilis*|*rpoB*

**Species in blod are most commonly isolated species in clinical from [CHINET](http://www.chinets.com/Data/AntibioticDrugFast).**

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
Ubuntu 16.04 LTS with perl v5.26.2 (Theoretically compatible with other generic Linux version but not tested)
# Bundled binaries
For Linux (compiled on Ubuntu 16.04 LTS) some of the binaries are included.
# Licence
* ccne is free software, released under the [GPL V3](https://github.com/biojiang/ccne/blob/main/LICENSE)

# Author
* Jiang Jianping
* Institute of Antibiotics, Huashan hospital, Fudan University
* jiangjianping@fudan.edu.cn
