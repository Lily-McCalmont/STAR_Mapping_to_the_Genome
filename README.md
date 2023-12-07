# Team 31: Read Mapping to Genomes using STAR
1. [Background](#311)<br>
2. [History of RNA Sequencing](#312)<br>
3. [Setting Up](#313)<br>
4. [Ways of Fixing the Ambiguous Mapping Problem](#314)<br>
5. [Improving the Process](#315)<br>
6. [STAR Background Information](#316)<br>
7. [Overview of STAR Steps](#317)<br>
8. [Applications](#318)<br>
9. [Potential Errors](#319)<br>
10. [Different Tools for BAM Mapping to Genome](#3110)<br>
11. [References](#3111)<br>

## 1. Introduction<a name="311"></a>

  One of the most common tools for human genome analysis is RNA-sequencing. This tool is used for defining alternative splicing sites and for finding differentially expressed genes. This presentation is about the program used for genome mapping portion of RNA-sequencing and differential analysis; STAR. STAR was created in order to address the pitfalls of high rates of mapping errors, low speed, and limited read lengths found in previous aligner programs. 

## 2. History of RNA Sequencing<a name="312"></a>

  Before we dive into STAR, it is important to look back on the developmental past of RNA-sequencing like microarrays. Microarrays were first used with fluorescent markers in order to identify hybridization events. Although microarrays can identify hybridization events, it cannot help with alternative splicing events, SNP variations, or mutations.  We are briefly mentioning these other technologies since they all helped to contribute to the development of RNA sequencing. STAR is able to help address pitfalls in these previous technologies. 

## 3. Setting Up<a name="313"></a>

  To actually start mapping, we need two things: 
- reads
- reference genome
  
We are going to be mapping the reads to the reference genome. The reason we use a reference genome and not a specifically built genome is because it takes up a lot of memory and time. But there is also another reason. Any differences between the mapped reads and the reference could be attributed to mutations. The next step is to actually map the reads. The initial approach was to brute force pattern matching. Essentially taking the read and moving downstream until a match is found. But while this works well for short genomes and single reads, it doesn’t work for hundreds of reads and a full genome length. If we imagine the genome as a book, and a read as a phrase, what we are essentially trying to do is skim through the book to find the phrase. But what happens if there is a typo in the phrase, or a mutation in the read? One solution we just have is to just tell the program that we are using that it’s okay if there isn’t an exact match. Let's say, for example, that we want a 99% similariy between a read and the genome. This allows for single nucleotide polymorphisms to be kept in, and since the match is mostly exact, we can continue mapping. 

## 4. Ways of Fixing the Ambiguous Mapping Problem<a name="314"></a>

  Another problem that exists in read mapping is ambiguous mapping. Assume that the genome resembles a puzzle, and a read is a puzzle piece. In some cases, a piece can look like it fits into multiple spots. This is essentially what goes on with ambiguous reads. One read could map to multiple locations. (Using the book metaphor from the previous section, one phrase could be present in multiple pages) A way to solve this is to use paired end reads, which are reads that are read both forwards and backwards. Looking at the read from both directions can give us clues as to what spot the read belongs in. Going back to the puzzle metaphor, let’s assume we have this piece below.

![puzzle piece](stock-vector-black-rotated-puzzle-piece-blue-gradient-vector-icon.jpeg)

  If we look at it from just the top, we would put it in the blue area. If we look at it from the bottom, we would put it in the purple area. Looking at it from both directions gives us a better understanding of the location of the piece. Paired end reads work the same way, as they are read both forward and backwards. This gives us clues as to the exact location of the read.

![paired end reads](Figure17.png)

  The next step is to make sure that the reads map well. (This is also useful to see if the correct location is chosen for the ambiguous read). For this we need to calculate a mapping quality, which is the probability that the mapping is incorrect. This is calculated using the individual read base qualities. If the quality is good, then we can feel confident in our mapping location of the read. And it’s this whole process in which STAR comes in.

## 5. Improving the Process<a name="315"></a>

  So how can we fix these mapping problems? We can improve mapping speed. STAR is about 50 fold faster than other current aligners. As shown in the table to the right, STAR does trade this efficiency with RAM usage, but it is significantly faster than other programs.  We can increase the lengths of reads being sequenced. With STAR having its own indexing command we are able to remove the limitations on read length. We can improve on, or more efficiently achieve the same accuracy as previous programs. 

## 6. STAR Background Information<a name="316"></a>

  STAR stands for spliced transcripts alignment to a reference (genome). Universally, this is a program used for alignment. It creates an indexed genome in order to map faster. STAR is especially useful for finding non-canonical splice sites, chimeric sequences, and can map full length RNA sequences. 

## 7. Overview of STAR Steps<a name="317"></a>

  In the process of RNA-seq analysis using the STAR method, several steps occur. Initially, we first have to index the reference genome extracted from FASTA/FASTQ files. This is critical to facilitate the alignment of RNA-seq reads in the next steps. The second step entails the alignment of these reads to the previously indexed reference genome. This step allows for the fine-tuning of parameters such as read length and sequence type to optimize alignment accuracy. Once the alignment is completed, the resulting files, typically in SAM or BAM format, are ready for a comprehensive analysis. Beyond the alignment data, valuable information such as mapping statistics summaries, spliced junctions, and details on unmapped sections enrich the dataset, providing a more holistic perspective for subsequent in-depth exploration and interpretation of RNA-seq results.

## 8. Applications<a name="318"></a>

  One application for STAR read mapping is that we can identify mutations and variants. If there are enough small differences in the lineage we are studying, then we can say that it is a variant of the original lineage. These variants can then be studied to make vaccines, if the genome that we are studying is a virus. This is clearly shown today when looking at covid. Another application is identifying gene expression levels. If one gene is expressed in greater levels than before, then with read mapping, we can identify the sequence that corresponds to said gene. Once that is identified, we can then conduct experiments to figure out what exactly that read does for gene expression. This could be applied to studying various diseases which cause, or are caused by, changes in gene expression levels. 

## 9. Potential Errors<a name="319"></a>

  While STAR is a very useful tool for RNA-seq analysis, several potential errors and challenges should be considered. One issue arises from updates to reference genomes, as modifications or additions can be incompatible with previously mapped BAM files. This process can be particularly difficult for large datasets, negatively affecting the efficiency of the analysis pipeline. Another challenge pertains to compatibility issues with downstream analysis tools. The immediate BAM/SAM output of certain remapping tools may not seamlessly integrate with subsequent analytical platforms. Additionally, errors in the reference genome itself can compromise the precision of the mapping process. Inaccuracies in the genome sequence can lead to misalignments, therefore also affecting downstream analyses such as variant calling (which is the process of identifying and categorizing genetic variations) and gene expression analysis.

## 10. Different Tools for BAM Mapping to Genome<a name="3110"></a>

  Besides STAR, here we can see a short summary of it in contrast to two other tools, there are other options that have their own strengths and weaknesses. Another such tool is BWA ( or Burrows-Wheeler Aligner), designed for mapping low-divergent sequences (which are the sequences that share a significant similarity with the reference genome) against large reference genomes. Its versatility extends to both short and long reads, making it adaptable to a range of sequencing applications. BWA comprises three algorithms for different read lengths and alignment scenarios. Another tool is Bowtie and its successor Bowtie 2 which are short sequence mapping tools. While Bowtie is suitable for shorter reads, Bowtie 2 accommodates longer reads exceeding 50 base pairs and offers additional features.

## 11. References<a name="3111"></a>

- Keisaris, Sofoklis. “Galaxy Training: RNA-Seq Alignment with Star.” 
Galaxy Training Network, Galaxy Training Network, 3 Nov.
2023, 
training.galaxyproject.org/training-material/topics/transcriptomics/tut
orials/rna-seq-bash-star-align/tutorial.html#alignment-to-a-reference-genome.
- Dobin, Alexander, et al. “Star: Ultrafast Universal RNA-Seq Aligner.” 
Bioinformatics 
(Oxford, England), U.S. National Library of Medicine, 1 Jan. 2013, 
www.ncbi.nlm.nih.gov/pmc/articles/PMC3530905/. 
- Uhrig, Sebastian. “Workflow.” Arriba, 
arriba.readthedocs.io/en/latest/workflow/. Accessed 27 
Nov. 2023. 
- Star Manual 2.7 - Cornell University, 
physiology.med.cornell.edu/faculty/skrabanek/lab/angsd/lecture_notes/
STARmanual.pdf. 
Accessed 27 Nov. 2023. 
- Koboldt, Daniel C, et al. “Challenges of Sequencing Human Genomes.” Briefings in Bioinformatics, 
U.S. National Library of Medicine, Sept. 2010, 
www.ncbi.nlm.nih.gov/pmc/articles/PMC2980933/. 
- Mapping with STAR (biocorecrg.github.io)
- Emms, David M., and Steven Kelly. “Orthofinder: Phylogenetic Orthology Inference for Comparative
 Genomics - Genome Biology.” BioMed Central, BioMed Central, 14 Nov. 2019, 
genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1832-y. 
- Burrows-Wheeler Alignment (BWA) - Research Computing Documentation, 
wiki.rc.usf.edu/index.php/Burrows-Wheeler_Alignment_(BWA). Accessed 27 Nov. 2023.


