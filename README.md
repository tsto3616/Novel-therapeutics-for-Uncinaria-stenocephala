# Novel-therapeutics-for-Uncinaria-stenocephala

## WGS preparation:

60Gb of data in total were processed through SPADES, no trimming was performed due to adequate read error rates. The code for the SPADES alignment is present below:

```         
spades.py -1 /scratch/U_steno_seq/Illumina/JS6871_1.fq.gz -2 /scratch/U_steno_seq/Illumina/JS6871_2.fq.gz /scratch/U_steno_seq/Illumina/JS6871
```

RepeatModeller was utilised to uncover repetitive elements. The repetitive elements are displayed in X file. The code for RepeatModller is:

```         
BuildDatabase -name /scratch/U_steno_seq/Illumina/scaffolds /scratch/U_steno_seq/Illumina/JS6871/scaffolds.fasta

RepeatModeler -pa 16 -database /scratch/U_steno_seq/Illumina/scaffolds > /scratch/U_steno_seq/Illumina/scaffolds-families.fa

cat /scratch/U_steno_seq/Illumina/scaffolds-families.fa | seqkit fx2tab | awk '{ print "abcDef1_"$0 }' | seqkit tab2fx > \
/scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa

cat /scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa | seqkit fx2tab | grep -v "Unknown" | seqkit tab2fx > \
/scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa.known
cat /scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa | seqkit fx2tab | grep "Unknown" | seqkit tab2fx > \
/scratch/U_steno_seq/Illumina/scaffoldss-families.prefix.fa.unknown

grep -c ">" /scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa.known
grep -c ">" /scratch/U_steno_seq/Illumina/scaffolds-families.prefix.fa.unknown
```

The output of repeatmodeler is presented in... Of particular interest to us was the transposable element (TE) Alu, an exclusively primate TE. Further searches were carried out to identify this TE within the genome. A BLASTn search of the genome was carried out for the consensus Alu TE, using the code:

```         
makeblastdb -in /scratch/U_steno_seq/Illumina/JS6871/contigs.fasta -dbtype nucl

blastn -query blastn -query /scratch/U_steno_seq/Illumina/Alu.fa -db /scratch/U_steno_seq/Illumina/JS6871/contigs.fasta -outfmt \
"6 qseqid sseqid sstart send" -out /scratch/U_steno_seq/Illumina/export/blastn_Alu.txt
```

One contig was chosen based on its size (3184bp in length).

## Contig level analyses:

The contig (contig1) was processed through a BLASTx search in NCBI's BLAST. Of the results there were five clear matches, an aminohydrolase (MCA6439020 and MCA2648676), D-alanine–D-alanine-ligase (MCA6448374 and MCA6439022) and a N-acetylglucosamine-6-phosphate deacetylase (MCA6439021). Four of the five results were originating from *Chitinophagaceae bacterium*. These matches were chosen for all amino acids being aligned, a high proportion of similarity (all above 90%) and the absence of gaps. These sequences were imported into CLC MainWorkbench, reverse translated and the resultant sequence aligned to contig1. Only MCA6439021 was aligned in full, so the hypothetical N-acetylglycosamine-6-phosphate deacetylase (NagA) protein of contig1 was determined through the amino acid homology and positioning of the aligned sequence. The sequence started a methionine and was terminated one amino acid before a stop codon. There were no exons. The *nagA* gene was processed through NCBI's BLASTn with no resultant matches, confirming that it is not originating from contamination. Contig1 was also processed through the same steps, the only results originated from primates.

Interested in the primate BLAST results, we further investigated the Alu TE. Searching contig1 against Dfam's database, against both the human genome and *Caenorhabditis elegans* genome. Default settings were used. The results were...

The NagA protein was searched against InterPro's database, resulting in confirmation of the NagA BLASTn result.

## NagA protein level analyses:

An alignment with default settings was performed on NagA and MGA6439021 proteins. The resultant alignment was processed through the "Predict Secondary Structure" option within CLC MainWorkbench. The PDB file for the NagA protein from *U. stenocephala* was retrieved from ColabFold, using default settings. The PBD file (NagA.pdb) was imported into ChimeraX, and visually compared to the *C. bacterium* NagA protein (A0A7J5WQB1).

Following that analyses were conducted in ProteinPlus (CHECK CAPITALS), to produce the PoseView 2D interactive plot.
