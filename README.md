# Our experience in repeat detection

Exaustive identification of de novo (species/taxon-specific) repetitive elements is crucial in thorough detection of repetitive regions in the genome of question. It has become a standard strategy in the genomicists' community, to perform a repeat library construction with RepeatModeler and later execute RepeatMakser using the repeat library constructed by RepeatModeler. Overall performance of repeat detection largely varies depending on the way how these programs are used. This document covers some practical topics that users could consider to achieve the maximal outcome.

## Make sure your RepeatModeler properly demonstrate the '-LTRStruct' option

When RepeatModeler is run, the option `-LTRStruct` is expected to reliably  identified. However, under some particular conditions of RepeatModeder installation, this function is disabled. If the repeat library file 'genome-families.fa' produced by RepeatModeler includes the repeat models whose header line starts with 'ltr-' in addition to those starting with 'rnd-' (like inserted below), your RepeatModeler runs properly with the `-LTRStruct` option. If there is no repeat model with 'ltr-', this means that your RepeatModeler did not run properly with this option. The problem is that RepeatModeler itself just skips the step and continues to run to to the end, producing no explicit error.
```
ATTCTTTGCAAACACTATGATTTTGGCTATCTTGGAGAACAATACAAGGA
ACATAAC
>ltr-1_family-99#LTR/Unknown [ Type=INT, Final Multiple Alignment Size = 1 ]
TGAAAACATAGTTTGACCTGAATAGCACCCACTGAAGCCTGTATCGGAGT
CGGGGAGTGAGAATTCCTGATCACCATTTAGAATGTCGGCCCTTTTTGGT
GTAGGGGGAATTCTAAGAATAGTGGTGAGTTTTCAAAAACA
>ltr-1_family-100#LTR/Unknown [ Type=INT, Final Multiple Alignment Size = 1 ]
TGTAGCCATCTGGGATGGCCACGTCCCGATTACAAAATGGACACTTGAAA
AGAATTCAGGGAAAATTGGACAATGCTAAGAAATAAGCAGGTGCAAGCTC
TGTCTGTTGATTAGAACCTTAAACTCTCAGACAGGACAGA...
>rnd-1_family-103#LINE/L1 ( RepeatScout Family Size = 181, Final Multiple Alignment Size = 100, Localized to 10 out of 10 contigs )
GAATACTCCGCGATCGTNATCTCCGACCACGCTCCACACTACATGGATGT
GAGGTTGGAGACGGGCCGGGCCCAACGCCCCACATGGAGGCTGGACACGG
CCCTCCTGGCCGACAAGGCCTTCTGCGAGAAAATATCGCGGGCCATAGGC
```

To examine if you got right output, open the log file from your RepeatModeler run, and make sure that you can find the line like below on the top.
```
LTR Structural Analysis: Enabled ( GenomeTools 1.6.2, LTR_Retriever v2.9.0,
                                   Ninja 0.95-cluster_only, MAFFT 7.475,
                                   CD-HIT 4.8.1 )`
```
and look for the lines far below in the section 'LTR Structural Analysis'
```
LTR Structural Analysis<
=======================
Running LtrHarvest...sh: line 1: 660113 Segmentation fault      (core dumped) /home/kuraku/programs/genometools-1.6.2/bin/gt ltrharvest -index esa_index -out ltrharvest.out > ltrharvest.log 2>&1<BR>
LtrPipeline: GenomeTools failed to run ltrharvest. Error code: 139
LTRPipeline: No results returned from LTR structural finder ( LtrHarvest ).
LTRPipeline Time: 223:12:40 (hh:mm:ss) Elapsed Time
```

This is an example from my RepeatModeler run in which the LTR identification did not run because of the high abundance of very long sequences. Included below is another example in which inappropriate versions of Genome Tools (v.0.6.5. versus v1.6.2 referred above) and Ninja was used. See here for tips from the developer.
```
LTR Structural Analysis: Enabled ( GenomeTools 0.6.5, LTR_Retriever v2.9.0,
                                   Ninja , MAFFT 7.475,
                                   CD-HIT 4.8.1 )
```                                   
Here is another error produced when RepeatModelder ran with an appropriate version of GenomeTools but not with an appropriate version of ninja. More useful  information can be found in issues raised for the RepeatModeler program at Github
```
LTR Structural Analysis
 =======================

 Running LtrHarvest...     : 26:29:35 (hh:mm:ss) Elapsed Time<BR>
 Running Ltr_retriever...  : 02:41:26 (hh:mm:ss) Elapsed Time<BR>
 Aligning instances...     : 09:41:58 (hh:mm:ss) Elapsed Time<BR>
 Clustering...LTRPipeline: Error - could not cluster MAFFT results.
```
Pasted below is a message in a successful run of LtrHarvest inside RepeatModeler. 
```
LTR Structural Analysis
=======================
Running LtrHarvest...     : 11:50:29 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...INFO: LTRPipeline - redundant library too large ( 34631 ).  Falling back to non-redundant library ( 3770 ).
  : 07:00:45 (hh:mm:ss) Elapsed Time
Aligning instances...     : 01:48:37 (hh:mm:ss) Elapsed Time
Clustering...             : 00:02:11 (hh:mm:ss) Elapsed Time
Refining families...Refiner::buildConsensus(): /home/kuraku/Squalomix-HiFi/storazame/rmasker2/scf1-10/RM_3399123.WedMar80005552023/LTR_799118.WedMar81256002023/ltr-1_family-56.fa ( ltr-1_family-56 ) didn't have any hsps.
      : 02:16:29 (hh:mm:ss) Elapsed Time
Program Time: 22:58:31 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 1810 RepeatScout/RECON families
       - 376 LTRPipeline families
       - Removed 164 redundant LTR families.
       - Final family count = 2022
LTRPipeline Time: 22:58:50 (hh:mm:ss) Elapsed Time
```

## Use more options for RepeatMasker

Consider setting the options `-s` (sensitive mode) and `-nolow` (no masking of low complexity repeats, e.g., in softmasking-aware gene prediction). There are more options such as `-noint` or `-norna`
