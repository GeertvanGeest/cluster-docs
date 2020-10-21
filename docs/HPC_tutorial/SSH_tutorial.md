## Public keys best practices

### Verify host public key

**Exercise 1A:** check for existing host key

```
$ ssh-keygen -F binfservms01.unibe.ch -l -f ~/.ssh/known_host
```

**Exercise 1B:** remove existing host key

```
$ ssh-keygen -R binfservms01.unibe.ch -f ~/.ssh/known_host
```

**Exercise 1C:** check host key

```
$ ssh binfservms01.unibe.ch
The authenticity of host 'binfservms01.unibe.ch (130.92.199.95)' can't be established.
ECDSA key fingerprint is SHA256:Yz6JYkqIEHYni+EJgEwQIPqlz0IEUBQLHEQVU8nEwSY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'binfservms01.unibe.ch' (ECDSA) to the list of known hosts.
Last login: Wed Oct 21 16:57:39 2020 from dhcp-100-237.vpn.unibe.ch
[berthier@binfservms01 ~]$ 
```

**Exercise 1D:** check content of .ssh directory on the server:

```
[berthier@binfservms01 ~]$ ls .ssh
ls: cannot access .ssh: No such file or directory
[berthier@binfservms01 ~]$ exit
```



### generate user keypair

**Exercise 2A:** use `ssh-keygen` command to generate a key and encrypt the private key

```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/pierre/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/pierre/.ssh/id_rsa
Your public key has been saved in /home/pierre/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:erkOXJp0loytm7+cbfk7rs9dDzaEtvP6GCmR9Bt7lvg pierre@pierre-laptop
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|                 |
|          .      |
|       + o o .   |
|      o S o = .  |
|     o O . o O . |
|      B o ..O B .|
|       * +oo.@ +.|
|      ooBoo=O=E .|
+----[SHA256]-----+
```

**Exercise 2B:** look at the generated files

```
$ ls -l .ssh
-rw------- 1 pierre pierre 2.6K Okt 21 17:01 id_rsa
-rw-r--r-- 1 pierre pierre  574 Okt 21 17:01 id_rsa.pub
$
$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD38aCe4ICZZ6kPrxlAYruBNhguvHv5YQ2OPjL5Bvs2lzAtV1JPu+QQV9F5SUE3AJc7jh9yn/Agkrg4pMC9EDObWKTl5lg6ritcGtzQMfXYszEevMNRv8ukV6nCt6WGfyjK4l61nXiuXxTv1RvGzJxAefdUGYMvMkkZOdkMKGKTxwE/xmyXJVYUPcEJEqGt4TSD3nC2Wg8GSp1L+MDpI5626UEVVafEzuOIbbHBmQMPhB+0MevP+ZsXzD0Dz1sWWI0wGlnU9W9a1gZ+QNiIeWCvKtNuxXFKB98338W3YQqE+dk/YwwSB1/jeUHIRTEVSyKaIcr42s2Hg9E2TEVZhmZM4vFJb8nozL8Hu3ZKAHqG1JR3FE1mqJ8kOHnWZiGNf3pQwUe3cgN7c5bsZPEl8VJGwuDArQSAFik+nmrNgQlcodIHYnzY6DtbOMnZUpWuVO1zfQQkPGBbGfdDuNT2cvxAkM1RkWtnCT5JdOSn//4njp6aCfg38SopbDn3tfJcJTM= pierre@pierre-laptop
$
$ cat .ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABAFGhqvzt
qUP/ybuCOapCXgAAAAEAAAAAEAAAGXAAAAB3NzaC1yc2EAAAADAQABAAABgQD38aCe4ICZ
Z6kPrxlAYruBNhguvHv5YQ2OPjL5Bvs2lzAtV1JPu+QQV9F5SUE3AJc7jh9yn/Agkrg4pM
...
/mOQjFg57Pn4XdswU+/gX3mbMWbZXJxIdUO1OIlCKolSe2dJA2CfYUv0XpCIWLe36Iiczn
NqTpG7AzfMNH/Ok9Ojr2pqrQnSI=
-----END OPENSSH PRIVATE KEY-----
```




### Copy Public Key to server

**Exercise 3A:** use ssh-copy-id to copy your public key to the server

**Exercise 3B:** login to the server without password

**Exercise 3B:** check content of .ssh on the server


### SSH-Agent

**Exercise 4A:** start ssh-agent

**Exercise 4B:** add a key to ssh-agent

**Exercise 4C:** use ssh-agent



