## IBU Cloud

The IBU cloud is accessible only for cluster users.

### Set up locally

[nextloud](https://nextcloud.com/) allows you to automatically synchronyse your local files with your IBUcloud folder (which is a nextcloud) and therefore easily backup and share your files. Setting up nextcloud on your local computer is quite easy:

Follow the instructions [here](https://nextcloud.com/install/):
1.  As server address you have to enter: [https://cloud.bioinformatics.unibe.ch/](https://cloud.bioinformatics.unibe.ch/)
2.  Then you need to login with the IBUcluster login
3.  Afterwards you need to define what you would like to automatically synchronise from the IBUcloud.
4.  Move all project related local folders to the newly created nextcloud shared folder.

!!! note "If you are out of cloud storage"
    Ask Pierre Berthier to extend your IBU cloud quota

### Upload files 

In a browser, go to [https://cloud.bioinformatics.unibe.ch](https://cloud.bioinformatics.unibe.ch/), and log in with your cluster account. Click on (+) to upload files from your local computer or directly from the cluster (see [Mount Cluster as Network Drive](https://projects.bioinformatics.unibe.ch/projects/ibu-best-practices/wiki/mount-cluster-as-network-drive-sshfs) for more info). Click on share icon and enter email address of recipient.

### Mount cluster folders

You can mount folder from the IBU cluster onto the cloud. These will be read-only at the cloud.

!!! note "This applies only to certain cluster users"

To mount folders:

```sh
sudo /usr/local/sbin/newcloudmount.sh \
filename_on_cloud \
/data/projects/p123/folder_to_mount \
your_username
``` 

Once the folder is visible ([https://cloud.bioinformatics.unibe.ch](https://cloud.bioinformatics.unibe.ch/)), click on share icon and enter the email address of the recipient. Select &quot;email&quot; (instead of &quot;remote&quot;). Now the recipient has received a download link. Files shared in this manner are read-only.

Check mounts:
```sh 
sudo /usr/local/sbin/lscloudmounts.sh
```

Delete mounts:

```sh
sudo /usr/local/sbin/delcloudmount.sh <ID>
```
by using the ID listed in the lscloudmounts.sh output

### Download from command line

If you want to download a file from the command line using an IBU Cloud 'Share Link', you need to append `/download` to the URL. For example:

```sh
wget --content-disposition https://cloud.bioinformatics.unibe.ch/index.php/s/CJdppn6Nn37Rjq2/download
```

Or, with `curl`:

```sh
curl -J -O https://cloud.bioinformatics.unibe.ch/index.php/s/CJdppn6Nn37Rjq2/download
```

## FTP

**Web interface**: [https://ftp01.binf.unibe.ch](https://ftp01.binf.unibe.ch/)

**login**:    username@bioinformatics.unibe.ch  

**password**: your cluster password  

You can upload files there or create folders and share them via URL with anyone.

## Campus Cloud

The is a university-wide cloud instance. 

### Command line upload

The bash script `share_file_through_campus_cloud.sh` on `binfservms01` allows you to upload a file or folder directly from the cluster to _your_ [campus cloud](https://campuscloud.unibe.ch/) and share it with external parties, even outside the university.

You can the script and more documentation in a dedicated [github repository](https://github.com/MrTomRod/Campus-Cloud-UniBe-Uploader/).

Simply upload a file/folder without sharing it:

```sh
/mnt/apps/centos7/bin/share_file_through_campus_cloud.sh -l <location>
```

Upload a file and create a public link to it: (**Only** works for files, link will be accessible by anyone.)

```sh
/mnt/apps/centos7/bin/share_file_through_campus_cloud.sh \
-l <location> \
--public-link \
-d <days>
```

Upload and then share the file/folder:

```sh
/mnt/apps/centos7/bin/share_file_through_campus_cloud.sh \
-l <location> \
-e <email(s)> \
-d <days>
```

!!! tip "Add the script to your `.bashrc`"
    If you want to use the script without having to remember the path to it, add this line at the end of your `~/.bashrc` file:

    ```sh
    alias share_file_through_campus_cloud.sh='/mnt/apps/centos7/bin/share_file_through_campus_cloud.sh'
    ```

Explanation of the parameters

<figure class="table"><table><tbody><tr><td>-l, —location</td><td>location of file/folder to be shared.</td></tr><tr><td>-e, —email</td><td>email of recipient(s): If multiple, they must be separated by comma; e.g., ‘-e a@x.com,b@x.com’ (Optional.)</td></tr><tr><td>-p, —public-link</td><td>return a public link. Takes no arguments. Only works for files. (Optional.)</td></tr><tr><td>-d, —days</td><td>days until access expires. 0 means share doesn’t expire. (Optional. Default = 10 days.)</td></tr><tr><td>-h, —help</td><td>display this help and exit</td></tr></tbody></table></figure>

Next, you will be prompted to enter your campus username and password, then for a name of the new folder on your Campus Cloud. Then, the upload will begin.


### Command line download

The opposite direction is also possible and it works as follows:

1.  Open [CampusCloud](https://campuscloud.unibe.ch/), locate the file you want to download.
2.  Create a freely accessible link to the file.
3.  Download using wget: `wget <link-to-file>`




!!! note "Notes"
    *   To save space on the Cluster, consider compression. (E.g.: `zip foo.zip file1.txt file2.txt` or `zip -r foo.zip /folder`)
    *   The files will be uploaded to your personal campus account.
    *   Access to the file wills end for the external user after 10 days or a specified number of days (`-d` parameter), but the file will then not be deleted on your Campus Cloud.
    *   If you want to share the same file later-on with more people, you don’t have to upload it again: Simply go to [campuscloud.unibe.ch](https://campuscloud.unibe.ch/) and change the sharing parameters of the file.
    *   If you want to revoke somebody’s right to see a file, go to [campuscloud.unibe.ch](https://campuscloud.unibe.ch/) and edit the relevant sharing permission.
    *   External users will have to set up a guest account the first time a file/folder is shared with their particular email address. They will have to know the login info for the next time something is shared with them.

### Can I use it on my own machine?

Yes, if your machine supports bash and `jq` (a JSON parser) is installed. Get the script from the [github page](https://github.com/MrTomRod/Campus-Cloud-UniBe-Uploader/).
