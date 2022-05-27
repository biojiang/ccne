# ccne: Carbapenemase-encoding gene Copy Number Estimator
# Introduction
**C**arbapenemase-encoding gene **C**opy **N**umber **E**stimator (ccne) is a tool to estimate the copy number of AMR genes. It uses housekeeping gene as the reference and compares the count of reads that mapped to AMR genes with the count of reads that mapped to the reference gene. 
# Quick start
## ccne-fast
```
$ ccne-fast --amr KPC-2 --sp Kpn --in File.list --out result.txt --cpus 4
All finished! Enjoy!

$ ls
File.list result.txt SRR14561347_1.fastq.gz SRR14561347_2.fastq.gz

$ head File.list
SRR14561347	./SRR14561347_1.fastq.gz	./SRR14561347_2.fastq.gz

$ head result.txt
ID      rpoB reads depth        SD of rpoB reads depth  KPC-2 reads depth       SD of KPC-2 reads depth Estimated KPC-2 copy number
SRR14561347     653.944899478779        53.7865295303472        2006.6179138322 96.5807513871426        3.06848163420428
```
## ccne-acc
```
$ ccne-acc --amr KPC-2 --in File.list --out result.txt --cpus 4
All finished! Enjoy!

$ ls
File.list result.txt SRR14561347_1.fastq.gz SRR14561347_2.fastq.gz  SRR14561347.fasta

$ head File.list
SRR14561347	./SRR14561347_1.fastq.gz	./SRR14561347_2.fastq.gz  SRR14561347.fasta

$ head result.txt
ID      Average reference reads depth   KPC-2 reads depth       Estimated KPC-2 copy number
SRR14561347     570     2127    3.73157894736842
```
# Installation
## Bioconda [![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat)](http://bioconda.github.io/recipes/ccne/README.html)
If you use Conda you can use the Bioconda channel:
```
conda install -c conda-forge -c bioconda -c defaults ccne
```
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
## ccne-fast
```
Name:
  ccne-fast 1.1.0 by Jianping Jiang <jiangjianping@fudan.edu.cn>
Synopsis:
  Carbapenemase-encoding gene copy number estimator
Usage:
  ccne-fast --amr KPC-2 --sp Kpn --in File.list --out result.txt
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
## ccne-acc
```
Name:
  ccne-acc 1.1.0 by Jianping Jiang <jiangjianping@fudan.edu.cn>
Synopsis:
  Carbapenemase-encoding gene copy number estimator
Usage:
  ccne-acc --amr KPC-2 --in File.list --out result.txt
General:
  --help             This help
  --version          Print version and exit
  --quiet            No screen output (default OFF)
Setup:
  --dbdir [X]        CCNE database root folders (default '$CCNE_bin/db')
  --listdb           List all configured AMRs
  --fmtdb            Format all the bwa index
Input:
  --amr [X]          AMR gene name, such as KPC-2, NDM-1, etc or AMR ID. Please refer to --listdb (required)
  --in [X]           Input file name (required)
Outputs:
  --out [X]          Output file name (required)
Computation:
  --cpus [N]         Number of CPUs to use (default '1')

