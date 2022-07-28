
Based on this youtube video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/7p0hrpNaJ14" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Log in

[https://gitlab.bioinformatics.unibe.ch](https://gitlab.bioinformatics.unibe.ch/)

User name and password are the same as for the cluster. The older https://binfgitlab.unibe.ch still exists for historical/compatibility reasons.

## Create a new project in GitLab

This is self-explanatory. In GitLab —&gt; New Project  
Note: It is easier NOT to initialize with a readme otherwise you will run into the issue described in the last step

## Connect to GitLab from binfservms01

We need to set up SSH keys so that binfservms01 and gitlab can talk to each other. You can do this following the instructions [here](https://docs.pages.bioinformatics.unibe.ch/cluster-docs/HPC_tutorial/SSH_tutorial/) to learn how to generate SSH keys. Once you generated your keys, you can follow the instructions [here](https://docs.gitlab.com/ee/user/ssh.html#add-an-ssh-key-to-your-gitlab-account) to copy your private key to gitlab.


To test if it worked: From the cluster, type:

```text
ssh -T git@gitlab.bioinformatics.unibe.ch
```

If everything is fine, you should get a `Welcome to GitLab` message.

## Specify user name and email

On the cluster, check your current git user name and email address:

```text
 git config --list
```

It is probably a good idea for the user name to be the same as for your cluster account (but not sure if this is strictly necessary).

You can change the current values like this:

```text
git config --global user.name <myname>
git config --global user.email <myemail@example.ch>
```

## Set up new repository on the cluster

```text
mkdir <myscripts>
cd <myscripts>
git init
```

Add files to this directory. When you now run `git status`, you see that there are untracked files in the folder. To start tracking the files

```text
git add *
git commit *
```

## Push to GitLab

Go to the project you created in GitLab and copy the path to the repository. This will look similar to this: git@binfgitlab.unibe.ch:/.  
On the cluster:

```text
git remote add origin git@gitlab.bioinformatics.unibe.ch:<username>/<myproject>
```

Now, everything is set up so that you can push to the GitLab repository:

```text
git push -u origin master
```

If you go back to GitLab and refresh the page, you should now see the new files.

If GitLab automatically created a readme file when you first created your project, you may get this error `Updates were rejected because the remote contains work that you do not have locally`. In this case, run

```text
git pull origin master
git push origin master
```
