
## User storage

Each user has storage capacity at `/home/<user>/` and at `/data/users/<user>`. Storage at `/home/` is limited to 20 GB per user, while `/data/users/` has a limit of 4 TB per user. If you don't work with projects (at `/data/projects`), it is therefore recommended to use `/data/users` for storing large files, while using `/home/` only if necessary.

You can get your diskquota with:

```sh
lsquota
```

This will give you:

```
Disk quotas for user berthier (uid 622):  
Filesystem  Used  S-Quota  H-Quota  Grace  #Files  Quota   Limit  Grace  
/home         4G      20G      21G   none      9k     0k      0k   none
/data/users   102G*   100G     105G     7d      3k     0k      0k   none
```

Where `S-Quota` indicaties soft quota, and `H-Quota` indicates hard quota. If you exceed the soft quota, (indicated by \*), you have 7 days to clean up. Otherwise, you will not be able to write to these folders.

## Temporary backup solution

!!! bug "This is possibly (partly) deprecated"

Currently, everything will be backupped that is in a folder called **/data/projects/pXXX\*/BACKUP**. This folder must contain files or hard links to files (no soft links).

*   You can use the `/data/scripts/hardLinkScripts.sh` script to automatically parse your projects/user folder and hard link files from specific folders and/or files with specific extensions to the BACKUP folder.
*   execute script once per day: open crontab: `crontab -e` and add following line `55 23 * * * pathToScript` and the script will be executed every day at 23:55
*   execute script every time you login to the cluster: If you execute the script within your .bashrc (within your home directory), the script is automatically executed everytime you login to the cluster

## Storage options DBMR

!!! bug "This is possibly (partly) deprecated"

[http://intern.unibe.ch/oe\_intranets/medizinische\_fakultaet/department\_for\_biomedical\_research/dienstleistungen/zentrale\_dienste/dbmr\_it/storagebestellungen/dbmr\_storage\_options/index\_ger.html](http://intern.unibe.ch/oe_intranets/medizinische_fakultaet/department_for_biomedical_research/dienstleistungen/zentrale_dienste/dbmr_it/storagebestellungen/dbmr_storage_options/index_ger.html)

## Strategy for cleaning up storage

!!! bug "This is possibly (partly) deprecated"

Eventually, all data will have to be moved from the old to the new storage. For now, we will proceed as follows:

1.  Gradually copy all **active projects** from old to new storage using

```text
rsync -avA <source> <destination>
```

1.  For **old/inactive projects**, delete files that are no longer needed, then create a tar ball using

```text
tar -zcvf tar-archive-name.tar.gz source-folder-name
```

1.  Move the tar ball to a yet to be specified directory on the old storage

### Which results should be kept within a project and when/how projects should be archived?

!!! bug "This is possibly (partly) deprecated"

Some suggestions as a basis for discussion

### Active projects

!!! bug "This probably needs to be moved to a different location"

For the Snakemake pipelines we can very easily define which outputs should be kept and which should be automatically deleted as soon as they are not needed anymore.

I suggest to keep the following:

**RNA-seq pipeline**

1.  Snakefile, rules, config files and experimental design file
2.  All results in DESeq folder. **Table of counts** can be recreated from DESeq object.
3.  **Quality report** with all underlying tables and figure (internal and customer report)
4.  Shiny app
5.  all log files

**GATK pipeline**  
Currently the following files are kept and write protected:

1.  **Bam file** with duplicates marked and sorted by coordinates. This still contains the original base qualities and would allow us to recreate the fastq as long as unmapped reads are included
2.  **g.vcf.gz** for each sample. These are kept to have the option to add samples to a project without having to rerun HaplotypeCaller
3.  **Joint vcf** with all samples from a given project before genotype refinement
4.  **Mapping statistics** for each sample (from samtools flagstat)  
    5a) **WGS: Depth of coverage summary tables** (without per-base files which can be very large)  
    5b) **WES: Diagnose targets output files**

Additionally, the following files are kept but not write-protected:

\- vcf after genotype refinement

\- text file with annovar annotations

\- text file with VEP annotations

\- text file with merged annotations from both tools  
\- text file with filtered variants
