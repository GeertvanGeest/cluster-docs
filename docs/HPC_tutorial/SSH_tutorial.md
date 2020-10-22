## Public keys best practices


### 0. Prerequisites

- be on Unibe or UniFR Network  (maybe start VPN)


### 1. Verify host public key

**Exercise 1A:** check for existing host key

Execute on your client computer:

```
$ ssh-keygen -F binfservms01.unibe.ch -l -f ~/.ssh/known_hosts
```

??? done "Result"
    ```
    # Host binfservms01.unibe.ch found: line 1 
    binfservms01.unibe.ch ECDSA SHA256:Yz6JYkqIEHYni+EJgEwQIPqlz0IEUBQLHEQVU8nEwSY
    $ 
    ```

??? hint "Possible error message in case you never used ssh before (can safely be ignored)"
    ```
    do_known_hosts: hostkeys_foreach failed: No such file or directory
    ```
    
**Exercise 1B:** remove existing host key

```
$ ssh-keygen -R binfservms01.unibe.ch -f ~/.ssh/known_hosts
```

??? done "Result"
    ```
    # Host binfservms01.unibe.ch found: line 1
    /home/user/.ssh/known_hosts updated.
    Original contents retained as /home/user/.ssh/known_hosts.old
    $
    ```

??? hint "Possible error message in case you never used ssh before (can safely be ignored)"
    ```
    mkstemp: No such file or directory
    ```


**Exercise 1C:** check host key

!!! warning "Warning"
    Replace *<username\>* with your username on the server.
    


```
$ ssh <username>@binfservms01.unibe.ch
```

??? done "Result"
    ```
    The authenticity of host 'binfservms01.unibe.ch (130.92.199.95)' can't be established.
    ECDSA key fingerprint is SHA256:Yz6JYkqIEHYni+EJgEwQIPqlz0IEUBQLHEQVU8nEwSY.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? 
    ```


??? alert "Note"
    To check that the fingerprint is correct, you should have got it from another source of information (eg email, website).
    
Verify that the fingerprint is correct, then answer yes:

??? done "Result"
    ```
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'binfservms01.unibe.ch' (ECDSA) to the list of known hosts.
     ___ ____  _   _   _     _                         _           _            
    |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __ 
     | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
     | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
    |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   
    
    <username>@binfservms01.unibe.ch's password: 
    ```
    
Enter your password:
    
??? done "Result"
    ```
    <username>@binfservms01.unibe.ch's password: ******* *ENTER*
    Last login: Wed Oct 21 16:57:39 2020 from dhcp-100-237.vpn.unibe.ch
    [<username>@binfservms01 ~]$ 
    ```
    

**Exercise 1D:** check content of .ssh directory on the server:

```
[<username>@binfservms01 ~]$ ls .ssh
```

??? done "Result"
    ```
    ls: cannot access .ssh: No such file or directory
    [<username>@binfservms01 ~]$ 
    ```

!!! warning "Warning"
    Logout before moving on to the next exercise.

```
[<username>@binfservms01 ~]$ exit
```


### 2. Generate user keys pair

**Exercise 2A:** use `ssh-keygen` command to generate a key and encrypt the private key with a passphrase

```
$ ssh-keygen
```

??? done "Result"
    ```
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/<user>/.ssh/id_rsa): 
    ```

Hit *ENTER*:
    
??? done "Result"
    ```
    Enter passphrase (empty for no passphrase): 
    ```

!!! warning "Warning"
    Do not enter an empty passphrase, otherwise your private key will be unprotected!

??? done "Result"
    ```
    Enter passphrase (empty for no passphrase): *********** *ENTER*
    Enter same passphrase again: *********** *ENTER*
    Your identification has been saved in /home/<user>/.ssh/id_rsa
    Your public key has been saved in /home/<user>/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:erkOXJp0loytm7+cbfk7rs9dDzaEtvP6GCmR9Bt7lvg user@laptop
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
    $  
    ```

    


**Exercise 2B:** look at the generated files

```
$ ls -l .ssh
```

??? done "Result"
    ```
    -rw------- 1 <user> <user> 2.6K Okt 21 17:01 id_rsa
    -rw-r--r-- 1 <user> <user>  574 Okt 21 17:01 id_rsa.pub
    -rw-r--r-- 1 <user> <user>  444 Okt 22 13:56 known_hosts
    $ 
    ```

```
$ cat .ssh/id_rsa.pub
```

