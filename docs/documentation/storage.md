
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

Where `S-Quota` indicates soft quota, and `H-Quota` indicates hard quota. If you exceed the soft quota, (indicated by \*), you have 7 days to clean up. Otherwise, you will not be able to write to these folders.

## Manual backup solution

Everything will be backupped that is in a folder called **/data/projects/pXXX\*/BACKUP**. This folder must contain files or hard links to files (no soft links).

*   You can use the `/data/scripts/hardLinkScripts.sh` script to automatically parse your projects/user folder and hard link files from specific folders and/or files with specific extensions to the BACKUP folder. Make a copy of the script and edit the first couple of lines to indicate the projects, folders or files with specific extensions that you want to include in your backup. You will have to remember to add new projects to the script as you open them.
*   To execute the script once per day: open crontab: `crontab -e` and add the following line `55 23 * * * <pathToScript>`. The script will now be executed every day at 23:55
*   To execute the script every time you login to the cluster: If you execute the script within your .bashrc (within your home directory), the script is automatically executed everytime you login to the cluster  

## Data storage options

**The IBU cluster cannot be used for long term archiving of data**.

To archive a project, we recommend you delete all files that are no longer needed, then create a tar ball using the command below. Copy the tar ball to locations of your choice and delete the project from the cluster.

```text
tar -zcvf tar-archive-name.tar.gz source-folder-name
```

Some useful resources on data management and storage:

*  [for DBMR members](http://intern.unibe.ch/oe_intranets/medizinische_fakultaet/department_for_biomedical_research/dienstleistungen/zentrale_dienste/dbmr_it/storagebestellungen/dbmr_storage_options/index_ger.html)  
* UniBe info on [research data management](https://www.unibe.ch/university/services/university_library/services/open_science/research_data_management/index_eng.html#pane587950)
* European Nucleotide Archive: We have a tutorial on how to upload data [here](https://www.bioinformatics.unibe.ch/services/faq/index_eng.html).
