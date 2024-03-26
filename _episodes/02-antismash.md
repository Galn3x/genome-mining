---
title: "Secondary metabolite biosynthetic gene cluster identification"
teaching: 20
exercises: 10
questions:
- "How can I annotate known BGC?"
- "Which kind of analysis antiSMASH can perform?"
- "Which file extension accepts antiSMASH?"
objectives:
- "Understand antiSMASH applications."
- "Perform a Minimal antiSMASH run analysis."
- "Explore several *Streptococcus* genomes by identifying the BGCs presence and the types of secondary metabolites produced."
keypoints:
- "antiSMASH is a bioinformatic tool capable of identifying, annotating and analysing secondary metabolite BGC"
- "antiSMASH can be used as a web-based tool or as stand-alone command-line tool"
- "The file extensions accepted by antiSMASH are GenBank, FASTA and EMBL"
---
## Introduction

Within microbial genomes, we can find some specific regions that
take part in the biosynthesis of secondary metabolites, these
sections are known as Biosynthetic Gene Clusters, which are relevant
due to the possible applications that they may have, for example:
antimicrobials, antitumors, cholesterol-lowering, immunosuppressants,
antiprotozoal, antihelminth, antiviral and anti-aging activities.

Natural products
Natural products (NPs) are organic molecules with a low molecular weight and a diverse range of chemical entities. They have multiple biological functions and can be produced by bacteria, fungi, plants, and animals. NPs are generally derived from specialized metabolism, also known as secondary metabolism, in the case of bacteria and fungi. Although NPs are not considered essential for cell growth, development, and reproduction, they confer adaptive advantages that increase the chances of survival, which is why they are produced under specific physiological conditions, usually when facing unfavorable environmental conditions, such as nutritional scarcity or the presence of other organisms 1,2.
Currently, there are over 126,000 NPs that have been discovered from various sources, including plants, bacteria, fungi and some animals 3. NPs play varied roles, which can be analyzed from two perspectives: 1) biological function, that is, the role that the NP plays in the producing organism, and 2) anthropocentric function, which refers to the usefulness that humans find in NPs for their own benefit. Examples of these utilities include their use as antibiotic agents, anticancer agents, immunosuppressants, insecticides, or herbicides. The organisms that are most capable of producing PN among bacteria belong to the Actinomycetota phylum, specifically the Streptomyces genus. They are responsible for producing two-thirds of all known antibiotics, as well as many anticancer agents, antifungals, and immunosuppressants. This makes them highly relevant to the medical industry, agriculture, and biotechnology. 
In an effort to combat the rise of multidrug-resistant pathogens, the biotechnology industry has been exploring compounds to find molecules with active antibiotic properties. However, the discovery of new drugs has decreased over the past 25 years due to factors such as high costs for clinical trials, a short window before products become generic, and the difficulty in finding antibiotics that are effective against resistant organisms. Despite these challenges, progress has been made in the development of new drugs through focused efforts in fields such as combinatorial chemistry, the discovery of greater biodiversity, systems biology, and genome mining.

Genome Mining
The term 'genomic mining' refers to the California gold rush of 1849 (Nett, 2014), a historical period in which a large number of immigrants arrived in the surroundings of San Francisco, California, in search of of gold in river sediments. Similarly, we can consider that within the thousands of sequenced genomes available (river sediment) there is enormous wealth in the form of NPs (gold) producing genes that we can take advantage of to obtain great benefits.
Genomic mining is a modern bioinformatics technique that involves analyzing nucleotide sequence data through computational methods. This technique is based on comparing and recognizing conserved patterns, which enables the search and prediction of physiological or metabolic properties. In the context of identifying natural products (NPs), genomic mining specifically focuses on identifying NP biosynthetic gene clusters.
A biosynthetic gene cluster (BGC) is a set of genes whose functions are associated with the biosynthesis, regulation, and transport of a certain metabolite. In bacteria, the genes that make up a BGC are generally organized contiguously at the same chromosome locus (Fig.?).
Biosynthetic genes form a cluster that functions as an assembly line. Each enzyme encoded by these genes performs a specific function, such as synthesis, ligation, dehydrogenation, oxidation-reduction, transfer, epimerization, or cyclization. These functions together shape the final structure of a given metabolite. Based on the functions of biosynthetic genes, their combination, and substrate affinity, BGCs are classified. If we compare several clusters, we can observe that certain genes are always present in the BGCs of the same class, known as class-specific genes. These genes determine the basic structure of the final natural product. These molecules have been grouped into six major categories, namely polyketides (PKs), non-ribosomal peptides (NRPs), ribosomally synthesized and post-translationally modified peptides (RiPPs), saccharides, terpenes, and alkaloids (Table 1). 
Accessory genes, on the other hand, are the surrounding biosynthetic genes that modify the base structure by adding different functional groups or changing its conformation. Regulatory genes are responsible for producing proteins that control the expression of BGC, either partially or completely. These genes get activated due to environmental stimuli or changes in cell development. Transport genes, on the other hand, produce proteins that facilitate the transportation of metabolites inside or outside the cell. Promoters are non-coding regions that act as anchor sites for the RNA polymerase enzyme, which eventually initiates the transcription process. The orientation and position of the promoters determine which of the two DNA strands will function as the template for messenger RNA synthesis.