??? done "Result"
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD38aCe4ICZZ6kPrxlAYruBNhguvHv5YQ2OPj
    L5Bvs2lzAtV1JPu+QQV9F5SUE3AJc7jh9yn/Agkrg4pMC9EDObWKTl5lg6ritcGtzQMfXYszEe
    vMNRv8ukV6nCt6WGfyjK4l61nXiuXxTv1RvGzJxAefdUGYMvMkkZOdkMKGKTxwE/xmyXJVYUPc
    EJEqGt4TSD3nC2Wg8GSp1L+MDpI5626UEVVafEzuOIbbHBmQMPhB+0MevP+ZsXzD0Dz1sWWI0w
    GlnU9W9a1gZ+QNiIeWCvKtNuxXFKB98338W3YQqE+dk/YwwSB1jeUHIRTEVSyKaIcr42s2Hg9E
    2TEVZhmZM4vFJb8nozL8Hu3ZKAHqG1JR3FE1mqJ8kOHnWZiGNf3pQwUe3cgN7c5bsZPEl8VJGw
    uDArQSAFik+nmrNgQlcodIHYnzY6DtbOMnZUpWuVO1zfQQkPGBbGfdDuNT2cvxAkM1RkWtnCT5
    JdOSn//4njp6aCfg38SopbDn3tfJcJTM=  <user>@laptop
    $
    ```

```
$ cat .ssh/id_rsa
```

??? done "Result"
    ```
    -----BEGIN OPENSSH PRIVATE KEY-----
    b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABAFGhqvzt
    qUP/ybuCOapCXgAAAAEAAAAAEAAAGXAAAAB3NzaC1yc2EAAAADAQABAAABgQD38aCe4ICZ
    Z6kPrxlAYruBNhguvHv5YQ2OPjL5Bvs2lzAtV1JPu+QQV9F5SUE3AJc7jh9yn/Agkrg4pM
    ...
    /mOQjFg57Pn4XdswU+/gX3mbMWbZXJxIdUO1OIlCKolSe2dJA2CfYUv0XpCIWLe36Iiczn
    NqTpG7AzfMNH/Ok9Ojr2pqrQnSI=
    -----END OPENSSH PRIVATE KEY-----
    $ 
    ```




### 3. Copy Public Key to server

**Exercise 3A:** use ssh-copy-id to copy your public key to the server

```
$ ssh-copy-id <username>@binfservms01.unibe.ch
```

??? done "Result"
    ```
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/<user>/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
     ___ ____  _   _   _     _                         _           _            
    |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __ 
     | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
     | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
    |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   
    
    <username>@binfservms01.unibe.ch's password: *********** *ENTER*

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh '<username>@binfservms01.unibe.ch'"
    and check to make sure that only the key(s) you wanted were added.
    ```
    
**Exercise 3B:** login to the server without password

```
$ ssh <username>@binfservms01.unibe.ch
```

??? done "Result"
    ```
      ___ ____  _   _   _     _                         _           _            
    |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __ 
     | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
     | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
    |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   
    
    Enter passphrase for key '/home/<username>/.ssh/id_rsa': *********** *ENTER*
    Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
    [<username>@binfservms01 ~]$ 
    ```
    

**Exercise 3B:** check content of .ssh on the server

```
[<username>@binfservms01 ~]$ ls -l .ssh
```

??? done "Result"
    ```
    -rw-------. 1 <username> <username> 988 Oct 21 18:48 authorized_keys
    [<username>@binfservms01 ~]$ 
    ```
    
```
[<username>@binfservms01 ~]$ cat .ssh/authorized_keys
```

??? done "Result"
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD38aCe4ICZZ6kPrxlAYruBNhguvHv5YQ2OPj
    L5Bvs2lzAtV1JPu+QQV9F5SUE3AJc7jh9yn/Agkrg4pMC9EDObWKTl5lg6ritcGtzQMfXYszEe
    vMNRv8ukV6nCt6WGfyjK4l61nXiuXxTv1RvGzJxAefdUGYMvMkkZOdkMKGKTxwE/xmyXJVYUPc
    EJEqGt4TSD3nC2Wg8GSp1L+MDpI5626UEVVafEzuOIbbHBmQMPhB+0MevP+ZsXzD0Dz1sWWI0w
    GlnU9W9a1gZ+QNiIeWCvKtNuxXFKB98338W3YQqE+dk/YwwSB1jeUHIRTEVSyKaIcr42s2Hg9E
    2TEVZhmZM4vFJb8nozL8Hu3ZKAHqG1JR3FE1mqJ8kOHnWZiGNf3pQwUe3cgN7c5bsZPEl8VJGw
    uDArQSAFik+nmrNgQlcodIHYnzY6DtbOMnZUpWuVO1zfQQkPGBbGfdDuNT2cvxAkM1RkWtnCT5
    JdOSn//4njp6aCfg38SopbDn3tfJcJTM= <user>@laptop
    [<username>@binfservms01 ~]$ 
    ```
    

### 4. SSH-Agent

**Exercise 4A:** start ssh-agent

```
$ eval $(ssh-agent)
```

??? done "Result"
    ```
    Agent pid 13868
    $
    ```
    
**Exercise 4B:** add a key to ssh-agent

```
$ ssh-add .ssh/id_rsa
```

??? done "Result"
    ```
    Enter passphrase for .ssh/id_rsa: ************* *ENTER*
    Identity added: .ssh/id_rsa (<username>@laptop)
    $ 
    ```
    

**Exercise 4C:** use ssh-agent


```
$ ssh <username>@binfservms01.unibe.ch
```

??? done "Result"
    ```
    Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
    [<username>@binfservms01 ~]$ 
    ```
    


