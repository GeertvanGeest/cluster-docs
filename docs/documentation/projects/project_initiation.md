!!! bug "This probably only for IBU core members"

## Project initiation

### Entirely through OpenProject (this is the preferred option)

The collaborator enters the project into OpenProject with a short description here: [newproject.bioinformatics.unibe.ch](https://newproject.bioinformatics.unibe.ch/)

One of the OpenProjects administrators (Rémy, Irene, Simone, Heidi, Marco or Geert) will process all incoming projects and set up a matching folder on the cluster. This is how they are supposed to do it:

1.  Add **members** to the project. Add at least one user with a cluster-login for folder creation to succeed. All users who are members with a cluster account will get access to the folder. Also add the collaborator to the project, an account was automatically.
2.  Change the **project title** so that it starts with the next available project number (pXXX, look it up at [newproject.bioinformatics.unibe.ch/next\_project\_number](https://newproject.bioinformatics.unibe.ch/next_project_number/)) and is informative
3.  Think about **how much storage** is required by your project. (Suggestions: small: **200 GB**, medium: **700 GB,** large: **3 TB**)
4.  **Change the project name** accordingly from `... [Preliminary]` to `... [? GB]` or `... [? TB]`. (If no folder is required, simply remove `[Preliminary]` and nothing will happen.)
5.  To **trigger the creation of a folder**, open this URL: https://newproject.bioinformatics.unibe.ch/create\_project\_folder/?p=0 (Replace 0 with the actual project number!)

**Note:** If you try to enter the new folder and get “permission denied”, try closing the shell and logging in again.  
**Note:** `TB` / `GB` must be **CAPITALIZED**.

### Partially through OpenProject, partially on the cluster

Create the project folder manually in `/data/projects` **before** creating the project in OpenProject.

On the file server, run the following command:  
`sudo /usr/local/sbin/newproject.sh [userslist]`

For example:  
`sudo /usr/local/sbin/newproject.sh 999 test_foo ikeller,berthier`

This will do the following:

1.  creates a folder `/data/projects/p999_test_foo`
2.  creates a unix group `p999` with users `ikeller` and `berthier`
3.  assigns RW permission to the folder for the new Unix group `p999`

Create the corresponding project on OpenProject, either [here directly](https://projects.bioinformatics.unibe.ch/projects/new) or on [newproject.bioinformatics.unibe.ch](https://newproject.bioinformatics.unibe.ch/)

In OpenProject, one of the admins performs the following steps


<br>
;aldkjf;a 
kdiejf
k;jkjdafa


fddfda

d
fa
dfa


dafadfa
#a 


lsienfiek
<br>



1.  Set the project title to exactly the same name as on the cluster. In our example: p999\_test\_foo
2.  Add members to the project
3.  Set the project type from from `... [Preliminary]` to `... [? GB]` or `... [? TB]`.

In this case, Pierre’s script will see that the project already exists on the cluster and will not create a new folder.

## Change storage quota after folder creation?

This is possible: Please refer to \[\[quota-and-rights-on-file-server\]\].

## Recreate a project that existed before the Great Cluster Crash (GCC)

It will pick the &quot;comment&quot; part of the project directory name and the users list, create a new , empty fileset on /data and setup the correct permissions.

```text
sudo /usr/local/sbin/recreateproject.sh <prjnb>
```