antiSMASH is a pipeline based on
[profile hidden Markov models](https://www.ebi.ac.uk/training/online/courses/pfam-creating-protein-families/what-are-profile-hidden-markov-models-hmms/#:~:text=Profile%20HMMs%20are%20probabilistic%20models,the%20alignment%2C%20see%20Figure%202)
that allow us to identify the gene clusters contained within the genome
sequences that encode secondary metabolites of all known broad
chemical classes.

## antiSMASH input files

antiSMASH pipeline can work with three different file formats `GenBank`,
`FASTA` and `EMBL`. Both `GenBank` and `EMBL` formats include genome
annotations, while a `FASTA` file just comprises the nucleotides of
each genomic contig.


## Running antiSMASH

The command line usage of antiSMASH is detailed in the following [repositories:](https://docs.antismash.secondarymetabolites.org/command_line/)

In summary, you will need to use your genome as the input. Then,
antiSMASH will create an output folder for each of your genomes.
Within this folder, you will find a single `.gbk` file for each of
the detected Biosynthetic Gene Clusters (we will use these files
for subsequent analyses) and a `.html` file, among others.
By opening the `.html` file, you can explore the antiSMASH annotations.

You can run antiSMASH in two ways **Minimal and Full-featured run**, as follows:  
 
|Run type   | Command                             	|  
|-----------|-----------------------------------------|  
|Minimal run| antismash genome.gbk|  
|Full-featured run |antismash --cb-general --cb-knownclusters --cb-subclusters --asf --pfam2go --smcog-trees genome.gbk|  


> ## Exercise 1: antiSMASH scope
> If you run an _Streptococcus agalactiae_ annotated genome using antiSMASH, what results do you expect?
>
> a. The substances that a cluster produces
>
> b. A prediction of the metabolites that the clusters can produce
>
> c. A prediction of genes that belong to a biosynthetic cluster
>  
> > ## Solution  
> > a. False. antiSMASH is not an algorithm devoted to substance prediction.    
> > b. False. Although antiSMASH does have some information about metabolites
> > produced by similar clusters, this is not its main purpose.  
> > c. True. antiSMASH compares domains and performs a prediction of the genes
> > that belong to biosynthetic gene clusters.   
> {: .solution}
{: .challenge}

## Run your own antiSMASH analysis

First, activate the GenomeMining conda environment:
~~~
$ conda deactivate
$ conda activate /miniconda3/envs/GenomeMining_Global
~~~
{: .language-bash}

Second, run the antiSMASH command shown earlier in this lesson
on the data `.gbk` or `.fasta` files. The command can be executed
either in one single file, all the files contained within a folder or
in a specific list of files. Here we show how it can be performed in
these three different cases:

### Running antiSMASH in a single file
Choose the annotated file ´agalactiae_A909_prokka.gbk´
~~~
$ mkdir -p ~/pan_workshop/results/antismash  
$ cd ~/pan_workshop/results/antismash  
$ antismash --genefinding-tool=none ~/pan_workshop/results/annotated/Streptococcus_agalactiae_A909_prokka/Streptococcus_agalactiae_A909_prokka.gbk    
~~~
{: .language-bash}

To see the antiSMASH generated outcomes do the following:
~~~
$ tree -L 1 ~/pan_workshop/results/antismash/Streptococcus_agalactiae_A909_prokka
~~~
{: .language-bash}

~~~
.
├── CP000114.1.region001.gbk
├── CP000114.1.region002.gbk
├── css
├── images
├── index.html
├── js
├── regions.js
├── Streptococcus_agalactiae_A909.gbk
├── Streptococcus_agalactiae_A909.json
├── Streptococcus_agalactiae_A909.zip
└── svg
~~~
{: .output}


### Running antiSMASH over a list of files
Now, imagine that you want to run antiSMASH over all
_Streptococcus agalactiae_ annotated genomes. Use
a `for` cycle and `*` as a wildcard to run antiSMASH
over all files that start with "S" in the annotated directory.    
 
~~~
$ for gbk_file in ~/pan_workshop/results/annotated/*/S*.gbk
> do
>	antismash --genefinding-tool=none $gbk_file
> done
~~~
{: .language-bash}

## Visualizing antiSMASH results   
In order to see the results after an antiSMASH run, we need to access to
the `index.html` file. Where is this file located?   
~~~
$ cd Streptococcus_agalactiae_A909_prokka
$ pwd
~~~
{: .language-bash}
~~~
~/pan_workshop/results/antismash/Streptococcus_agalactiae_A909_prokka
~~~
{: .output}

As outcomes you should get a folder comprised mainly by the following files:
* **`.gbk` files** For each Biosynthetic Gene cluster region found.
* **`.json` file** To know the input file name, the antiSMASH used
  	version and the regions data (id,sequence_data).
* **`index.html` file** To visualize the outcomes from the analysis.


In order to access these results, we can use scp protocol to
download the directory in your local computer.

If using `scp` , on your local machine, open a GIT bash terminal in the
**destiny** folder and execute the following command:
~~~
$ scp -r user@bioinformatica.matmor.unam.mx:~/pan_workshop/results/antismash/S*A909.prokka/* ~/Downloads/.
~~~
{: .language-bash}

If using R-studio then in the left panel, choose the "more" option,
and "export" your file to your local computer. Decompress the
 Streptococcus_agalactiae_A909.prokka.zip file.  

Another way to download the data to your computer, you first compress the folder and download the compressed file from JupyterHub
~~~
$ cd ~/pan_workshop/results/antismash
$ zip -r Streptococcus_agalactiae_A909_prokka.zip Streptococcus_agalactiae_A909_prokka
$ ls
~~~
{: .language-bash}

In the JupyterHub, navigate to the pan_workshop/results/antismash/ folder, select the file we just created, and press the download button at the top

Once in your local machine, in the directory, Streptococcus_agalactiae_A909.prokka,
open the `index.html` file on your local web browser.

## Understanding the output
The visualization of the results includes many different options.
To understand all the possibilities, we suggest reading the following [tutorial:](https://docs.antismash.secondarymetabolites.org/understanding_output/)

Briefly, in the "Overview" page ´.HTML´ you can find all the regions
found within every analyzed record/contig (antiSMASH inputs), and
summarized all this information in five main features:

* **Region:** The region number.
* **Type:** Type of the product detected by antiSMASH.
* **From,To:** The region's location (nucleotides).
* **Most similar known cluster:** The closest compound from th MIBiG database.
* **Similarity:** Percentage of genes within the closest known
compound that have significant BLAST hit (The last two columns
containing comparisons to the MIBiG database will only be shown
if antiSMASH was run with the KnownClusterBlast option ´--cc-mibig´).

## antiSMASH web services
antiSMASH can also be used in an online server in the
[antiSMASH website:](https://antismash.secondarymetabolites.org/#!/start)
You will be asked to give your email. Then, the results
will be sent to you and you will be allowed to donwload a
folder with the annotations.

#### Exercises and discussion

> ## Exercise 2: NCBI and antiSMASH webserver  
> Run antiSMASH web server over _S. agalactiae_ A909. First, explore the
> NCBI assembly database to obtain the accession. Get the id of your results.   
>
> > ## Solution
> > 1. Go to [NCBI](https://www.ncbi.nlm.nih.gov/) and search _S. agalactiae_ A909.  
> > 2. Choose the assembly database and copy the GenBank sequence ID at the bottom of the site.    
> > 3. Go to [antiSMASH](https://antismash.secondarymetabolites.org/#!/start)    
> > 4. Choose the accession from NCBI and paste `CP000114.1`    
> > 5. Run antiSMASH and paste into the collaborative document your results id
> >  example ` bacteria-cbd13a47-8095-495f-957c-dcf58c261529`  
> > {: .output}
> {: .solution}
{: .challenge}


> ## Exercise 3: (*Optional exercise ) Run antiSMASH over _thermophilus_
> Try Download and annotate _S. thermophilus_ genomes strains LMD-9 (MiBiG), and strain CIRM-BIA 65 reference genome.
> With the following information, generate the script to run antiSMASH only on the _S. thermophilus_ annotated genomes.   
>  
> ~~~
>  done  
>	antismash --genefinding-tool=none $__  
>  for ___ in  ____________  
> do  
> ~~~
> > {: .language-bash}
>
> > ## Solution
> > Guide to solve this problem 
> > First download genomes with genome-download. Remember to activate the conda environment!
> >
> > Download the annotated genomes, and then the genbank files.
> >  0. `ncbi-genome-download --formats genbank --genera "Streptococcus thermophilus"  -S "LMD-9"  -o thermophilus bacteria`
> >   `ncbi-genome-download --formats fasta --genera "Streptococcus thermophilus"  -S "CIRM-BIA 65" -n bacteria`
> > Then, move the genomes to the directory `~/pan_workshop/results/annotated/S*.gbk` 
> >   
> >  2. First, we need to start the for cycle: `for mygenome in ~/pan_workshop/results/annotated/S*.gbk`  
> >  note that we are using the name `mygenome` as the variable name in the for cycle.
> > 
> >   	 
> >  3. Then you need to use the reserved word `do`  to start the cycle.	 
> >    
> >  3. Then you have to call antiSMASH over your variable `mygenome`. Remember the `$` before your variable
> >  to indicate to bash that now you are using the value of the variable.
> > 	 
> >  4. Finally, use the reserved word `done` to finish the cycle.    
> > ~~~
> >  for variable in ~/pan_workshop/results/annotated/t*.gbk  
> >	do  
> > 	antismash --genefinding-tool=none $mygenome  
> >	done   
> > ~~~ {: .language-bash}
> >
> {: .solution}
{: .challenge}


> ## Exercise 4: The size of a region  
> Sort the structure of the next commands that attempt to know the size of a region:
>
>  `SOURCE` `ORGANISM` `LOCUS`  
>  `head` `grep`  
> ~~~
>  $ _____  region.gbk  
>  $ _____ __________ region.gbk  
> ~~~
> {: .language-bash}
>
> Apply your solution to get the size of the first region of _S. agalactiae_ A909
>
> > ## Solution
> >
> > ~~~
> > $ cd ~/pan_workshop/results/antismash  
> > $ head Streptococcus_agalactiae_A909.prokka/CP000114.1.region001.gbk
> > $ grep LOCUS Streptococcus_agalactiae_A909.prokka/CP000114.1.region001.gbk
> > ~~~
> > Locus contains the information of the characteristics of the sequence
> > {: .language-bash}
> > ~~~
> > LOCUS   	CP000114.1         	42196 bp	DNA 	linear   UNK 13-JUN-2022
> > ~~~
> > {: .language-bash}
> > In this case the size of the region is 42196 bp
> > {: .output}
> {: .solution}
{: .challenge}


> ## Discussion 1: BGC classes in _Streptococcus_
>
> What BGC classes does the genus _Streptococcus_ have ?   
>
> > ## Solution
> >
> > In antiSMASH website output of A909 strain we see clusters of two classes, T3PKS and arylpolyene. Nevertheless, this is only one _Streptococcus_ in order to infer all BGC classes in the genus we need to run antiSMASH in more genomes.
> {: .solution}
{: .discussion}

{% include links.md %}



