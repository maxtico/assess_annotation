# Assess Selenoproteins Annotation

## Table of contents
* [General info](#general-info)
* [Prerequisites](#prerequisites)
* [Usage](#usage)
* [Examples](#examples)
* [Script description](#script-description)
* [Author](#author)
* [Acknowledgements](#acknowledgements)

## General info
This program obtains two tables containing the annotations for all selenoprotein predicted genes from Selenoprofiles of the input genome.

## Prerequisites
These are the requisites for running this code:
* Pyranges version: 0.0.120
* Pandas version: 1.1.5
* Numpy version: 1.19.2
* Easyterm version: 0.7.2

## Usage
To run the script, use the following command:
```
python checking_annotations.py [arguments]
```
The following options are available:
* **`-s`**: Specify the input Selenoprofiles file in GTF or GFF format.
* **`-e`**: Specify the input genome file in GFF3 or GTF format.
* **`-f`**: Specify the input genome Fasta file.
* **`-o`**: Specify the name of the output csv table containing annotation for each Ensembl transcript.
* **`-agg`**: Specify the name of the output csv aggregate table.
* **`-cs`**: Specify the name of the column which will be taken as ID to work with. Default is **transcript_id**.
* **`-cg`**: Specify the name of the column which will be taken as ID to work with. Default is **ID**.

Note that if any the input or output files are not specified the script will crash. For more information, see [Script description](#script-description).

## Examples
Suppose you want to assess selenoprotein annotation in Homo sapiens genome. You obtained a selenoprofiles GTF file with predicted genes called **homo_sapiens_selenoprofiles.gtf**:
```
selenoprofiles_genewise CDS     80211251        80211472        .       -       .       gene_id "DI.3.selenocysteine"; transcript_id "DI.3.selenocysteine";
selenoprofiles_genewise CDS     80202713        80203288        .       -       .       gene_id "DI.3.selenocysteine"; transcript_id "DI.3.selenocysteine";
```
You also contain a GFF3 ensembl file with all Homo sapiens transcripts called **Homo_sapiens.GRCh38.108.gff3**:
```
havana  CDS     80202689        80203288        .       -       0       ID=CDS:ENSP00000451419;Parent=transcript:ENST00000557010;protein_id=ENSP00000451419
havana  CDS     80211251        80211472        .       -       0       ID=CDS:ENSP00000451419;Parent=transcript:ENST00000557010;protein_id=ENSP00000451419
```
To run this script and save the first output to a file named **homo_sapiens.csv** and the aggregate table to a file named **homo_sapiens_aggregate.csv**, you can use the following command:
```
python optimized.py -s homo_sapiens_selenoprofiles.gtf -e Homo_sapiens.GRCh38.108.gff3 -f Homo_sapiens.GRCh38.dna.toplevel.fa -o homo_sapiens.csv -agg homo_sapiens_aggregate.csv
```
The script will process the input files and generate the output tables. **homo_sapiens.csv** table will have the following contents:
```
transcript_id           transcript_id_ens   Type_annotation
DI.3.selenocysteine     ENSP00000451265     Upstream
```
The second output table, **homo_sapiens_aggregate.csv**, will contain the following contents:
```
transcript_id           transcript_id_ens   Type_annotation
DI.3.selenocysteine     ENSP00000451265     Missannotation
```

## Script description
This script has been initially designed to work with Selenoprofiles GTF file format and genome GFF3 file format. If another type of formats are given to these input options, **`-cs`** and **`-cg`** must be provided to run correctly.
It works only with coding sequences (CDS), so 3' and 5' UTRs are discarded. This script assumes each Selenoprofiles ID contains a described *Selenocysteine* position (Genomic intervals, strand...). 
This script compares genomic intervals between Selenoprofiles and genomes. Thus, annotations are based on overlaps between Selenoprofiles predicted genes and genome transcripts.

## Author
**Max Ticó Miñarro**

If you have any questions or feedback about the script, please feel free to email me at:
**__max.tico@alumni.esci.upf.edu__**

## Acknowledgements
This script uses **selenoprofiles4** to generate the input for the script. **selenoprofiles4** is a homology-based in silico method to scan genomes for members of the known eukaryotic selenoprotein families. We would like to thank the developers of **selenoprofiles4** for their contributions to the open source community.
