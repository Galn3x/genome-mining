---
title: "BigScape"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
FIXME

# BiG-SCAPE

## Introduction

In the previous section, we learnt how to study the BGCs encoded by each of the genomes of our analyses. In case you are interested in the study of a certain BGC or a certain strain, this may be enough. However, sometimes the researcher aims to compare the biosynthetic potential of tens or hundreds of genomes. To perform this kind of analysis, we will use BiG-SCAPE (Navarro-Muñoz et al., 2019), a workflow that will compare all the BGCs detected by antiSMASH to find their relatedness. BiG-SCAPE will search for Pfam domains (Mistry et al., 2021) in the protein sequences of each BGC. Then, the Pfam domains will be linearized compared, creating different similarity networks and scoring the similarity of each pair of clusters. Based on this, the diverse BGCs will be classified on Gene Cluster Families (GCFs) to facilitate their study. A single GCF is supposed to encompass BGCs that produce chemically related metabolites (molecular families).

Let's see how this analysis can be done:

## Preparing the input

First, we need to create a folder with all the BGC files annotated by antiSMASH. 

~~~
$ mkdir BGCs_antiSMASH
~~~

In each of the antiSMASH output folders, we will find a single .gbk file for each BGC that includes "region" within its filename. Thus, we will copy all those files to the new folder.