```
# Running
## Input Requirements
### ccne-fast
* Sequence read file(s) in FASTQ format (can be .gz compressed) format
* AMR gene name (refer to --listdb)
* The species code ( refer to --listsp or species code table)
### ccne-acc
* Sequence read file(s) in FASTQ format (can be .gz compressed) format
* AMR gene name (refer to --listdb)
* The genome assembly
## Output File
The ccne will output the result to the file with the name the user provided.
## Columns in the output file
### ccne-fast
Name|Description
|:---|:--
|ID|The sample ID user provided in the input file
|rpoB reads coverage|The estimated reads coverage of the input reference housekeeping gene
|SD of rpoB reads coverage|The standard deviation of rpoB reads coverage
|KPC-2 reads coverage|The estimated reads coverage of the input carbapenemase-encoding gene
|SD of KPC-2 reads coverage|The standard deviation of KPC-2 reads coverage
|Estimated KPC-2 copy number|Divide the reads coverage of AMR gene into that of housekeeping gene
### ccne-acc
Name|Description
|:---|:--
|ID|The sample ID user provided in the input file
|Average reads coverage|The estimated reads coverage of the genome
|KPC-2 reads coverage|The estimated reads coverage of the input carbapenemase-encoding gene
|Estimated KPC-2 copy number|Divide the reads coverage of AMR gene into that of housekeeping gene
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
ID      rpoB reads depth        SD of rpoB reads depth  KPC-2 reads depth       SD of KPC-2 reads depth Estimated KPC-2 copy number
SRR14561347     653.944899478779        53.7865295303472        2006.6179138322 96.5807513871426        3.06848163420428
```
## AMR genes (2412 genes) in ccne
|Antimicrobial agents|Subtypes|Gene or protein names1
|:---|:--|:---
|Aminoglycosides||AAC(2')(7), AAC(3)(20), AAC(6')(65), ANT(2'')(1), ANT(3'')(35), ANT(4')(4), ANT(6)(5), ANT(9)(2), APH(2'')(7), APH(3'')(3), APH(3')(18), APH(4)(2), APH(6)(7), APH(7'')(1), APH(9)(3), ApmA(1), ArmA(1), KamB(1), NpmA(1), RmtA(1), RmtB(1), RmtC(1), RmtD(2), RmtE(2), RmtF(1), RmtG(1), RmtH(1), SAT(3), Sgm(1), Sta(1), StrA(1), StrB(1)
|β-lactams|β-lactamase|ACC(4), ACT(35), ADC(14), AER(1), AIM(1), AQU(3), ARL(6), AST-1(1), AmpC1(1), BAT-1(1), BCL-1(1), BIL(1), BJP(1), BPU-1(1), BRO(3), BUT-1(1), BcI(1), BcII(1), Bla1(1), Bla2(1), CAM-1(1), CARB(20), CAU(1), CBP(1), CKO(1), CMH(1), CMY(114), CPS(1), CblA(1), CepS(1), CfiA(1), CfxA(4), CphA(10), DHA(19), EBR(1), ESP(1), FEZ(1), FONA(6), FOX(9), FTU(1), GES(4), GIM(2), GOB(13), HERA(7), HMB(1), IND(16), JOHN-1(1), KHM(1), L1(1), LAP(2), LCR(1), LHK(1), LRA(13), MIR(16), MOX(9), MSI(2), MUS(2), NPS(1), OXA(114), PC1(1), PDC(10), PEDO(3), PNGM(1), R39(1), RCP(1), RHO(1), RM3(1), ROB(2), RSA(2), SCO(1), SDA-A(1), SED(1), SFB(1), SIM(1), SLB(1), SMB(1), SPG(1), SRT(1), TEM(51), THIN-B(1), TMB(1), TRU(1), TUS(1), blaA(1), blaB21(1), blaF(1)
|β-lactams|ESBL|ACI(1), ADC(32), BEL(3), BES(1), CARB(1), CGA(1), CIA(4), CME(1), CMY(5), CTX-M(141), CepA(1), CfxA(3), DES(1), ERP(1), FAR(1), GES(9), OCH(8), OXA(25), OXY(24), PDC(3), PER(7), SFO-1(1), SHV-OKP-LEN(42), SRT(1), TEM(84), TLA(4), VEB(10)
|β-lactams|Carbapenem|BIC(1), BKC(1), CGB(1), CTX-M(1), DIM(1), EBR(1), FIM(1), FPH(1), FRI(3), GES(13), IMI(7), IMP(48), KPC(20), NDM(27), OXA(300), SFH(1), SME(5), SPM(1), TMB(1), VCC(1), VIM(40)
|β-lactams|Chromosomal β-lactamase|SHV-OKP-LEN(163)
|β-lactams|Inhibitor-resistant β-lactamase|SHV-OKP-LEN(6), TEM(23)
|β-lactams|Inhibitor-resistant ESBL|TEM(9)
|Colistin||ICR-Mo(1), Mcr1(14), Mcr2(2), Mcr3(12), Mcr4(5), Mcr5(2), Mcr6(1), Mcr7(1), Mcr8(1), Mcr9(1)
|Fosfomycin||Fom(2), FosA(4), FosB(7), FosC(2), FosD(1), FosK(1), FosX(2)
|Fluoroquinolone||CrpP(1), NorA(1), QepA(1), Qnr(1), QnrA(7), QnrB(71), QnrC(1), QnrD(2), QnrE(2), QnrS(12), QnrVC(6)
|Glycopeptide||VanA(1), VanB(1), VanC(1), VanD(1), VanE(1), VanF(1), VanG(1), VanHA(1), VanHB(1), VanHD(1), VanHE(1), VanHM(1), VanHO(1), VanI(1), VanJ(1), VanL(1), VanM(1), VanN(1), VanO(1), VanRA(1), VanRB(1), VanRC(1), VanRD(1), VanRE(1), VanRF(1), VanRG(1), VanRI(1), VanRL(1), VanRM(1), VanRN(1), VanRO(1), VanSA(1), VanSB(1), VanSC(1), VanSD(1), VanSE(1), VanSF(1), VanSG(1), VanSL(1), VanSM(1), VanSN(1), VanSO(1), VanTC(1), VanTE(1), VanTG(1), VanTN(1), VanTmL(1), VanTrL(1), VanUG(1), VanVB(1), VanWB(1), VanWG(1), VanWI(1), VanXA(1), VanXB(1), VanXD(1), VanXE(1), VanXI(1), VanXM(1), VanXO(1), VanXYC(1), VanXYE(1), VanXYG(1), VanXYL(1), VanXYN(1), VanYA(1), VanYB(1), VanYD(1), VanYF(1), VanYG1(1), VanYM(1), VanZA(1), VanZF(1)
|Macrolides||CarA(1), CfrA(1), CfrB(1), CfrC(1), Emt(1), EreA(2), EreB(1), EreD(1), Erm(47), LinG(1), LnuA(1), LnuB(1), LnuC(1), LnuD(1), LnuF(2), LnuG(1), LnuP(1), LsaA(1), LsaB(1), LsaC(1), LsaE(1), MefB(1), Mel(1), MgtA(1), MphA(1), MphB(2), MphC(2), MphE(2), MphF(1), MphG(1), MphH(1), MphI(1), MphJ(1), MphK(1), MphL(1), MphM(1), MphN(1), MphO(1), MsrA(2), MsrC(2), MsrE(1), MyrA(1), OleB(1), OleC(1), OleD(1), OleI(1), OptrA(1), PoxtA(1), SrmB(1), TirC(1), TvaA(1), VatA(1), VatB(1), VatC(1), VatD(2), VatE(1), VatF(1), VatH(1), VatI(1), VgaA(2), VgaB(1), VgaC(1), VgaD(1), VgaE(2), VgbA(1), VgbB(1), VgbC(1), VmlR(1), clbB(1), clbC(1), clcD(1), gimA(1)
|Phenicols||Cat(33), CatBx(1), CmlA(5), CmlB(2), CmlR(1), CmlV(1), Cmr(1), CmrA(2), FloR(2), PexA(1)
|Rifampin||Arr(7), IRI(1), RphB(1)
|Sulfonamide||Sul(4)
|Tetracycline||OtrA(1), OtrB(1), OtrC(1), Tcr3(1), Tet-30(1), Tet-31(1), Tet-32(2), Tet-33(1), Tet-35(1), Tet-36(1), Tet-37(1), Tet-38(1), Tet-39(1), Tet-40(1), Tet-41(1), Tet-42(1), Tet-43(1), Tet-44(1), Tet-45(1), Tet-48(1), Tet-49(1), Tet-50(1), Tet-51(1), Tet-52(1), Tet-53(1), Tet-54(1), Tet-55(1), Tet-56(1), Tet-59(1), TetA(7), TetB(6), TetC(1), TetD(1), TetE(2), TetG(2), TetH(2), TetJ(1), TetK(1), TetL(2), TetM(2), TetO(1), TetQ(1), TetS(1), TetT(1), TetU(1), TetV(1), TetW(2), TetX(1), TetY(1), TetZ(1)
|Tigecycline||TetX(2), tmexC(1), tmexD(1), toprJ(1)
|Trimethoprim||Dfr(52)

