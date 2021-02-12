## Pipelines
### SLURM-ready pipelines
#### RNA-seq
We use RNA sequencing to test for differential gene expression between experimental groups. The pipeline includes Differential gene expression analysis, Gene ontology enrichment analysis and creation of an interactive Shiny app.

For details please see [Bulk RNA-seq bundle](https://www.bioinformatics.unibe.ch/services/offers/bundles/index_eng.html)

* GitLab repository: [hts_rnaseqpipeline_slurm](https://binfgitlab.unibe.ch/ikeller/hts_rnaseqpipeline_slurm)
* Please see here for an overview of all RNA-seq library prep kits that are used by the NGS platform (17.9.2018): [RNA-seq Protocols](RNA-seq.md#Protocols)
* Contact: Irene Keller

#### scRNA-seq
We offer pre-processing of data from 10x Genomics Chromium gene expression assays. We use cell ranger count to map the reads to the reference genome and count the number of reads overlapping with each gene to produce a count matrix for each sample. This service may be useful for researchers who want to do their own downstream analyses, but want to avoid the preprocessing steps which typically requires access to a high performance computing infrastructure.

For details please see [Single cell RNA-seq bundle](https://www.bioinformatics.unibe.ch/services/offers/bundles/index_eng.html)

* GitLab repository: [https://gitlab.bioinformatics.unibe.ch/lischer/scrnaseq](https://gitlab.bioinformatics.unibe.ch/lischer/scrnaseq)
* Contact: Heidi Lischer / Irene Keller

#### WGS/WES resequencing human hg19
We use whole exome (WES) or whole genome (WGS) sequencing data to identify single nucleotide polymorphisms and small insertions and deletions where the sequenced individuals differ from the human reference genome. Our variant calling pipeline is based on the [GATK best practices guidelines](https://software.broadinstitute.org/gatk/best-practices/workflow?id=11145).
In a next step, the variants are annotated using the Ensembl Variant Effect Predictor [VEP](https://www.ensembl.org/info/docs/tools/vep/index.html) and [Annovar](http://annovar.openbioinformatics.org/en/latest/). For each variant, we will obtain diverse information such as whether it falls within a gene whether it has been observed before etc.

For details please see [Human WGS/WES bundle](https://www.bioinformatics.unibe.ch/services/offers/bundles/index_eng.html)

* GitLab repository: [hts_gatkpipeline_slurm](https://binfgitlab.unibe.ch/ikeller/hts_gatkpipeline_slurm)
* Contact: Irene Keller

#### Somatic variant calling (human only)
We use Illumina sequencing data to identify SNPs, indels, copy number variants (CNVs) and structural variants (SVs) in matched tumor-normal or tumor-only samples from human cancer patients. Our pipeline is based on bcbio-nextgen and includes the steps outlined below.

For details please see [Somatic variant calling bundle](https://www.bioinformatics.unibe.ch/services/offers/bundles/index_eng.html)

* Contact: Irene Keller  

#### Metagenomics: Taconomic and functional profiling
We use whole genome sequencing data from metagenomics samples for taxonomic and functional profiling, for example of gut microbiota. The analysis mostly relies on [tools developed by the Huttenhower Lab](https://bitbucket.org/biobakery/biobakery/wiki/Home) at Harvard.

Note: The pipeline runs a Maaslin analysis testing for associations between taxa / pathway abundances and all provided explanatory variables. If customers want to run additional tests, they can do this in R using the Maaslin tutorial provided at the bottom of the page.

For details please see [Metagenomics bundle](https://www.bioinformatics.unibe.ch/services/offers/bundles/index_eng.html)

* GitLab repository: [hts_metagenomicspipeline_slurm](https://binfgitlab.unibe.ch/ikeller/hts_metagenomicspipeline_slurm)
* Contact: Irene Keller

#### Metagenomics: Mutation analysis
This is a pipeline developed specifically for the Macpherson group but it could be adjusted to other (low complexity) metagenomics data. Currently, it expects as input whole genome metagenomics data from 12 species in the sDMDMm2  flora. Reads will be mapped to a concatenation of all 12 genomes and then split by species. Next, the pipeline runs variant calling using loFreq, identifies structural variants using Pilon and assesses read depths using GATK DepthOfCoverage.

* Contact: Irene Keller

#### Bacterial genome assembly and annotation
Bacterial genome assembly and annotation for Illumina paired-ended reads.

* GitHub repository: [https://github.com/MrTomRod/Bacterial_genome_assembly](https://github.com/MrTomRod/Bacterial_genome_assembly)
* Contact: Thomas Roder / Simone Oberhänsli

### Old pipelines
#### Whole genome bisulfite sequencing
* Contact: Irene Keller
#### Differential isoform expression
* Contact: Irene Keller

## Scripts
* Please add any scripts that might be useful for others to `/data/scripts`
* Please document it well, if possible in the script itself as well as here.
* Write down your name.

**Some background on Irene’s python scripts:**

The general principle is the same for all of these scripts. Each script will ask you to provide specific information to run an analysis. Run `.py -h/—help` to see all required and optional arguments. Some arguments will have default values that are already set. When you execute the python script, it will check if the most important paths and arguments are correct and then write out a shell script. This shell script contains all necessary instructions for slurm as well as the commands to be run. It can be submitted as is using `sbatch <myjob>.sh`

For example, to run fastp on the fastq files from sample Benign1, you would do the following:
```
Fastp_slurm.py -i Benign1_R1.fastq.gz -m Benign1_R2.fastq.gz -o Benign1_clean -N 2 --mem 1G --time 1:0:0 -n pXXX_fastp --version 0.12.5
```

This produces the file fastp_pXXX.sh in the current working directory. To submit the job do
```
sbatch fastp_pXXX.sh
```

### Convert between file formats
* **run_me.sh** - A very useful script that can convert anything into anything.
    * Input: File to be converted and new filename
    * Output: Converted file
    * Example: `./run_me.sh in.pdf out.jpg`

### Index reference fasta
* **indexGenome.py** - Produces the following index files: .dict, .fai, hisat2, bowtie2, bwa
    * Contact: Irene Keller
    * Input: `indexGenome.py -h`
    * Output: various index files


### Read trimming and QC
* **fastp_slurm.py** - Set up shell script to run fastp to remove adaptors and poly-G tails that may be observed in Novaseq data. Currently, quality filtering and length filtering is disabled
    * Contact: Irene Keller
    * Input: `fastp.py -h`
    * Output: fastq files
* **Trimmomatic_slurm.py** - Set up shell script to run trimmomatic on fastq files. Can do adapter trimming and various quality trimming/filtering steps
    * Contact: Irene Keller
    * Input: `Trimmomatic.py -h`
    * Output: fastq files
* **Fastqc_slurm.sh** - Runs fastqc v.0.11.5 and writes output to current working directory
    * Contact: Irene Keller
    * Input: fastq file
    * Output: fastqc report
    * Example: `sbatch fastqc_slurm.sh mysample.fastq.gz`

### Mapping and bam file manipulations
* **BWA_slurm.py** - Python script which will set up a shell script to run bwa mem. For additional info, see top of file.
    * Contact: Irene Keller
    * Input: `BWA_slurm.py -h`
    * Output: Sam file
    * Example:
    ```
    BWA_slurm.py -r /data/references/sDMDMm2_flora/12NEWGenomes -i reads/sample1_clipped_R1.fastq.gz -m reads/sample1_clipped_R2.fastq.gz -N 2 --sample sample1 --library sample1 -n pXXX_bwa -o sample1 -v 0.7.13
    ```
* **RunBowtie2_PE_slurm.py** - Sets up shell script to run bowtie2 for paired-end samples in —end-to-end —sensitive mode.
    * Contact: Irene Keller
    * Input: paired-end fastq files `RunBowtie2_PE_slurm.py -h`
    * Output: sam
* **DepthOfCoverage_slurm.py** - Sets up shell script to run DepthOfCoverage walker from GATK on coordiante sorted bam files
    * Contact: Irene Keller
    * Input: Coordinate sorted bam file. For additional arguments see `DepthOfCoverage_slurm.py -h`
    * Output: Various files with read depth info.
* **MarkDuplicates_slurm.py** - Set up shell script to run Picard tools MarkDuplicates.
    * Contact: Irene Keller
    * Input: sam or bam `MarkDuplicates.py -h`
    * Output: bam
* **MergeSamFiles_slurm.py** - Set up shell script to run Picard tools MergeSamFiles. Typically useful when a sample was sequenced on multiple lanes so that you have multiple fastq files and multiple bam files from the initial mapping step.
    * Contact: Irene Keller
    * Input: multiple sam or bam files `MergeSamFiles_slurm.py -h`
    * Output: bam

### Variant calling, annotation and filtering
* **loFreq_slurm.py** - Sets up shell script to run low frequency variant calling with [loFreq](http://csb5.github.io/lofreq/)
    * Contact: Irene Keller
    * Input: bam file `loFreq_slurm.py -h`
    * Output: vcf

### (Germline) structural variant calling with Delly
following documentation here [https://github.com/dellytools/delly](https://github.com/dellytools/delly)

* **Delly_call_slurm.py** - Set up shell script to run structural variant calling with Delly. Can be used for the initial round of calling, but also for recalling specific positions passed via -v argument
    * Contact: Irene Keller
    * Input: `Delly_call_slurm.py.py -h`
    * Output: bcf file
* **Delly_merge_slurm.py** - Merge SV sites from multiple individuals into a unified site list.
    * Contact: Irene Keller
    * Input: `Delly_merge_slurm.py -h`
    * Output: bcf file
* **MergeBCF_slurm.py** - Merges 2 or more bcf files
    * Contact: Irene Keller
    * Input: `MergeBCF_slurm.py -h`
    * Output: bcf file
* **Delly_filter_slurm.py** - Apply the germline structural variant filter.
    * Contact: Irene Keller
    * Input: `Delly_filter_slurm.py -h`
    * Output: bcf file

### Run selected steps from GATK germline variant calling pipeline
* **HaplotypeCaller_3.7_slurm.py** - Sets up shell script to run GATK HaplotypeCaller version 3.7 or later.
    * Contact: Irene Keller
    * Input: `HaplotypeCaller_3.7_slurm.py -h`
    * Output: vcf or g.vcf (depending on —erc argument)
* **GenotypeGVCFs_slurm.py** - Sets up shell script for joint variant calling across g.vcf files.
    * Contact: Irene Keller
    * Input: one or several g.vcf files from HaplotypeCaller `GenotypeGVCFs_slurm.py -h`
    * Output: joint vcf file
* **HardFilterVcf_slurm.py** - Sets up shell script to run VariantFiltration and SelectVariants walkers from GATK. Filtering thresholds are hard-wired based on recommendations [here](https://gatkforums.broadinstitute.org/gatk/discussion/2806/howto-apply-hard-filters-to-a-call-set)
    * Contact: Irene Keller
    * Input: vcf file `HardFilterVcf_slurm.py -h`
    * Output: vcf

### Variant calling from RNA-seq data
* **RNAseqVariantCalling_slurm.py** - Sets up shell script to run variant calling following this strategy: [https://gatkforums.broadinstitute.org/gatk/discussion/3891/calling-variants-in-rnaseq](https://gatkforums.broadinstitute.org/gatk/discussion/3891/calling-variants-in-rnaseq). Script will run 2-pass mapping with star, add read groups, mark duplicates, run GATK Split’N’Trim, GATK BQSR, and GATK HaplotypeCaller.
    * Contact: Irene Keller
    * Input: fastq files `RNAseqVariantCalling_slurm.py -h`
    * Output: vcf
* **FilterSNPs_RNAseq_slurm.py** - Sets up shell script to filter variants from previous script. Hard filters are based on recommendations [here](https://software.broadinstitute.org/gatk/documentation/article.php?id=3891)
    * Contact: Irene Keller
    * Input: vcf file `FilterSNPs_RNAseq_slurm.py -h`
    * Output: vcf

### annotation
* **snpEff_slurm.py** - Sets up shell script to annotates vcf using snpEff. Output mode is GATK to produce a vcf file that can be fed to GATK VariantAnnotator to simplify the annotations (see next script).
    * Contact: Irene Keller
    * Input: vcf file `snpEff.py -h`
    * Output: annotated vcf
* **VariantAnnotator_slurm.py** - Sets up shell script that will run GATK VariantAnnotator, Takes as input an unannotated vcf AND the same vcf annotated with SnpEff (i.e. the output of the previous script).
    * Contact: Irene Keller
    * Input: vcf file `VariantAnnotator.py -h`
    * Output: vcf

### vcf file manipulations
* **MergeVcf_slurm** - Sets up a shell script for merging multiple vcf files with vcftools.
    * Contact: Irene Keller
    * Input: multiple vcf files `MergeVcf_slurm -h`
    * Output: joint vcf

### snakemake resource usage
* **resourceUsageScript.sh** - parses snakemake slurm logs and get resource usage statistics.
    * Contact: Heidi Tschanz-Lischer
    * Input: path to slurm-log folder and path to output file
    * Output: file with resource usage for each rule, summary of overall resource usage
    * Example:
    ```
    ./resourceUsageScript.sh /PATH/TO/slurm_logs /PATH/TO/resourceUsage.txt
    ```

### File corruption check
* **CorruptionDiagnostic.jar** - parses the list of corrupted files and returns all files within a project or user which are corrupted and non-corrupted.
    * Contact: Heidi Tschanz-Lischer
    * Input: project number (e.g. p501) or user name (e.g. lischer) and output folder. `java -jar CorruptionDiagnostic.jar -h`
    * Output: file with list of corrupted files, file with list of non-corrupted files and EXCEL file with an overview of corrupted and non-corrupted files
    * Example (check a project folder):
    ```
    java -jar CorruptionDiagnostic.jar -p p501 -o /PATH/TO/OUTPUTFOLDER
    ```
    * Example (check a user folder):
    ```
    java -jar CorruptionDiagnostic.jar -u lischer -o /PATH/TO/OUTPUTFOLDER
    ```
* **copyNonCorruptedFiles.sh** - Script to copy back all files listed in a file (keeping directory structure). This script will generate all necessary folders and copy all files from /data.old to the new storage (/data) listed in a file.
    * Contact: Heidi Tschanz-Lischer
    * Input: file with a list of files
    * Example:
    ```
    copyNonCorruptedFiles.sh file_nonCorrupted.txt
    ```
* **Check the status of .gz files:**
```
for file in *gz
do
    if zcat $file > /dev/null 2>&1
    then
        echo $file" ok"
    else
        echo $file" corrupt"
    fi
done
```
    * Contact: David Ferreira
