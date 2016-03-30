# GALAXY WORKSHOP on ChIP-seq DATA ANALYSIS

##Useful literature 

**ChIP-seq in general:**  


**Landt et al. (2012):** [ChIP-seq guidelines and practices of the ENCODE and modENCODE consortia](http://genome.cshlp.org/content/22/9/1813.long), (doi:10.1101/gr.136184.111) - This is a very useful "encyclopedic" paper with many details about the tools the (mod)ENCODE consortia use. It also contains a long section about antibody validation etc.. It does not explain much of the reasoning behind the bioinformatics tools, though.

**Zentner and Henikoff (2012):** [Surveying the epigenomic landscape, one base at a time](http://genomebiology.biomedcentral.com/articles/10.1186/gb-2012-13-10-250), (doi:10.1186/gb-2012-13-10-250) - Overview of popular *-seq techniques; very nice description of DNase-seq, MNase-seq, FAIRE-seq etc.

**Kidder et al. (2011):** [Technical considerations to obtaining high-quality data](http://www.nature.com/ni/journal/v12/n10/abs/ni.2117.html), (doi:10.1038/ni.2117) - Nice, readable introduction into all aspects of ChIP-seq experiments (from antibodies to cell numbers to replicates to data analysis)

**Leleu et al. (2010):** [Processing and analyzing ChIP-seq data](http://www.ncbi.nlm.nih.gov/pubmed/20861161), (doi: 10.1093/bfgp/elq022) - Fairly detailed review of key concepts of ChIP-seq data processing (less detailed on analysis)

**Peter Park (2009):** [ChIP-seq: Advantages and challenges of a maturing technology](http://www.nature.com/nrg/journal/v10/n10/full/nrg2641.html), (doi:10.1038/nrg2641)

**Kharchenko et al. (2008):** [Design and analysis of ChIP-seq experiments for DNA-binding proteins](http://www.ncbi.nlm.nih.gov/pubmed/19029915), (doi:10.1038/nbt.1508)

**Liu et al. (2010):** [Q&A: ChIP-seq technologies and the study of gene regulation](http://bmcbiol.biomedcentral.com/articles/10.1186/1741-7007-8-56), (doi:10.1186/1741-7007-8-56) - Short overview of several (typical) issues of ChIP-seq analysis

**Carroll et al. (2014):**  [Impact of artifact removal on ChIP quality metrics in ChIP-seq and ChIP-exo data](http://journal.frontiersin.org/article/10.3389/fgene.2014.00075/full),(doi:10.3389/fgene.2014.00075)  


**Peak Calling Methods (ChIP-seq)**

**Pepke et al. (2009):** [Computation for ChIP-seq and RNA-seq studies](http://www.ncbi.nlm.nih.gov/pubmed/19844228), (doi: 10.1038/nmeth.1371) - First comparison of peak callers, focuses on the explanation of basic principles of ChIP-seq data processing and general workflows of peak calling algorithms

**Wilbanks et al. (2010):** [Evaluation of Algorithm Performance in ChIP-Seq Peak Detection](http://www.ncbi.nlm.nih.gov/pubmed/20628599), (doi: 10.1371/journal.pone.0011471) - Another comparison of peak callers - focuses more on the evaluation of the peak callers performances than Pepke et al. (2009)

**Micsinai et al. (2012):** [Picking ChIP-seq peak detectors for analyzing chromatin modification experiments](http://www.ncbi.nlm.nih.gov/pubmed/22307239), (doi: 10.1093/nar/gks048) - How to choose the best peak caller for your data set - their finding: default parameters, surprisingly, yield the most reproducible results regardless of the data set type

**MACS**

**Fen et al. (2012):** [Identifying ChIP-seq enrichment using MACS.](http://www.ncbi.nlm.nih.gov/pubmed/22936215), (doi:10.1038/nprot.2012.101) - How to use MACS - Nature Protocols

**Zhang et al. (2008):** [Model-based Analysis of ChIP-Seq (MACS)](http://genomebiology.biomedcentral.com/articles/10.1186/gb-2008-9-9-r137), (doi:10.1186/gb-2008-9-9-r137) - The original publication of MACS


##Slides from Workshop


The slides for part 1 of this session can be downloaded from here 
[ChIP-seq1-galaxy_course_2015.pdf](https://drive.google.com/open?id=0B9urRnOAUUI8UmwzbTVpdmZucWM)

The slides for part 2 can be downloaded from here 
[ChIP-seq2-galaxy_course_2015.pdf](https://drive.google.com/open?id=0B9urRnOAUUI8cHpzYVBscjNKWEE).  


# Hands on!  

This exercise uses the dataset from the  Nature publication [Ross-Inness et al.2012](http://www.ncbi.nlm.nih.gov/pubmed/22217937). There are 8 samples, half of them are the so-called 'input' data for which the same treatment as the ChIP-seq samples is done but except for the IP. The input files are used to identify sequencing bias like open chromatin or GC bias. For each ChIP-seq experiment there is a matching input. The ChIP was performed to identify the gene targets of the Oestrogen receptor, a transcription factor known to be associated with different types of breast cancer.  For this, ChIP-seq was done in breast cancer cells from 4 patients of different outcomes (good and poor).

Because processing time for the large files associated to the study we have selected small samples for practice and provide already processed data for subsequent steps.

**Step 1: Quality control**

Create a new history for this exercise.

- Import to the history the [FASTQ file patient1_input_good_outcome_chr11_25k_reads.fastq](https://github.com/bgruening/training_data/raw/master/ChIPseq/Galaxy1-%5Bpatient1_input_good_outcome_chr11_25k_reads.fastq%5D.fastqsanger).

- Take a look at the file pressing the 'eye' icon. There is a lot of text, but can you spot where is the DNA sequence stored? Can you guess what the other information means?

- Run the tool *FastQC* on one of the two FASTQ files to control the quality of the reads. An explanation of the results is found in the [fastQC web page](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/). 

**Step 2: Trimming and clipping of reads**

- Apply the tool *Trim Galore* to your FASTQ file. First, view the full parameter list and tell Trim Galore to trim low quality bases with a cut-off of 15 and clip a possibly trailing adapter sequence from the reads. The default sequence that appears is the generic part of Illumina adaptors and should be used. Instruct Trim Galore to trim adapter sequences with an overlap of at least 3. 

-> If your FASTQ files cannot be selected, you might check whether their format is FASTQ with Sanger-scaled quality values (fastqsanger). Edit the data type by clicking on the pencil item.


**Step 3: Mapping of the reads**

- Run Bowtie2 to map the single-end reads to the human (GRCh38) genome.

- By clicking over the resulting history entry basic mapping statistics can be seen. How many reads where mapped?


**Step 4: Visualization of the reads in IGV**

- Load the reads into the IGV browser by clicking the option 'display with IGV local'. To see the reads in IGV you will need to zoom in the start of chromosome 11. Try this region if you don't see any reads: chr11:1,562,200-1,591,483

- Notice that the reads have a direction. By hovering over a read extra information is displayed. Some reads have lines over them. Try to zoom in in one of those lines to identify the reason for this. 

Note: because the number of reads over a region can be quite large, the IGV browser by default only allows to see the reads that fall in a short window. In the preferences panel this behaviour can be changed.


**Step 5: Inspection of the SAM format**

- Go back to Galaxy and run the tool *BAM-to-SAM* using the BAM file that was created in step 2. Click 'include header in output'. Because the BAM file is a compressed format it's contents can not be directly seen unless is converted to SAM (Sequence Alignment Map) format.

- Click on the view icon ('eye'). The first part of the file, the headers, contain the chromosome names and lengths. After the header, the location and other information of each read found on the FASTQ file is given.


**Step 6: Correlation between samples**

- For this step we need to load into the history the mapped files (.bam files) in the shared libraries. For this, please import into the current history all BAM files found in Galaxy courses -> Hands-on.
 
*bam files examples* -> link

- Next, run the tool *multiBamSummary* from the deepTools package. Select only the 8 bam files imported into the history. 

- Use as fragment length: 100 and, to reduce the computation time, set the distance between bins to 500000 and choose only one chromosome, for example 'chr1'.

- After the computation is done, run *plotCorrelation* from the deepTools package to visualize the results. Feel free to try different parameters.


**Step 7: GC bias**

- Use the tool *computeGCbias* to check if the samples have more reads from regions of the genome with high GC. For practical reasons select only one of the bam files available, preferably an input file.

- For fragment size select 300 and limit the operation to only one chromosome.

Does this dataset have a GC bias?


**Step 8: IP strength**

- To evaluate the quality of the immuno-precipitation step use the tool *plotFingerprint*. Please select one of the ChIP-samples and the matching input. Set as fragment size 100 and limit the operation to only one chromosome.

What do you think about the quality of the IP for this experiment?


**Step 9: Generate coverage files normalized by sequencing depth**

- If you have not done already, please import to the history the BAM files patient4_ChIP_ER_poor_outcome.bam and patient4_input_poor_outcome.bam. These files are provided in the data library under Galaxy courses -> ChIP-seq -> bam_files.

- The first file contains the reads from a ChIP-seq experiment for the transcription factor ER using human cells from a breast cancer patient. The second file contains the reads from the matching input sample. The sequencing reads were mapped to the human genome assembly hg18.

- Use the SAM Tools *IdxStats* tool to find out how many reads mapped to which chromosome. Furthermore, this will also tell you the chromosome lengths and naming convention (with or without 'chr' in the beginning).

- Run the tool *bamCoverage* to generate a signal coverage file for the ER ChIP sample normalized by sequencing depth. Set the fragment size to 100 and the bin size to 25. Normalize to 1x genomic coverage. The output file should be in human-readable format bedGraph. To speed up computation, limit the operation to chromosome 'chr11'.

Generally, you should adjust the effective genome size according to the used genome assembly. In our case, you however have to specify the size of chromosome chr11 only when limiting the computation to this region. We obtained this value just in the last step :-)

- Inspect the bedGraph output file.

- Re-run the tool and generate a *bigWig* output file. Inspect the signal coverage in IGV. Remember that the bigWig file contains only the signal on chromosome 11!


**Step 10: Generate input-normalized coverage files**

- Run the tool *bamCompare* to normalize the ChIP signal BAM file patient4_ChIP_ER_poor_outcome.bam by the input control provided by patient4_input_poor_outcome.bam.

- Set the fragment size to 100 again and the bin size to 50. Compute the log2 ratio of the read counts of ER ChIP vs. input sample. The output file should be in human-readable format bedGraph. To speed up computation, limit the operation to chromosome 'chr11'.

- Inspect the bedGraph output file.

- Re-run the tool and generate a bigWig output file. Inspect the log2 ratio in IGV. Remember that the bigWig file contains only the signal on chromosome 11!


**Step 11: Call enriched regions (peaks)**

To speed up computation, we will now restrict our analysis to chromosome 11. 

- To this end, please import to the history the BAM files patient4_ChIP_ER_poor_outcome_chr11.bam and patient4_input_poor_outcome_chr11.bam. These files are provided in the data library under Galaxy courses -> ChIP-seq -> bam_files.

- Use the MACS2 callpeak tool to call peak regions from the alignment results in these two BAM files. Adjust the effective genome size to *Homo sapiens* genome assembly hg18, chromosome 11. Output the peaks as BED file and the HTML summary page.

You might adjust the advanced options depending on your sample:

-> If your ChIP-seq experiment targets regions of broad enrichment, e.g., non-punctuate histone modifications, select calling of broad regions.
-> If your sample has a low duplication rate (e.g. below 10%), you might keep all duplicate reads (tags). Otherwise, you might use the 'auto' option to estimate the maximal allowed number of duplicated reads per genomic location.

- Inspect the result files. When visualized in IGV, do the called peaks match your expectation based on the signal coverage and log2 ratio tracks?

The called peak regions can be filtered by, e.g., fold change, FDR and region length for further downstream analysis.


**Step 12: Visualize your ChIP-seq results as heatmap**

For the last exercise, we will use the data resulting from step 10.

- If you have not done so, please generate a bigWig file with the log2 ratio of ER ChIP over input signal restricted to chromosome 'chr11' as described in step 10.

- Furthermore, please download the following human gene annotation from **UCSC Table Browser**:
Human genome assembly hg18, RefSeq Genes track, limited to region chr11. Download the data as BED file and send it to Galaxy. This will import to your history the regions of all genes on chromosome 11 that are annotated in RefSeq.

- Now, run the *computeMatrix* tool to prepare a file with scores per genomic region, which is required as input for the next tool.

Use the regions provided by the gene annotation file downloaded from UCSC and your log2 ratio score bigWig file obtained by bamCompare in step 10 as input. If you have trouble finding the correct input data: better rename datasets with self-explanatory names the next time ;-)

- You can run computeMatrix in two alternative modes:

-> scale-regions: stretches or shrinks all regions in the BED file (here: genes) to the same length (bp) as indicated by the user
-> reference-point: considers only those genomic positions before (downstream) and/or after (upstream) a reference point (e.g. TSS, which corresponds to the annotated gene start in our case).

- Run the tool in either mode. Ensure that the advanced option 'Convert missing values to 0?' is activated.

- Generate a heatmap with the *plotHeatmap* tool based on your output of *computeMatrix*. Set the advanced option 'Did you compute the matrix with more than one groups of regions?' appropriately.

- How can you increase the "resolution" of the data in the resulting heatmap?