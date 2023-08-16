
With port forwarding you can access a port on a remote machine as if it were on your local machine. This is useful for accessing web interfaces running on a remote machine.

Start an interactive session with SLURM

```sh
srun --pty --mem=4G --time=4:0:0 /bin/bash
```

As an example, we will use an rstudio container. First, we need to pull the container:

```sh
apptainer pull docker://rocker/rstudio:4.2
```

Now, we run the container with apptainer based on the [rocker documentation](https://rocker-project.org/use/singularity.html):

```sh
mkdir -p run var-lib-rstudio-server

printf 'provider=sqlite\ndirectory=/var/lib/rstudio-server\n' > database.conf

PASSWORD=mypass apptainer exec \
   --bind run:/run,var-lib-rstudio-server:/var/lib/rstudio-server,database.conf:/etc/rstudio/database.conf \
   rstudio_4.2.sif \
   /usr/lib/rstudio-server/bin/rserver --www-address=0.0.0.0 --auth-none=0 \
   --auth-pam-helper-path=pam-helper --server-user=$(whoami)
```

This will create an rstudio instance on port 8787 of the compute node. We can't approach this port directly, but we can create an ssh tunnel to it. 

So, let's create an ssh tunnel to the compute node on your local computer:

```sh
ssh -N -f -L 8787:binfservasXX:8787 gvangeest@binfservms01.unibe.ch
```

!!! note
    Replace `binfservasXX` with the name of the compute node you are on. You can find this with `hostname`. Of course, also replace `gvangeest` with your username.

Now, you can open a browser on your local computer and go to `localhost:8787`. You should see the rstudio login screen. Login with your cluster username and the password specified at `PASSWORD` in the `apptainer exec` command above.

I did not test the jupyter stack yet, but it definitely works with jupyter installed with conda, e.g. like this:

```sh
srun --pty --mem=4G --time=4:0:0 /bin/bash
conda create -n jupyterlab jupyterlab
conda activate jupyterlab
jupyter lab --no-browser --ip=0.0.0.0 --port=8879
```

Locally, you can now create an ssh tunnel to the compute node on your local computer:

```sh
ssh -N -f -L 8879:binfservasXX:8879 gvangeest@binfservms01.unibe.ch
```

You should be able to access jupyterlab on `localhost:8879`. To login enter the token that is printed in the terminal.
