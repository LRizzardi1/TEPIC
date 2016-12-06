#TEPIC
------
Annotation of genomic regions using Transcription factor (TF) binding sites and epigenetic data.

##Introduction
*TEPIC* segments the genome into user specified regions and annotates those with TF binding using TRAP (1). 
These predictions are aggregated to gene scores. 
Within this aggregation TEPIC offers exponential decay (2) and scaling of TF region scores using the signal of an open chromatin assay.

##Using TEPIC
To start TEPIC, run the script *TEPIC.sh*

    ./TEPIC.sh

The following parameters are required to run TEPIC:

* -g The reference genome. This must be in RefSeq format (without "chr" prefix).
* -b Regions the user want to be annotated.
* -o Prefix of the output files.
* -p File containing position weight matrices (PWMs) in Jaspar format.

The optional parameters are:

* -a Genome annotation file (gtf). All genes contained in this file will be annotated. The file must have the original format provided by gencode. 
* -w Size of the window around the TSS of genes.
* -d Signal of the open chromatin assay in bg format. Used to compute the average per peak coverage within the regions specified in -b
* -e Deactivates the exponential decay
* -n Indicates that the file in -b contains the average signal in the peaks in the specified column. In this case the -d option is not required to obtain scaled TF affinities.
* -c Number of cores used within TRAP.
* -f A gtf file containing genes of interest. Only regions contained in the file specified by the -b option that are within the window specified by the -w option around these genes will be annotated.

Depending on the used arguments, TEPIC produces files containing:

* TF affinities for all user specified regions.
* Scaled TF affinities for all user specified regions.
* TF affinities for all genes contained in the annotation file.
* Scaled TF affinities for all genes contained in the annotation file.
* A file holding all regions which were annotated.
* A file containing the factors used to scale the original TF affinities.

Each run of TEPIC generates an *analysis meta datafile (amd)* containing all parameters, files, and outputs associated with the last run of TEPIC.
Together with the provided process xml file, the executed command lines  can be reconstructed (3). We provide amd files in the folder
*MetaData*. These correspond to the gene scores of the *50kb* and *50kb-S* annotation introduced in the *TEPIC* manuscript.

* Python (minimum version 2.7)
* [bedtools](https://github.com/arq5x/bedtools2)
* A g++ compiler supporting openmp to use the parallel implementation of TRAP.

To compile the C++ version of TRAP execute the script
	[Code/compileTRAP.sh](Code/compileTRAP.sh).

##Example
To run a test trial of *TEPIC*, you can use the data provided in the *Example* folder. You can run it with the command

	./TEPIC.sh -g ../Example/example_sequence.fa -b ../Example/example_regions.bed -o TEPIC-Example -p pwm_vertebrates_jaspar_uniprobe_original.txt -a ../Example/example_annotation.gtf -w 3000 -e

This will generate gene scores for the genes contained in *example_annotation.gtf*, using a window of size 3000bp, all pwms contained in *pwm_vertebrates_jaspar_uniprobe_converted.txt*, and without 
exponential decay. 

##Provided files
The position weight matrices used in the article are stored in the file
	[Code/pwm_vertebrates_jaspar_uniprobe_original.txt](Code/pwm_vertebrates_jaspar_uniprobe_original.txt).
An extended set of pwms is also available:
	[Code/pwm_vertebrates_jaspar_uniprobe_hoc_extended.txt](Code/pwm_vertebrates_jaspar_uniprobe_hoc_extended.txt)

##Citation
If you are using TEPIC please cite:

**Combining transcription factor binding affinities with open-chromatin data for accurate gene expression prediction**
Florian Schmidt et al.
Nucleic Acids Research 2016; doi: 10.1093/nar/gkw1061 [full text](http://nar.oxfordjournals.org/content/early/2016/11/29/nar.gkw1061.full) 

Other works that have influenced ours:
> (1) Predicting transcription factor affinities to DNA from a biophysical model, Roider HG, et al., Bioinformatics, 2007.

> (2) ChIP-Seq of transcription factors predicts absolute and differential gene expression in embryonic stem cells, Ouyang Z, et al.,  PNAS, 2009.

> (3) A general concept for consistent documentation of computational analyses, Ebert P, et al.,  Database, 2015.
