# Ref_seq_gtf
Generate genome annotation gtf file for NCBI refseq

I plan to run salmon on my DNA-seq data to get the transcripts abundance. I want to have Refseq version. 

I found the information here: 
https://bioinformatics.stackexchange.com/questions/2548/hg38-gtf-file-with-refseq-annotations

Instead of re-invent wheel, I donwload the refseq gene here:
https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/genes/

# refGene vs refSEQ

The RefGene database was created from the UCSC database. RefGene specifies known human protein-coding and non-protein-coding genes taken from the NCBI RNA reference sequences collection (RefSeq). If you would like to annotate your variants to genes, you can use the simpler refGene database. If you would like to determine the exons that your variants are in, use the refGene_exon database. See the available annotation fields for each database below.

Description of the database from NCBI:

RefSeqGene, a subset of NCBI's Reference Sequence (RefSeq) project, defines genomic sequences to be used as reference standards for well-characterized genes. These sequences, labeled with the keyword RefSeqGene in NCBI's nucleotide database, serve as a stable foundation for reporting mutations, for establishing conventions for numbering exons and introns, and for defining the coordinates of other variations. RefSeq mRNA and protein sequences have long been used for this purpose, but have the obvious weakness of not providing explicit coordinates for flanking or intronic sequence. RefSeq chromosome sequences do provide explicit coordinates no matter the relationship to any gene annotation, but have awkwardly large coordinate values that will change when the sequence is updated because of a re-assembly. Sequences of the RefSeqGene project counter both of these drawbacks by providing more stable gene-specific genomic sequence for each gene, as well as including upstream and downstream flanking regions. If modifications must be made to any RefSeqGene sequence, it will be versioned and tools will be provided to facilitate conversion of coordinates. The RefSeqGene sequences are aligned to reference chromosomes, and current and previous chromosome coordinates are available because of that re-alignment. The Clinical Remap tool make that conversion easy.
Although the majority of UCSC Known Genes (KG) are identical to RefSeq genes, there are some significant differences according to this post:

Not every RefSeq is a KG. Some RefSeqs were filtered out because they did not pass our gene-check processing step (e.g. RefSeqs with no start or stop codons, or bad reading frames are filtered out).
If there is a UniProt protein which maps well to a GenBank mRNA, and it passes the gene-check filter and there is no equal or better corresponding RefSeq, the mRNA/UniProt pair will be added to the KG data set.
UCSC KG is updated once in a few months. Our RefSeq track is updated nightly. So the refGene table may contains some latest RefSeq updates that came after the last KG build.


It turns out the version from UCSC is old. I decided to download Refseq annotation from NCBI directly.  human Release 109

## Obstacle
The seqname from NCBI is the accession number. I need to convert this to numbers 1, 2, 3 ... X, Y, MT etc etc.

## Download human refseq gff and gtf files:
https://www.ncbi.nlm.nih.gov/projects/genome/guide/human/index.shtml

api: https://github.com/NCBI-Hackathons/EDirectCookbook

NCBI ftp site
https://ftp.ncbi.nlm.nih.gov/refseq/

For latest human:
https://ftp.ncbi.nlm.nih.gov/refseq/H_sapiens/annotation/GRCh38_latest/refseq_identifiers/

release 109 
https://ftp.ncbi.nlm.nih.gov/refseq/H_sapiens/annotation/annotation_releases/109.20200228/GCF_000001405.39_GRCh38.p13/

library(readr)
human_refseq_match <- read_delim("human_refseq_match.txt", 
                                 "\t", escape_double = FALSE, trim_ws = TRUE)



#### 
cmd =paste0("sed -e 's/", human_refseq_match$`RefSeq-Accn`, '/', human_refseq_match$`Sequence-Name`, "/g' | \\")
writeLines(cmd, "D:/dicerna/annotations/human/refseq/ncbi2ensembl.sh")

## sort, bgzip, and tabix 
https://github.com/billzt/gff3sort

