###Running RepeatMasker on Drosophila assemblies

#Data
Bed files of RepeatMasker calls for each Drosophila assembly

#Steps:

1. Fastq files were downloaded from https://github.com/danrdanny/Drosophila15GenomesProject/tree/master/assembledGenomes

2. Fastq files were changed to remove '_pilon_pilon_pilon' from contig names as there are too many characters in the original contig names for RepeatMasker to run successfully.
This was done using the following command:
```
#!/bin/bash
gunzip -c $fastqfile | sed 's/_pilon_pilon_pilon//' > $trimmedfastqfile
```

3. Fastq files were then broken up into chunks (divided by number of fastq records) for parallel processing.

4. RepeatMasker was then run on each chunk in parallel using the following command:
```
RepeatMasker -pa 1 -species drosophila -xsmall -poly -gff $chunk
```

5. All output files were then concatenated.

6. RepeatMasker 'out' files (*.rm.out) were then transformed into bed files, and '_pilon_pilon_pilon' was added to the end of contig names to maintain consistency with fasta files.

##Notes

1. Details of RepeatMasker run:

###RepeatMasker version development-$Id: RepeatMasker,v 1.332 2017/04/17 19:01:11 rhubley Exp $

###Search Engine: NCBI/RMBLAST [ 2.2.27+ ]

###Master RepeatMasker Database: /short/te53/software/repeatmasker/4.0.8/RepeatMasker/Libraries/RepeatMaskerLib.embl ( Complete Database: dc20181026-rb20181026 )

###RepeatMasker also uses the Dfam consensus database in its run

2. BED file format (tab-separated):

#Contig name  #Contig_start_position  #Contig_end_position  #Combined_RepeatMasker_Output*  #Perc_div_score  #Strand

*Note: Output from RepeatMasker was merged by ';' in the fourth column (commonly 'Name' column) to retain annotation information in the bed file. 
 	More information about RepeatMasker output can be found in http://www.repeatmasker.org/webrepeatmaskerhelp.html under 'How to read the results'.
