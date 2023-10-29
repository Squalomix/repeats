# Our experience in repeat detection


Exaustive identification of de novo (species/taxon-specific) repetitive elements is crucial in thorough detection of repetitive regions in the genome of question. It has become a standard strategy in the genomicists' community, to perform a repeat library construction with RepeatModeler and later execute RepeatMakser using the repeat library constructed by RepeatModeler. Overall performance of repeat detection largely varies depending on the way how these programs are used.   This document covers some of the practical topics that users could consider to achieve the maximal outcome.

1. Make sure your RepeatModeler properly demonstrate the '-LTRStruct' option
When RepeatModeler is run, the option '-LTRStruct' is expected to reliably  identified. However, under some particular conditions of RepeatModeder installation, this function is disabled. If the repeat library file 'genome-families.fa' produced by RepeatModeler includes the repeat models whose header line starts with 'ltr-' in addition to those starting with 'rnd-', your RepeatModeler runs properly with the '-LTRStruct' option. If there is no repeat model with 'ltr-', this means that your RepeatModeler did not run properly with this option. 

To analyze this, open the log file from your RepeatModeler run, and make sure that you can find the line like below on the top.

`LTR Structural Analysis: Enabled ( GenomeTools 1.6.2, LTR_Retriever v2.9.0,<br>
                                   Ninja 0.95-cluster_only, MAFFT 7.475,<br>
                                   CD-HIT 4.8.1 )`

and look for the lines far below in the section 'LTR Structural Analysis'

`LTR Structural Analysis<BR>
=======================<BR>
Running LtrHarvest...sh: line 1: 660113 Segmentation fault      (core dumped) /home/kuraku/programs/genometools-1.6.2/bin/gt ltrharvest -index esa_index -out ltrharvest.out > ltrharvest.log 2>&1<BR>
LtrPipeline: GenomeTools failed to run ltrharvest. Error code: 139<BR>
LTRPipeline: No results returned from LTR structural finder ( LtrHarvest ).<BR>
LTRPipeline Time: 223:12:40 (hh:mm:ss) Elapsed Time`

This is an example from my RepeatModeler run in which the LTR identification did not run because of the high abundance of very long sequences. Included below is another example in which inappropriate versions of Genome Tools (v.0.6.5. versus v1.6.2 referred above) and Ninja was used. See here for tips from the developer.

`LTR Structural Analysis: Enabled ( GenomeTools 0.6.5, LTR_Retriever v2.9.0,<BR>
                                   Ninja , MAFFT 7.475,<BR>
                                   CD-HIT 4.8.1 )`
                                   
Here is another error produced when RepeatModelder ran with an appropriate version of GenomeTools but not with an appropriate version of ninja. More useful  information can be found in issues raised for the RepeatModeler program at Github

`LTR Structural Analysis<BR>
 =======================<BR>
<BR>
 Running LtrHarvest...     : 26:29:35 (hh:mm:ss) Elapsed Time<BR>
 Running Ltr_retriever...  : 02:41:26 (hh:mm:ss) Elapsed Time<BR>
 Aligning instances...     : 09:41:58 (hh:mm:ss) Elapsed Time<BR>
 Clustering...LTRPipeline: Error - could not cluster MAFFT results.`

Pasted below is a message in a successful run of LTRharvest inside RepeatModeler. 

`LTR Structural Analysis<BR>
=======================<BR>
Running LtrHarvest...     : 11:50:29 (hh:mm:ss) Elapsed Time<BR>
Running Ltr_retriever...INFO: LTRPipeline - redundant library too large ( 34631 ).  Falling back to non-redundant library ( 3770 ).<BR>
  : 07:00:45 (hh:mm:ss) Elapsed Time<BR>
Aligning instances...     : 01:48:37 (hh:mm:ss) Elapsed Time<BR>
Clustering...             : 00:02:11 (hh:mm:ss) Elapsed Time<BR>
Refining families...Refiner::buildConsensus(): /home/kuraku/Squalomix-HiFi/storazame/rmasker2/scf1-10/RM_3399123.WedMar80005552023/LTR_799118.WedMar81256002023/ltr-1_family-56.fa ( ltr-1_family-56 ) didn't have any hsps.<BR>
      : 02:16:29 (hh:mm:ss) Elapsed Time<BR>
Program Time: 22:58:31 (hh:mm:ss) Elapsed Time<BR>
  -- Clustering results with previous rounds...<BR>
       - 1810 RepeatScout/RECON families<BR>
       - 376 LTRPipeline families<BR>
       - Removed 164 redundant LTR families.<BR>
       - Final family count = 2022<BR>
LTRPipeline Time: 22:58:50 (hh:mm:ss) Elapsed Time`