~~~
$ cp */*.region*.gbk BGCs_antiSMASH/
~~~

---
## Executing BiG-SCAPE

BiG-SCAPE can be executed in different ways, depending on the installation mode that you applied. You could call the program through `bigscape`, `run_bigscape`or `run_bigscape.py`. Here, based on our installation (see Setup section) we will use `run_bigscape`. 

A minimal run just requires to include the input and the output folder names. For example:

~~~
$ run_bigscape BGCs_antiSMASH output_BiG-SCAPE
~~~

To know all the possibilities of this program, just write down `run_bigscape -h`

optional arguments:
--------

| Command               | Description |
| :---                  |    :----:   |
|  -h, --help           | show this help message and exit |
|  -l LABEL, --label LABEL | An extra label for this run (will be used as part of the folder name within the network_files results) |
|  -i INPUTDIR, --inputdir INPUTDIR | Input directory of gbk files, if left empty, all gbk files in current and lower directories will be used. |
|  -o OUTPUTDIR, --outputdir OUTPUTDIR  | Output directory, this will contain all output data files. |
|  --pfam_dir PFAM_DIR  | Location of hmmpress-processed Pfam files. Default is same location of BiG-SCAPE |
|  -c CORES, --cores CORES | Set the number of cores the script may use (default: use all available cores) |
|  --include_gbk_str INCLUDE_GBK_STR [INCLUDE_GBK_STR ...] | Only gbk files with this string(s) will be used for the analysis (default: 'cluster', 'region'). Use an asterisk to accept every file (overrides '--exclude_gbk_str') |
|  --exclude_gbk_str EXCLUDE_GBK_STR [EXCLUDE_GBK_STR ...] | If any string in this list occurs in the gbk filename, this file will not be used for the analysis (default: final). |
|  -v, --verbose        | Prints more detailed information. Toggle to activate. |
|  --include_singletons | Include nodes that have no edges to other nodes from the network. Toggle to activate. |
|  -d DOMAIN_OVERLAP_CUTOFF, --domain_overlap_cutoff DOMAIN_OVERLAP_CUTOFF |Specify at which overlap percentage domains are considered to overlap. Domain with the best score is kept (default=0.1). |
|  -m MIN_BGC_SIZE, --min_bgc_size MIN_BGC_SIZE | Provide the minimum size of a BGC to be included in the analysis. Default is 0 base pairs |
|  --mix                | By default, BiG-SCAPE separates the analysis according to the BGC product (PKS Type I, NRPS, RiPPs, etc.) and will create network directories for each class. Toggle to include an analysis mixing all classes |
|  --no_classify        | By default, BiG-SCAPE classifies the output files analysis based on the BGC product. Toggle to deactivate (note that if the --mix parameter is not activated, BiG-SCAPE will not create any network file). |
|  --banned_classes {PKSI,PKSother,NRPS,RiPPs,Saccharides,Terpene,PKS-NRP_Hybrids,Others} [{PKSI,PKSother,NRPS,RiPPs,Saccharides,Terpene,PKS-NRP_Hybrids,Others} ...] | Classes that should NOT be included in the classification. E.g. "--banned_classes PKSI PKSOther" |
|  --cutoffs CUTOFFS [CUTOFFS ...] |Generate networks using multiple raw distance cutoff values. Values should be in the range [0.0, 1.0]. Example: --cutoffs 0.1 0.25 0.5 1.0. Default: c=0.3. |
|  --clans-off          | Toggle to deactivate a second layer of clustering to attempt to group families into clans |
|  --clan_cutoff CLAN_CUTOFF CLAN_CUTOFF | Cutoff Parameters for which clustering families into  clans will be performed in raw distance. First value is the cutoff value family assignments for BGCs used in clan clustering (default: 0.3). Second value is the cutoff value for clustering families into clans (default: 0.7). Average linkage for BGCs in a family is used for distances between families. Valid values are in the range [0.0, 1.0]. Example: --clan_cutoff 0.3 0.7) |
 | --hybrids-off        | Toggle to also add BGCs with hybrid predicted products from the PKS/NRPS Hybrids and Others classes to each subclass (e.g. a 'terpene-nrps' BGC from Others would be added to the Terpene and NRPS classes) |
 | --mode {global,glocal,auto} | Alignment mode for each pair of gene clusters. 'global': the whole list of domains of each BGC are compared; 'glocal': Longest Common Subcluster mode. Redefine the subset of the domains used to calculate distance by trying to find the longest slice of common domain content per gene in both BGCs, then expand each slice. 'auto': use glocal when at least one of the BGCs in each pair has the 'contig_edge' annotation from antiSMASH v4+, otherwise use global mode on that pair. |
 | --anchorfile ANCHORFILE |Provide a custom location for the anchor domains file, default is anchor_domains.txt. |
 | --force_hmmscan      | Force domain prediction using hmmscan even if BiG-SCAPE finds processed domtable files (e.g. to use a new version of PFAM). |
 | --skip_ma            | Skip multiple alignment of domains' sequences. Use if alignments have been generated in a previous run. |
 | --mibig              | Use included BGCs from then MIBiG database. Only relevant (i.e. those with distance < max(cutoffs) against the input set) will be used. Currently uses version 2.1 of MIBiG. See https://mibig.secondarymetabolites.org/ |
 | --mibig14            | Include BGCs from version 1.4 of MIBiG |
 | --mibig13            | Include BGCs from version 1.3 of MIBiG |
 | --query_bgc QUERY_BGC |Instead of making an all-VS-all comparison of all the input BGCs, choose one BGC to compare with the rest of the set (one-VS-all). The query BGC does not have to be within inputdir |
 | --domain_includelist  Only analyze BGCs that include domains with the pfam accessions found in the domain_includelist.txt file |
 | --version           |  Show program's version number and exit |

We will include the `--mix`label to create a similarity network with all the different type of BGCs together. Since none of the our BGCs is on a contig edge, we could use the global mode. However, frequently, when analyzing draft genomes, this is not the case. Thus, the auto mode will be the most appropriate, which will use the global mode to align domains except for those cases in which the BGC is located near to a contig end, for which the glocal mode is automatically selected. Thus, we could also use the auto mode. 

~~~
$ run_bigscape BGCs_antiSMASH output_BiG-SCAPE --mix --mode auto
~~~

Once the process end, you will find in your terminal screen some basic results, such as the number of BGCs included in each type of network. In the output folder you will find all the data. An easy way to prospect your results is by opening the "index.html" file with a browser (Firefox, Chrome, Safari, etc.). There are diverse sections in the visualization. At the left it is represented the number of BGCs per genome and BGC-type (Fig. Xa). At the right side it is represented a clustered heatmap of your genomes based on the presence/absence of GCFs (Fig. Xb). You can customize this heatmap and select the clustering method (Fig. Xb1) or the number of GCFs represented. Also, there are some summary statistics included the at the top right of the overciew screen (Fig. Xc). 

![BiG-SCAPE](https://github.com/AxelRamosGarcia/Genome-Mining/fig/BiG-SCAPE.png)

When you click on any of the options of the upper bar (Fig. Xd) it will appear a similarity network of BGCs (Fig. X). It may take some time to charge the network.

![Network](https://github.com/AxelRamosGarcia/Genome-Mining/fig/BiG-SCAPE_network.png)

Here, it is represented a single network for each GCF or each Gene Cluster Clan (GCC), which may comprise several GCFs. Each dot represent a BGC. Those dots with bold circles are already described BGCs that have been recruited from the MiBIG database (Kautsar et al., 2020) because of its similarity with some BGC of the analysis. When you click on a BGC (dot), it appears its GCF at the right. You can click on the GCF name to see the phylogenetic distances among all the BGCs comprised by a single GCF (Figure X)

![Tree](https://github.com/AxelRamosGarcia/Genome-Mining/fig/BiG-SCAPE_tree.png)

 Here, the BGC that represents the "centre" of the GCF is labelled in red. These trees will be useful to prioritize the search of secondary metabolites, for example, by focusing on the most divergent BGC clade or those that are distant to already described BGCs.

-------------

## MiBIG database

As commented before, both antiSMASH and BiG-SCAPE will compare our BGCs with those already described and included in the MiBIG database. This is a curated repository for BGCs of known function also available through this link: https://mibig.secondarymetabolites.org/ 

-------------

## Cytoscape visualization of the results

You can also customize and re-renderize the similarity networks of your results with Cytoscape (https://cytoscape.org/). To do so, you will need some files included in the ouput directory of BiG-SCAPE. Both are located in the same folder. You can choose any folder included in the Network_files/(date)hybrids_auto/ directory, depdending on your intereset. The "Mix/" folder represents the complete network, including all the BGCs of the analysis. There, you will need a the files "mix_c0.30.network" and "mix_clans_0.30_0.70.tsv". When you upload the ".network" file, it is needed that you select as "source" the first column and as "target" the second one. Then, you can upload the ".tsv" and just select the first column as "source". Finally, you need to click on Tools -> merge -> Networks -> Union to combine both GCFs and singletons. Now you can change colors, labels, etc. according to your specific requirements.

-------------

### References
- Navarro-Muñoz, J.C., Selem-Mojica, N., Mullowney, M.W. et al. "A computational framework to explore large-scale biosynthetic diversity". Nature Chemical Biology (2019).

- Mistry, J., Chuguransky, S., Williams, L., Qureshi, M., Salazar, G. A., Sonnhammer, E. L., ... & Bateman, A. (2021). Pfam: The protein families database in 2021. Nucleic Acids Research, 49(D1), D412-D419.

- Kautsar, S. A., Blin, K., Shaw, S., Navarro-Muñoz, J. C., Terlouw, B. R., van der Hooft, J. J., ... & Medema, M. H. (2020). MIBiG 2.0: a repository for biosynthetic gene clusters of known function. Nucleic acids research, 48(D1), D454-D458.

## Corrida de BiG-SCAPE hecha por Clau el 070622

~~~
pwd
~~~
{: .bash}

~~~
/home/betterlab/GenomeMining/datos/copia-antis-output
~~~
{: .output}


~~~
ls Streptococcus_agalactiae_*/*region*gbk | wc -l
~~~
{: .bash}

