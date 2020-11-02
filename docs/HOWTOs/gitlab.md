# How to work with GitLab

Based on https://www.youtube.com/watch?v=7p0hrpNaJ14

### 1) Log in

[https://gitlab.bioinformatics.unibe.ch](https://gitlab.bioinformatics.unibe.ch/)

User name and password are the same as for the cluster. Note than older https://binfgitlab.unibe.ch still exists for historical/compatibility reasons.

### 2) Create a new project in GitLab

This is self-explanatory. In GitLab —&gt; New Project  
Note: It is easier NOT to initialize with a readme otherwise you will run into the issue described in the last step

### 3) Connect to GitLab from binfservms01

We need to set up SSH keys so that binfservms01 and binfgitlab can talk to each other. You can do this following the instructions here: https://binfgitlab.unibe.ch/help/ssh/README#generating-a-new-ssh-key-pair

To test if it worked: From the cluster, type:

```text
ssh -T git@binfgitlab.unibe.ch
```

If everything is fine, you should get a `Welcome to GitLab` message.

### 4) Specify user name and email

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

### 5) Set up new repository on the cluster

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

### 6) Push to GitLab

Go to the project you created in GitLab and copy the path to the repository. This will look similar to this: git@binfgitlab.unibe.ch:/.  
On the cluster:

```text
git remote add origin git@binfgitlab.unibe.ch:<username>/<myproject>
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

### A note on Auto DevOps

You will probably get an email that your pipeline has failed. This is not a problem. It just means that you cannot use Auto DevOps which would allow you to run automated testing and deploying of your software. By default, Auto DevOps is enabled when a new project is created but it will then be automatically disabled on the first pipeline failure (https://docs.gitlab.com/ee/topics/autodevops/)
