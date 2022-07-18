The IBU has different partitions to which you can submit, the default is `pall`. Each with their own limits and assigned nodes. To see to which partitions you can submit run:

```sh
lsaccounts.sh
```

For an overview of all partitions, their time limits, assigned nodes and max memory:

```sh
sinfo --format="%.P %.10l %.6D %.N %.m" 
```

Returning:

```sh
PARTITION  TIMELIMIT    NODES NODELIST                MEMORY
pall*      28-00:00:0   30    binfservas[07-17,19-37] 250000+
ptest      7-00:00:00   1     binfservas99            1800
pshort     3:00:00      3     binfservas[03,06,18]    112385+
phighmem   110-00:00:   1     binfservas03            2000000
pcmpg      infinite     7     cmpgnode[01-07]         32768+
```
