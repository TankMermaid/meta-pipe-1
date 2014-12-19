meta-pipe
=========
Metagenomic pipeline

Usage:
```python
pipeline.py [-d|--inputdir] [-i|-inputfilename] [-o|--outputdir] [-n|--outputfilename] [-p|--phredcutoff 18] [-b|--bwa 1] [-a|--anchor 1] [-r|--derep] [-m|--derepmin 2]
```

Command Line Arguments
----------------------
Example:
```python
pipeline.py -d ./input/ -i 142404_0_input -o ./output -n 142405_out -p 18 -b 0 -a 0-r 1 -m 1
pipeline.py --inputdir ./input/ --inputfilename 142404_0_input --outputdir ./output --outputfilename 142405_out --phredcutoff 18 --derep 1 --derepmin 1
```

-d|--inputdir input directory.

-i|--inputfilename name of input file. Should be a .fastq file. Ex. if the file is named "test.fastq", input "test"

-o|--outputdir output directory.

-n|--outputfilename base name of all output files.

-p|--phredcutoff (used in SolexaQA++ dynamictrim) Phred quality score (between 0 and 41) at which base-calling error is considered too high 

-b|--bwa (used in SolexaQA++ dynamictrim) use BWA trimming algorithm (1 for true/0 for false)

-a|--anchor (used in SolexaQA++ dynamictrim) Reads will only be trimmed from the 3' end (1 for true/0 for false)

-r|--derep (used in dereplication) Type of duplicates to filter. Allowed values are 1, 2, 3, 4 and 5. Use integers for multiple selections (e.g. 124 to use type 1, 2 and 4). The order does not matter. Option 2 and 3 will set 1 and option 5 will set 4 as these are subsets of the other option.
1 (exact duplicate), 2 (5' duplicate), 3 (3' duplicate), 4 (reverse complement exact duplicate), 5 (reverse complement 5'/3' duplicate) 

-m|--derepmin (used in dereplication) This option specifies the number of allowed duplicates. If you want to remove sequence duplicates that occur more than x times, then you would specify x+1 as the -derep\_min values. For examples, to remove sequences that occur more than 5 times, you would specify -derep_min 6. This option can only be used in combination with -derep 1 and/or 4 (forward and/or reverse exact duplicates).

Before you run analysis
-----------------------
Make sure to run this script on a genius by running 'ssh -X super-genius'

Modules that need to be added on coyote ('module add [module name]'):
* SolexaQA++
* rmblast
* rmblast
* bmtools
* srprism
* prinseq-lite
* bowtie2
* python/2.7.8
* numpy/1.6.2


Analysis Process
-----------------
1) Trimming - done using SolexaQA++ dynamictrim.
2) Dereplication - done using prinseq-lite
3) Filtering - done using BMtagger
4) Identifying organisms - done using metaphlan2

For an initial input of 4548554 sequences, the complete analysis took 78 minutes. 

------------
Output files
------------
6 files with the name you provided in -n|--outputfilename with theses suffixes,
 * _1_trim.fastq
 * _2_dereplication_good.fastq
 * _2_dereplication_bad.fastq
 * _3_screening.fastq
 * _4_organism.fastq (contains the ID of each sequence and the corresponding GI number)
 * _4_organism (a percent breakdown of organisms)

in addition to a file called numberOfSequences.txt, which contains the number of sequences left after each step.