~~~
13
~~~
{: .output}

Copy the following script to a file named `change-name.sh` using `nano`:
~~~
# This script is to rename the AntiSMASH GBKs for them to include the species and strain names, taken from the folder name.
# The argument it requires is the name of the folder with the AntiSMASH output, which must NOT contain a slash at the end.

# Usage for one AntiSMASH output folder:
# 	sh change-names.sh <folder>

# Usage for multiple AntiSMASH output folders:
# 	for species in <output folder pattern*>
# 		do
# 			sh change-names.sh $species
# 		done



ls -1 "$1"/*region*gbk | while read line # enlist the gbks of all regions in the folder and start a while loop
 do
	dir=$(echo $line | cut -d'/' -f1) # save the folder name in a variable 
	file=$(echo $line | cut -d'/' -f2) # save the file name in a variable
    for directory in $dir
        do
        	cd $directory # enter the folder
            newfile=$(echo $dir-$file) # make a new variable that fuses the folder name with the file name
 			echo "Renaming" $file " to" $newfile # print a message showing the old and new file names
 			mv $file $newfile # rename
 			cd .. # return to main folder befor it beggins again
 		done
 done
~~~
{: .bash}

~~~
for species in Streptococcus_agalactiae_*
	do
 		sh change-names.sh $species
 	done

~~~
{: .bash}

~~~
ls Streptococcus_agalactiae_*/*region*gbk | wc -l
~~~
{: .bash}

