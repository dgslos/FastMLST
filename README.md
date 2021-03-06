![Conda](https://img.shields.io/conda/pn/bioconda/fastmlst)![CircleCI](https://img.shields.io/circleci/build/github/EnzoAndree/FastMLST/master)![GitHub](https://img.shields.io/github/license/EnzoAndree/FastMLST)[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat)](http://bioconda.github.io/recipes/fastmlst/README.html)[![Downloads](https://img.shields.io/conda/dn/bioconda/fastmlst.svg?style=flat)](http://bioconda.github.io/recipes/fastmlst/README.html)


# FastMLST
A multi-core Multilocus Sequence Typing tool coupled with allele concatenation.
# Introduction
FastMLST is a high speed standalone script wrote in Python3, which takes assemblies in FASTA format (gzipped is also allowed) and determines its ST according to MLST schemes defined in [PubMLST](https://doi.org/10.12688/wellcomeopenres.14826.1). The main advantage over other ST determination programs is that FastMLST allows the generation of a FASTA file containing the concatenated alleles for all analyzed genomes ready to be aligned and used in phylogenetic inference.

You can read a complete guide to MLST analysis in our [Wiki](https://github.com/EnzoAndree/FastMLST/wiki/The-definitive-guide-to-MLST-analysis).
# Installation
Currently the only way to install this script is using Conda.
```bash
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
conda install fastmlst
```
## Dependencies
It is expected that all dependencies will be resolved when using conda for installation.
* Python > 3
* Biopython
* tqdm
* pandas
* NCBI BLAST+
# Quick Start
```
$ fastmlst cdiff_refferences/RT078_CDM120.fasta
RT078_CDM120.fasta,cdifficile,11,adk(5),atpA(8),dxr(5),glyA(11),recA(9),sodA(11),tpi(8),mlst_clade(5.0)

$ fastmlst cdiff_refferences/RT078_CDM120.fasta.gz
RT078_CDM120.fasta.gz,cdifficile,11,adk(5),atpA(8),dxr(5),glyA(11),recA(9),sodA(11),tpi(8),mlst_clade(5.0)

$ fastmlst cdiff_refferences/*.fasta
RT001_BI9.fasta,cdifficile,3,adk(1),atpA(1),dxr(2),glyA(1),recA(1),sodA(1),tpi(1),mlst_clade(1.0)
RT001_Liv24.fasta,cdifficile,3,adk(1),atpA(1),dxr(2),glyA(1),recA(1),sodA(1),tpi(1),mlst_clade(1.0)
RT002_TL178.fasta,cdifficile,8,adk(1),atpA(1),dxr(2),glyA(6),recA(1),sodA(5),tpi(1),mlst_clade(1.0)
RT012_CD630_chr_V12.fasta,cdifficile,54,adk(1),atpA(4),dxr(7),glyA(1),recA(1),sodA(3),tpi(3),mlst_clade(1.0)
RT014_TL176_v3.fasta,cdifficile,13,adk(1),atpA(1),dxr(6),glyA(1),recA(5),sodA(3),tpi(1),mlst_clade(1.0)
RT015_TL174.fasta,cdifficile,44,adk(2),atpA(5),dxr(2),glyA(1),recA(1),sodA(3),tpi(1),mlst_clade(1.0)
RT017_CF5.fasta,cdifficile,86,adk(3),atpA(7),dxr(3),glyA(8),recA(6),sodA(19),tpi(11),mlst_clade(4.0)
RT017_M68.fasta,cdifficile,37,adk(3),atpA(7),dxr(3),glyA(8),recA(6),sodA(9),tpi(11),mlst_clade(4.0)
RT023_CD305.fasta,cdifficile,791,adk(65),atpA(1),dxr(4),glyA(7),recA(2),sodA(8),tpi(7),mlst_clade(nan)
RT027_CD196.fasta,cdifficile,1,adk(1),atpA(1),dxr(1),glyA(10),recA(1),sodA(3),tpi(5),mlst_clade(2.0)
RT027_R20291_July2013.fasta,cdifficile,1,adk(1),atpA(1),dxr(1),glyA(10),recA(1),sodA(3),tpi(5),mlst_clade(2.0)
RT078_CDM120.fasta,cdifficile,11,adk(5),atpA(8),dxr(5),glyA(11),recA(9),sodA(11),tpi(8),mlst_clade(5.0)
RT106_Liv22.fasta,cdifficile,42,adk(1),atpA(1),dxr(2),glyA(1),recA(1),sodA(7),tpi(1),mlst_clade(1.0)
```
# Usage
FastMLST uses as input a assembly in FASTA format. Optionally it can be compressed with gzip or bzip2.
```
$ fastmlst cdiff_refferences/RT078_CDM120.fasta
RT078_CDM120.fasta,cdifficile,11,adk(5),atpA(8),dxr(5),glyA(11),recA(9),sodA(11),tpi(8),mlst_clade(5.0)
```
The output is a comma separated file (csv) by default, but it can be modified using the `-s` option.
```
$ fastmlst -s '\t' cdiff_refferences/RT078_CDM120.fasta
RT078_CDM120.fasta      cdifficile      11      adk(5)  atpA(8) dxr(5)  glyA(11)        recA(9) sodA(11)        tpi(8)  mlst_clade(5.0)
```
There are two options for saving the result in a text file:
```
$ fastmlst -to mlst.csv cdiff_refferences/RT078_CDM120.fasta
$ fastmlst cdiff_refferences/RT078_CDM120.fasta > mlst.csv
```
Both options generate the `mlst.csv` file containing the FastMLST result.

FastMLST is able to generate a file in FASTA format with the alleles concatenated in the same way as they are in PubMLST. If any genome is not found in this result, it means that (1) Allele contain Ns, (2) alleles missing or (3) contamination (multiple alleles for one genome). Optionally the name could be modified with `-fo` option:
```
$ fastmlst cdiff_refferences/RT078_CDM120.fasta
```
FastMLST will  try to use all available cores. It can be modified with `-t` option:
```
$ fastmlst -t 2 cdiff_refferences/RT078_CDM120.fasta 
```
You also can specify to FastMLST the scheme name using the `--scheme` option, this is particularly useful when there is more than one scheme per species. If you use this option, it will generate a table with a new format (available since version 0.0.10) which is easier to use in other programs like [phyloviz](http://www.phyloviz.net/).

```
$ fastmlst --scheme cdifficile cdiff_refferences/RT078_CDM120.fasta
Genome,Scheme,ST,adk,atpA,dxr,glyA,recA,sodA,tpi,mlst_clade
RT078_CDM120.fasta,cdifficile,11,5,8,5,11,9,11,8,5.0
```

If you want the old format just add the option `--legacy`:

```
$ fastmlst --legacy --scheme cdifficile cdiff_refferences/RT078_CDM120.fasta
RT078_CDM120.fasta,cdifficile,11,adk(5),atpA(8),dxr(5),glyA(11),recA(9),sodA(11),tpi(8),mlst_clade(5.0)
```

A list of schemes supported is displayed with the option `--scheme-list` in the following format `(n) code_name: Full species name`

**Hint: You must use just the `code_name` in the `--scheme` option.**

```
$ fastmlst --scheme-list
There are 153 schemes (A round of applause to @keithajolley! (Jolley, et al., 2018)):

(1) achromobacter: Achromobacter spp.
(2) abaumannii#1: Acinetobacter baumannii#1
(3) abaumannii#2: Acinetobacter baumannii#2
(n) (...)
(151) xfastidiosa: Xylella fastidiosa
(152) ypseudotuberculosis: Yersinia pseudotuberculosis
(153) yruckeri: Yersinia ruckeri
```

A new option in version v0.0.14 is the possibility to obtain the alleles divided into individual FASTA files (one for each allele in the scheme), ready to be used in other programs such as MLSTest.

```
$ fastmlst --scheme cdifficile cdiff_refferences/*.fasta --splited-output splited_mlst
Genome,Scheme,ST,adk,atpA,dxr,glyA,recA,sodA,tpi,mlst_clade
RT001_BI9.fasta,cdifficile,3,1,1,2,1,1,1,1,1.0
RT001_Liv24.fasta,cdifficile,3,1,1,2,1,1,1,1,1.0
RT002_TL178.fasta,cdifficile,8,1,1,2,6,1,5,1,1.0
RT012_CD630_chr_V12.fasta,cdifficile,54,1,4,7,1,1,3,3,1.0
RT014_TL176_v3.fasta,cdifficile,13,1,1,6,1,5,3,1,1.0
RT015_TL174.fasta,cdifficile,44,2,5,2,1,1,3,1,1.0
RT017_CF5.fasta,cdifficile,86,3,7,3,8,6,19,11,4.0
RT017_M68.fasta,cdifficile,37,3,7,3,8,6,9,11,4.0
RT023_CD305.fasta,cdifficile,791,65,1,4,7,2,8,7,
RT027_CD196.fasta,cdifficile,1,1,1,1,10,1,3,5,2.0
RT027_R20291_July2013.fasta,cdifficile,1,1,1,1,10,1,3,5,2.0
RT078_CDM120.fasta,cdifficile,11,5,8,5,11,9,11,8,5.0
RT106_Liv22.fasta,cdifficile,42,1,1,2,1,1,7,1,1.0
```

```
$ ls splited_mlst/
adk.fasta  atpA.fasta  dxr.fasta  glyA.fasta  recA.fasta  sodA.fasta  tpi.fasta
cat splited_mlst/adk.fasta
>RT001_BI9.fasta adk
CATATATCAACAGGAGATATATTCAGAAAGAATATAAAAGAGGGAACAGAACTTGGAAAA
AAAGCTAAAGAATACATGGACCAAGGTTTATTAGTACCAGATGAGTTAACTGTAGGTTTA
GTTACTGATAGAATATCTCAAGAAGATTGTAAAAATGGATTTATGTTAGATGGATTTCCA
AGAAATGTAGCACAAGGAGAACATTTAGATATCTTCTTAAAAAATGCTGGTATATCACTA
GATAAAGTTGTCAATATTGAAGTTGATAAGAGTATATTAGTGTCTAGAGCAGTTGGTAGA
AGAATATGTAAGTCTTGTGGAGCTACTTACCATGTTGAGTTTAATCCTCCTAAAGTAGAA
GGTGTATGTGATGTATGCCAAGGAGAATTATATCAAAGAGCTGATGATAATGAAGAAACT
GTATCTAAGAGAATACAAGTTTATCTAGATGAAACTAAGCCTTTAGTAGATTATTATAGC
AAACAAGGTATAATAGCAGAT
...
>RT106_Liv22.fasta adk
CATATATCAACAGGAGATATATTCAGAAAGAATATAAAAGAGGGAACAGAACTTGGAAAA
AAAGCTAAAGAATACATGGACCAAGGTTTATTAGTACCAGATGAGTTAACTGTAGGTTTA
GTTACTGATAGAATATCTCAAGAAGATTGTAAAAATGGATTTATGTTAGATGGATTTCCA
AGAAATGTAGCACAAGGAGAACATTTAGATATCTTCTTAAAAAATGCTGGTATATCACTA
GATAAAGTTGTCAATATTGAAGTTGATAAGAGTATATTAGTGTCTAGAGCAGTTGGTAGA
AGAATATGTAAGTCTTGTGGAGCTACTTACCATGTTGAGTTTAATCCTCCTAAAGTAGAA
GGTGTATGTGATGTATGCCAAGGAGAATTATATCAAAGAGCTGATGATAATGAAGAAACT
GTATCTAAGAGAATACAAGTTTATCTAGATGAAACTAAGCCTTTAGTAGATTATTATAGC
AAACAAGGTATAATAGCAGAT
```



## Output symbology

Symbol | Meaning | Length | Identity
---   | --- | --- | ---
`n`   | Exact intact allele                   | 100%            | 100%
`~n`  | Novel full length allele similar to n | 100%            | &ge; `-pid`
`n?`  | Partial match to known allele        | &ge; `-cov` | &ge; `-pid`
`-`   | Allele missing (or allele containing Ns) | &lt; `-cov` | &lt; `-pid`
`n,m` | Multiple alleles                     | &nbsp;          | &nbsp;
## Scoring system
FastMLST uses a scoring system to determine the scheme to be employed similar to that proposed by [Tseemann](https://github.com/tseemann/mlst). The score for a scheme with N alleles is as follows:

* +100/N points for an exact allele match _e.g._ `1`
* +70/N points for a novel allele match _e.g._ `~1`
* +20/N points for a partial allele match _e.g._ `1?`
* 0 points for a missing allele _e.g._ `-`
# Updating the Schemes
You should **always, always, always keep the PubMLST database updated**. Fortunately there is a function to simply update the database:
```
$ fastmlst --update-mlst
```
You can indicate how many schemes will be downloaded in parallel with `-t` option if you want more download speed.
```
$ fastmlst --update-mlst -t 24
```
# Complete usage Options
```
usage: fastmlst [-h] [-t THREADS] [-v {0,1,2}] [-s SEPARATOR] [-sch SCHEME] [--scheme-list] [-fo FASTAOUTPUT]
                [-to TABLEOUTPUT] [-cov COVERAGE] [-pid IDENTITY] [--update-mlst] [-sp SPLITED_OUTPUT] [--fasta2line]
                [--longheader] [--legacy] [-n NOVEL] [-V]
                [genomes ...]

positional arguments:
  genomes

optional arguments:
  -h, --help            show this help message and exit
  -t THREADS, --threads THREADS
                        Number of threads to use (default 12)
  -v {0,1,2}, --verbose {0,1,2}
                        Verbose output level choices: [0, 1, 2]
  -s SEPARATOR, --separator SEPARATOR
                        Choose a character to use as a separator (default ",")
  -sch SCHEME, --scheme SCHEME
                        Set a scheme target (I am not dumb, let me choose a scheme by myself!)
  --scheme-list         Show all schemes supported
  -fo FASTAOUTPUT, --fastaoutput FASTAOUTPUT
                        File name of the concatenated alleles output (default "")
  -to TABLEOUTPUT, --tableoutput TABLEOUTPUT
                        File name of the MLST table output (default STDOUT)
  -cov COVERAGE, --coverage COVERAGE
                        DNA %Cov to report high quality partial allele [?] (default 99%)
  -pid IDENTITY, --identity IDENTITY
                        DNA %Identity of full allelle to consider 'similar' [~] (default 95%)
  --update-mlst         Perform an update of the PubMLST database
  -sp SPLITED_OUTPUT, --splited-output SPLITED_OUTPUT
                        Directory output for splited alleles (default "")
  --fasta2line          The fasta files will be in fasta2line format
  --longheader          If --longheader is invoked, the header of FASTA file contain a long description
  --legacy              If --legacy is invoked, the csv reported contain the gene name and the allele id in the row
                        [adk(1),atpA(4),dxr(7),glyA(1),recA(1),sodA(3),tpi(3)]. This option is only available when the
                        --scheme is defined
  -n NOVEL, --novel NOVEL
                        File name of the novel alleles
  -V, --version         Show program's version number and exit
```
# Citation
If you use FastMLST in your publication, I could recommend the following sentence in methodology:

The ST determination was done using FastMLST (https://github.com/EnzoAndree/FastMLST) in combination with the [PubMLST](https://doi.org/10.12688/wellcomeopenres.14826.1) database. The alleles of the scheme were concatenated in alphabetical order.

It is currently unpublished, but we are doing our best to get this paper out.

For now, you could cite this [pre-print](https://www.biorxiv.org/content/10.1101/2020.10.13.338517v2) where we analyze the speed of FastMLST:

FastMLST: A multi-core tool for multilocus sequence typing of draft genome assemblies.
Enzo Guerrero-Araya, Marina Muñoz, César Rodríguez, Daniel Paredes-Sabja
bioRxiv 2020.10.13.338517; doi: https://doi.org/10.1101/2020.10.13.338517

