

## Project quota

By default, a project will have a quota of 300 GB. You can change this as follows:

```sh
sudo /usr/local/sbin/chproject.sh -Q 1024 p380
```

will change the quota of project p380 to 1TB. The quota (after -Q) must be given in GB. Hard quota will be set to this value, soft quota to 95% of this value.

## Add or remove users

```sh
chproject.sh [-Q ] [--adduser usr] [--deluser usr]
```

Modifies the existing project # prjnb and

*   sets a new quota (option -Q) # see previous section
*   adds a user to the project (option --adduser). Note that you may need to either reopen the shell or run `newgrp pXXX` for the permissions to be updated.
*   removes a user from the project (option--deluser)

## Display project information

To show information about a project, e.g. creation date, quotas:

```sh
sudo /usr/local/sbin/lsproject.sh p380
```

to see free space within a project:

```sh
df -h /data/projects/p999_XXXX
```

To see which users belong to a certain project:

```sh
sudo /usr/local/sbin/lsproject.sh -u p387
```

or for all projects:

```sh
sudo /usr/local/sbin/lsproject.sh -a
```