~~~
13
~~~
{: .output}

~~~
mkdir -p bigscape/bgcs_gbks/
~~~
{: .bash}

~~~
scp Streptococcus_agalactiae_*/*region*gbk bigscape/bgcs_gbks/
~~~
{: .bash}

~~~
ls bigscape/bgcs_gbks/
~~~
{: .bash}

~~~
Streptococcus_agalactiae_18RS21-AAJO01000016.1.region001.gbk
Streptococcus_agalactiae_18RS21-AAJO01000043.1.region001.gbk
Streptococcus_agalactiae_18RS21-AAJO01000226.1.region001.gbk
Streptococcus_agalactiae_515-AAJP01000027.1.region001.gbk
Streptococcus_agalactiae_515-AAJP01000037.1.region001.gbk
Streptococcus_agalactiae_A909-CP000114.1.region001.gbk
Streptococcus_agalactiae_A909-CP000114.1.region002.gbk
Streptococcus_agalactiae_CJB111-AAJQ01000010.1.region001.gbk
Streptococcus_agalactiae_CJB111-AAJQ01000025.1.region001.gbk
Streptococcus_agalactiae_COH1-AAJR01000002.1.region001.gbk
Streptococcus_agalactiae_COH1-AAJR01000044.1.region001.gbk
Streptococcus_agalactiae_H36B-AAJS01000020.1.region001.gbk
Streptococcus_agalactiae_H36B-AAJS01000117.1.region001.gbk
~~~
{: .bash}

Run bigscape:

~~~
run_bigscape bigscape/bgcs_gbks/ bigscape/output_070622 --mix --hybrids-off --mode auto
~~~
{: .bash}

~~~
input bgcs_gbks
output output_070622
Running BiG-SCAPE


   - - Processing input files - -
 Including files with one or more of the following strings in their filename: 'cluster', 'region'
 Skipping files with one or more of the following strings in their filename: 'final'

Importing GenBank files

 Starting with 13 files
 Files that had its sequence extracted: 13

Creating output directories

Trying threading on 32 cores

Predicting domains using hmmscan
 Predicting domains for 13 fasta files
 Finished generating domtable files.

Parsing hmmscan domtable files
 Processing 13 domtable files
  No domains where found in Streptococcus_agalactiae_18RS21-AAJO01000226.1.region001.domtable. Removing it from further analysis
 New domain sequences to be added; cleaning domains folder
 Finished generating pfs and pfd files.

Processing domains sequence files
 Adding sequences to corresponding domains file
 Reading the ordered list of domains from the pfs files
 Creating arrower-like figures for each BGC
  Parsing hmm file for domain information
    Done
  Found file with domains colors
  Reading BGC information and writing SVG
 Finished creating figures


   - - Calculating distance matrix - -
Performing multiple alignment of domain sequences

 Using hmmalign
launch_hmmalign took 1.050 seconds
 Trying to read domain alignments (*.algn files)

Generating distance network files with ALL available input files
   Writing the complete Annotations file for the complete set

 Mixing all BGC classes

  Mix (12 BGCs)
  Calculating all pairwise distances
Ignored unknown character X (seen 1 times)
generate_network took 0.055 seconds
  Writing output files
  Calling Gene Cluster Families
  Cutoff: 0.3

 Working for each BGC class
  Sorting the input BGCs


  Others (6 BGCs)
   Writing annotation files
   Calculating all pairwise distances
Ignored unknown character X (seen 1 times)
generate_network took 0.046 seconds
   Writing output files
  Calling Gene Cluster Families
  Cutoff: 0.3

  PKSother (6 BGCs)
   Writing annotation files
   Calculating all pairwise distances
generate_network took 0.102 seconds
   Writing output files
  Calling Gene Cluster Families
  Cutoff: 0.3


	Main function took 17.353 s
~~~
{: .output}


Move your `bigscape/` folder to the parent folder:
~~~
mv bigscape/ ../
~~~
{: .bash}

Go to your local machine and locate in a folder where you want to store your BiG-SCAPE results.

Copy your `output_070622/` folder to your local machine:

~~~
scp -r betterlab@132.248.196.38:/home/betterlab/GenomeMining/datos/bigscape/output_070622/ .
~~~
{: .bash}

Open your `index.html` with a browser:
~~~
firefox output_070622/index.html # If you have Firefox available
~~~
{: .bash}

{% include links.md %}