1Numbers in the last brackets are the number of alleles.
Details refer to [CARD AMR genes](https://github.com/katholt/Kleborate/blob/master/kleborate/data/CARD_AMR_clustered.csv) in Kleborate.

## Supported species in ccne-fast
|Class|Group/Family|Genus|Species
|:---|:---|:---|:---
|Aerobic Gram-positive cocci|||
||Staphlococcaceae|*Staphylococcus*|*Staphylococcus aureus, Staphylococcus epidermidis, Staphylococcus haemolyticus, Stapylococcus hominis, Staphylococcus lugdunensis, Staphylococcus pseudintermedius*
||Streptococcaceae|*Streptococcus*|*Streptococcus agalactiae, Streptococcus canis, Streptococcus dysgalactiae, Streptococcus gallolyticus, Streptococcus oralis, Streptococcus pneumoniae, Streptococcus pyogenes, Streptococcus suis, Streptococcus thermophilus, Streptococcus uberis, Streptococcus equi*
||Enterococcaceae|*Enterococcus*|*Enterococcus faecalis, Enterococcus faecium*
|Aerobic Gram-positive bacillus|||
||Listeriaceae|*Listeria*|*Listeria monocytogenes*
||Lactobacillaceae|*Lactobacillus*|*Lactobacillus salivarius*
||Bacillaceae|*Bacillus*|*Bacillus cereus, Bacillus licheniformis, Bacillus subtilis*
||Corynebacteriaceae|*Corynebacterium*|*Corynebacterium diphtheriae*
||Streptomycetaceae|*Streptomyces*|*Streptomyces spp.*
||Mycobacteriaceae|*Mycobacterium*|*Mycobacterium abscessus, Mycobacterium massiliense*
|Aerobic Gram-negative cocci|||
||Neisseriaceae|*Neisseria*|*Neisseria spp.*
||Moraxellaceae|*Moraxella*|*Moraxella catarrhalis*
|Aerobic Gram-negative bacillus|||
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
|Aerobic Gram-negative bacillus (Non-Enterobacteriaceae)|||
||Aeromonadaceae|*Aeromonas*|*Aeromonas spp.*
||Vibrionaceae|*Vibrio*|*Vibrio cholerae, Vibrio spp., Vibrio parahaemolyticus, Vibrio tapetis, Vibrio vulnificus*
|Aerobic Gram-negative bacillus (Sugar unfermented)|||
||Pseudomonadaceae|*Pseudomonae*|*Pseudomonas aeruginosa, Pseudomonas fluorescens*
||Moraxellaceae|*Acinetobacter*|*Acinetobacter baumannii*
||Flavobacteriaceae|*Flavobacterium*|*Flavobacterium psychrophilum*
||Burkholderiaceae|*Burkholderia*|*Burkholderia cepacia, Burkholderia pseudomallei*
||Xanthomonadaceae|*Stenotrophomonas*|*Stenotrophomonas maltophilia*
||Alcaligenaceae|*Achromobacter*|*Achromobacter spp.*
|Other aerobic Gram-negative bacteria|||
||Pasteurellaceae|*Hemophilus*|*Haemophilus influenzae, Haemophilus parasuis, Haematopinus suis*
||Neisseriaceae|*Kingella*|*Kingella kingae*
||Alcaligenaceae|*Bordetella*|*Bordetella pertussis*
||Pasteurellaceae|*Pasteurella*|*Pasteurella multocida*
||Bartonellaceae|*Bartonella*|*Bartonella henselae*
|Anaerobic Gram-positive bacillus (Non-Spore-Forming)|||
||Propionibacteriaceae|*Propionibacterium*|*Propionibacterium acnes* 
|Anaerobic Gram-positive bacillus (Spore-Forming)|||
||Clostridiaceae|*Clostridium*|*Clostridium botulinum, Clostridium difficile, Clostridium septicum*
|Anaerobic Gram-negativ bacillus|||
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
|Other|||
||||*Anaplasma aphagocytophilum*
||||*Peptoclostridium difficile*
||||*Carnobacterium maltaromaticum*
||||*Cronobacter spp.*
||||*Mannheimia haemolytica*
||||*Melissococcus plutonius*
||||*Ornithobacterium rhinotracheale*
||||*Paenibacillus larvae*
||||*Pediococcus pentosaceus*
||||*Riemerella anatipestifer*
||||*Sinorhizobium spp.*
||||*Taylorella spp.*
||||*Tenacibaculum spp.*
||||*Wolbachia spp.*
||||*Xylella fastidiosa*

## Species codes and default housekeeping genes
Codes in ccne|Species|Default housekeeping genes
|:---|:---|:---
Aba|***Acinetobacter baumannii***|*rpoB*
Ach|*Achromobacter spp.*|*rpoB*
Aer|*Aeromonas spp.*|*gltA*
Arc|*Arcobacter spp.*|*gltA*
Aap|*Anaplasma aphagocytophilum*|*atpA*
Bcc|***Burkholderia cepacia***|*atpD*
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
Ecl|***Enterobacter cloacae***|*rpoB*
Eco|***Escherichia spp.***|*rpoB*
Shi|*Shigella spp.*|*rpoB*
Eta|*Edwardsiella tarda*|*adk*
Efa|***Enterococcus faecalis***|*aroE*
Efm|***Enterococcus faecium***|*adk*
Fps|*Flavobacterium psychrophilum*|*atpA*
Hci|*Helicobacter cinaedi*|*aroE*
Hin|***Haemophilus influenzae***|*adk*
Hpa|*Haemophilus parasuis*|*rpoB*
Hpy|*Helicobacter pylori*|*atpA*
Hsu|*Haematopinus suis*|*atpA*
Kki|*Kingella kingae*|*abcZ*
Kox|***Klebsiella oxytoca***|*rpoB*
Kpn|***Klebsiella pneumoniae***|*rpoB*
Kae|***Klebsiella aerogenes***|*rpoB*
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
Pae|***Pseudomonas aeruginosa***|*ppsA*
Pfl|*Pseudomonas fluorescens*|*glnS*
Pgi|*Porphyromonas gingivalis*|*ftsQ*
Pla|*Paenibacillus larvae*|*rpoB*
Pmu|*Pasteurella multocida*|*adk*
Ppe|*Pediococcus pentosaceus*|*dalR*
Ran|*Riemerella anatipestifer*|*rpoB*
Sag|***Streptococcus agalactiae***|*adhP*
Pau|***Staphylococcus aureus***|*arcC*
Sca|*Streptococcus canis*|*gki*
Sdy|*Streptococcus dysgalactiae*|*atoB*
Sen|*Salmonella enterica*|*aroC*
Sep|***Staphylococcus epidermidis***|*arcC*
Sga|*Streptococcus gallolyticus*|*aroE*
Sha|*Staphylococcus haemolyticus*|*arcC*
Sho|***Stapylococcus hominis***|*arcC*
Sin|*Sinorhizobium spp.*|*asd*
Slu|*Staphylococcus lugdunensis*|*aroE*
Sma|***Stenotrophomonas maltophilia***|*atpD*
Sor|*Streptococcus oralis*|*aroE*
Spn|***Streptococcus pneumoniae***|*aroE*
Sps|*Staphylococcus pseudintermedius*|*ack*
Spy|***Streptococcus pyogenes***|*gki*
Ssu|*Streptococcus suis*|*aroA*
Sth|*Streptococcus thermophilus*|*rpoB*
Str|*Streptomyces spp.*|*atpD*
Sub|*Streptococcus uberis*|*arcC*
Seq|*Streptococcus equi*|*arcC*
Tay|*Taylorella spp.*|*adk*
Ten|*Tenacibaculum spp.*|*atpA*
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
Smr|***Serratia marcescens***|*rpoC*
Pmi|***Proteus mirabilis***|*rpoB*

**Species in blod are most commonly isolated species in clinical. (Data come from the [CHINET](http://www.chinets.com/Data/AntibioticDrugFast))**

# Q&A
1. How the reference genes in ccne were determined?

   The reference genes in ccne are single copy housekeeping genes. If the species has been curated in [pubMLST](https://pubmlst.org/), then the hosuekeeping genes in pubMLST will be used. Usually, the fisrt allele will be used as the reference in ccne. Otherwise, the reference genome and gene model of the species will be download from NCBI GenBank. The [busco](https://busco.ezlab.org/) is used to determine the single copy genes. By reads simulation and mapping, the top 10 single copy genes with the lowest RMSE (the comparison between base pair wise read depths and simulated read depth) are selected as the reference genes.
   
2. How to interpret the SD of reads depth?

   The SD of reads depth is the standard deviation of reads depth of a gene. If this value is large, then the reads depth will be unreliable. In our practical analysis, the SD of reads depth is usually less than 15% of the mean.
   

# Dependencies
## ccne-fast & ccne-acc
* **HTStream**</br>
Used for raw reads QC</br>
*Petersen, Kristen R., David A. Streett, et al., 2015, Super deduper, fast PCR duplicate detection in fastq files. In Proceedings of the 6th ACM Conference on Bioinformatics, Computational Biology and Health Informatics, pp. 491-492.* 
* **bwa**</br>
Used for reads mapping</br>
*Li H. and Durbin R., 2009, Fast and accurate short read alignment with Burrows-Wheeler transform. Bioinformatics, 25:1754-1760.* [PMID: [19451168](http://www.ncbi.nlm.nih.gov/pubmed/19451168)]
* **samtools**</br>
Used for fetching mapped reads and sorting them by locus</br>
*Li H., Handsaker B. et al., 2009, The Sequence alignment/map (SAM) format and SAMtools, Bioinformatics, 25(16):2078-9.* [PMID:[19505943](http://www.ncbi.nlm.nih.gov/pubmed/19505943)]
* **bedtools**</br>
Used for getting bed files</br>
*Quinlan R A. and Hall M I., 2010, BEDTools: a flexible suite of utilities for comparing genomic features, Bioinformatics, 26(6):841-2.* [PMID:[20110278](https://pubmed.ncbi.nlm.nih.gov/20110278)]
* **deepTools**&</br>
Used for getting bed files</br>
[Installation](https://deeptools.readthedocs.io/en/develop/content/installation.html)</br>
*Ramírez, Fidel, Devon P. Ryan, et al., 2016, deepTools2: A next Generation Web Server for Deep-Sequencing Data Analysis, Nucleic Acids Research.* [PMID:[27079975](https://pubmed.ncbi.nlm.nih.gov/27079975)]
* **blast**&</br>
Used for finding AMR gene on the genome</br>
*Camacho C., Coulouris G., et al., 2008, BLAST+: architecture and applications, BMC Bioinformatics, 0:42.* [PMID:[20003500](https://pubmed.ncbi.nlm.nih.gov/20003500)]
* **perl Math::CDF**&</br>
Used for estimating AMR CN</br>
[Math::CDF](https://metacpan.org/pod/Math::CDF)
<p>& Only required for ccne-acc, the others are required for both ccne-fast and ccne-acc.</p>

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
