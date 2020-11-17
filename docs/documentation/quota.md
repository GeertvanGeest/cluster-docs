
## Information about your &quot;personal&quot; quota on /home and /data/users:

```sh
lsquota
```

&quot;For example, in this output you can see that I have exceeded my soft-quota in Gigabytes on /data/users (look at the \*) and I have got 7days to clean up. If I do not clean up within these 7 days, I will no longer be able to write new data to these folders.&quot;

```
Disk quotas for user berthier (uid 622):  
Filesystem  Used  S-Quota  H-Quota  Grace  #Files  Quota   Limit  Grace  
/home         4G      20G      21G   none      9k     0k      0k   none
/data/users   8G*      4G      64G     7d      3k     0k      0k   none
```



## Display available partitions

!!! bug "This probably needs to be moved to a different location"

```sh
/usr/local/bin/lsaccounts.sh
```

```
Account  Partition
-------------------- ----------
simone                 phighmem
simone                     pall
simone                   pshort
```

## Check who can use a particular queue:

```sh
sacctmgr show assoc where partition=phighmem
```
