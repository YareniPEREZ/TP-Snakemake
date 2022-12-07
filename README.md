# TP-Snakemake
Yareni DURANTHON (née PEREZ)

## Project TP-Snakemake
Identify the genomic regions accessible by the DNA during the transcription. In order to do this, we re-analyzed the ATAC-seq results from a publication of 2019 ("STATegra, a comprehensive multi-omics dataset of B-cell differentiation in mouse" David Gomez-Cabrero et al. 2019 https://doi.org/10.1038/s41597-019-0202-7).

The analyse was performed on a B3 murine cellular line from the mouse model the pre-B1 stage.
After the nuclear translocation of the transcription factor Ikaros, those cells grow to the pre-BII stage. During this stage, the B cells progenitor are subject to a growth arrest and a differentiation. This B3 cell line was transduced by a retroviral pathway with a vector coding for a fusion protein, Ikaros-REt2, which can control nuclear levels of Ikaros after exposition to the Tamoxifen drug. After this treatment, the cultures were collected at t=0h and t=24h.

## Objective
The main objective is to introduce you to a workflow language and its interest
compared to building a rudimentary workflow built in bash ; then adopt the FAIR
principle for a reproducible science and finaly handling the ressources of the
infrastructure to optimize your codes. To built a build a workflow in Snakemake allowing
you to reproduce the analysis conducted in the ATACseq project from the quality
control of the fastq.gz sequences to the identification of regions of accessibility to
the DNA in the 2 biological conditions .

## Experimental design 
About 50 000 cells were collected for each sample. There are 3 biological replicates by sample : R1, R2 and R3 Each of the biological replicates was performed for the two cellular stages : 0h and 24h In total, 6 samples were studied (3 replicates for t=0h and 3 replicates for t=24h).
Each of these samples were sequenced by an Illumina sequencing with Nextera-based sequencing primers. Considering that the sequencing was performed in paired-end, each of the 6 samples has a forward result file and a reverse result file. So, in total, 12 samples were collected for the analysis.

# WORKFLOW

## First quality control 
we start by performing a quality control on the raw dataser with the objective to see the general qualitu of our data. we create an array of elements wich contains ow seauencing data (1 Forward strand 2 Reverse strand) and the the fonction fastqc will be perform in each of the elements of the array 

Bioinformatique tool : Fastqc  a tool for multi-genome mapping and quality control 

## Adapter's elimination 
after the performance of the first quality control we need to eliminate the sequencing primers, in order to not have bias in the results. For this we ill use the trimommatic function on each elemnt of the array. 

Bioinformatique tool: Flexible trimmer for illumina sequence data. 

## Second Quality control 
After the triming we must permorf a second quality control to compare the modification of our data after the trimming 

Bioinformatique tool: Fastqc  a tool for multi-genome mapping and quality control 


## Mapping 
When we are sure our sequence has a good quality level we will sincronize the forward and reverse files of each sample. this will be performe by a mapping of our sequence with a reference genome Mus_musculus_GRCm39, for this we will use the tool Bowtie2. 
after this we will obtain two different arrays of 6 elemnts each one, one of them will contain all the forwad files and the other one will contain all the reverse files.

Bioinformatic tool: Bowtie2

## supresion of duplicates 
As a result of the Bowtie2 tool, we get the alignement of the sequences on the reference genome. However, there is some duplicates that can bias the analysis later. That's why we will remove these duplicates thanks to the MarkDuplicates function from the picard module.

Bioinformatic tool : Picards module, fonction MarkDuplicates 

## Data exploration
After havinf our results, we will perform some statistical analysis with the objective to see if we hace a correlation between the samples for this all the input files will be combined. 

Bioinformatic tool: DeepTools

## Identification of DNA access sites
After perfom our statistical analysis we will identify the acces sites of the DNA, this will be permormed by the function callPeaks from the module MACS2, the results will be represented like a picks wich are the accesible regions of the DNA . 

Bioinformatic tool: Macs2 module, function callPeack 

## Identification of common and uniques DNA acces sites
After obtained the acces sites we need to see the acces regions for the t=0h and the t=24h results, we will observe the common regions and the uniques resgions for each condition for this we will use the module BedTools with the fonction intersec. 

## Visualization with IGV tool 
we can observe the impact of the Tamoxifen by using IGV tool to see the sites that are unique and common between our two conditions. 

## Run the program
You can run all the steps of the workflow with the Snakefile by launching the command : snakemake --cores all --use-conda --snakefile scripts/Snakefile

### Conclusion
The tamoxifene has an effect of close the DNA access sites 

