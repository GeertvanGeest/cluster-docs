## Public keys best practices

### Material

[:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/hpc_intro.pdf){: .md-button }

### 0. Prerequisites

- be on Unibe or UniFR Network  (maybe start VPN)

- have a "IBU cluster" account (have you received the "Welcome to IBU Cluster" email?

??? alert "Welcome to IBU Cluster email"
    Dear John

    we have created an account for you on the IBU computing cluster.

    Login Name: jdoe

    Password will be sent in a separate email.  
    ...

- in the following tutorial, prompts starting with `$` are on your local computer; prompts starting with `[<hpcuser>@binfservms01 ~]$ ` are on the remote server.


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
    Replace *<hpcuser\>* with your Login Name on the server.



```
$ ssh <hpcuser>@binfservms01.unibe.ch
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

    <hpcuser>@binfservms01.unibe.ch's password:
    ```

Enter your password:

??? done "Result"
    ```
    <hpcuser>@binfservms01.unibe.ch's password: ******* *ENTER*
    Last login: Wed Oct 21 16:57:39 2020 from dhcp-100-237.vpn.unibe.ch
    [<hpcuser>@binfservms01 ~]$
    ```


**Exercise 1D:** check content of .ssh directory on the server:

```
[<hpcuser>@binfservms01 ~]$ ls .ssh
```

??? done "Result"
    ```
    ls: cannot access .ssh: No such file or directory
    [<hpcuser>@binfservms01 ~]$
    ```

!!! warning "Warning"
    Logout before moving on to the next exercise.

```
[<hpcuser>@binfservms01 ~]$ exit
```


### 2. Generate user keys pair

**Exercise 2A:** use `ssh-keygen` command to generate a key and encrypt the private key with a passphrase

```
$ ssh-keygen
```

??? done "Result"
    ```
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/<localuser>/.ssh/id_rsa):
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
    Your identification has been saved in /home/<localuser>/.ssh/id_rsa
    Your public key has been saved in /home/<localuser>/.ssh/id_rsa.pub
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
    -rw------- 1 <localuser> <localuser> 2.6K Okt 21 17:01 id_rsa
    -rw-r--r-- 1 <localuser> <localuser>  574 Okt 21 17:01 id_rsa.pub
    -rw-r--r-- 1 <localuser> <localuser>  444 Okt 22 13:56 known_hosts
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
    JdOSn//4njp6aCfg38SopbDn3tfJcJTM=  <localuser>@laptop
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
$ ssh-copy-id <hpcuser>@binfservms01.unibe.ch
```

??? done "Result"
    ```
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/<localuser>/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
     ___ ____  _   _   _     _                         _           _            
    |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __
     | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
     | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
    |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   

    <hpcuser>@binfservms01.unibe.ch's password:
    ```

Enter your password:

??? done "Result"
    === "mac OS/Linux"
        ```
        <hpcuser>@binfservms01.unibe.ch's password: *********** *ENTER*

        Number of key(s) added: 1

        Now try logging into the machine, with:   "ssh '<hpcuser>@binfservms01.unibe.ch'"
        and check to make sure that only the key(s) you wanted were added.
        ```
    === "Windows"
        You will be prompted for your password:

        <figure>
          <img src="../../assets/images/passphrase_while_copy.png" width="300"/>
        </figure>

**Exercise 3B:** login to the server without password

```
$ ssh <hpcuser>@binfservms01.unibe.ch
```

??? done "Result"
    ```
      ___ ____  _   _   _     _                         _           _            
    |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __
     | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
     | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
    |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   

    Enter passphrase for key '/home/<localuser>/.ssh/id_rsa': *********** *ENTER*
    Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
    [<hpcuser>@binfservms01 ~]$
    ```


**Exercise 3B:** check content of .ssh on the server

```
[<hpcuser>@binfservms01 ~]$ ls -l .ssh
```

??? done "Result"
    ```
    -rw-------. 1 <hpcuser> <hpcuser> 988 Oct 21 18:48 authorized_keys
    [<hpcuser>@binfservms01 ~]$
    ```

```
[<hpcuser>@binfservms01 ~]$ cat .ssh/authorized_keys
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
    JdOSn//4njp6aCfg38SopbDn3tfJcJTM= <localuser>@laptop
    [<hpcuser>@binfservms01 ~]$
    ```


### 4. SSH-Agent

This part is platform dependent. Choose here therefore either **mac OS/Linux** or **Windows**.

=== "mac OS/Linux"
    **Exercise 4A:** start ssh-agent


    ```
    $ ssh-agent > sshagent.txt
    $ cat sshagent.txt
    ```

    ??? done "Result"
        ```
        SSH_AUTH_SOCK=/tmp/ssh-JGZOTKftGNqM/agent.23764; export SSH_AUTH_SOCK;
        SSH_AGENT_PID=23765; export SSH_AGENT_PID;
        echo Agent pid 23765;
        $
        ```

    **Exercise 4B:** connect to ssh-agent

    ```
    $ . sshagent.txt
    ```

    ??? done "Result"
        ```
        Agent pid 23765
        ```



    **Exercise 4C:** List identities

    ```
    $ ssh-add -l
    ```

    ??? done "Result"
        ```
        The agent has no identities.
        $
        ```


    **Exercise 4D:** add a key to ssh-agent

    ```
    $ ssh-add .ssh/id_rsa
    ```

    ??? done "Result"
        ```
        Enter passphrase for .ssh/id_rsa: ************* *ENTER*
        Identity added: .ssh/id_rsa (<user>@laptop)
        $
        ```

    **Exercise 4E:** List identities

    ```
    $ ssh-add -l
    ```

    ??? done "Result"
        ```
        3072 SHA256:uw6W41Pg5MMLKwASHK1G3JhRe+UOUQyt4NHNp2grVTo <user>@laptop (RSA)
        $
        ```


    **Exercise 4F:** use ssh-agent


    ```
    $ ssh <hpcuser>@binfservms01.unibe.ch
    ```

    ??? done "Result"
        ```
         ___ ____  _   _   _     _                         _           _            
        |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __
         | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
         | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
        |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   

        Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
        [<hpcuser>@binfservms01 ~]$
        ```


    **Exercise 4G:** reuse a running ssh-agent from a new terminal

    Open a new terminal.


    ```
    $ ssh-add -l
    ```

    ??? done "Result"
        ```
        Could not open a connection to your authentication agent.
        $
        ```


    Load the information to connect to ssh-agent:


    ```
    $ . sshagent.txt
    ```

    ??? done "Result"
        ```
        Agent pid 23765
        ```


    **Exercise 4H:** use ssh-agent

    ```
    $ ssh <hpcuser>@binfservms01.unibe.ch
    ```

    ??? done "Result"
        ```
         ___ ____  _   _   _     _                         _           _            
        |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __
         | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
         | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
        |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   

        Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
        [<hpcuser>@binfservms01 ~]$
        ```

=== "Windows"

    **Exercise 4A:** List identities

    ```sh
    ssh-add -l
    ```

    ??? done "Result"
        ```
        error fetching identities for protocol 1: agent refused operation
        error fetching identities for protocol 2: agent refused operation
        The agent has no identities.
        ```

    **Exercise 4B:** Change the MobaXterm SSH configuration

    In the MobaXterm gui go to **settings > configuration > SSH**. Check the option "Check Use internal SSH agent "MobAgent"" and uncheck "Use external Pageant". So it looks like this:

    <figure>
      <img src="../../assets/images/check_mobagent.png" width="700"/>
    </figure>

    You will be prompted to restart MobaXterm. Choose "Yes".

    From now on, at startup you are asked to give your password for the rsa key (This is the one you chose at exercise 2A):

    <figure>
      <img src="../../assets/images/passphrase_at_startup.png" width="300"/>
    </figure>

    **Exercise 4C:** Check whether the identity is added:

    ```sh
    ssh-add -l
    ```

    ??? done "Result"
        ```sh
        2048 SHA256:iOWN8KEErX5BpIZQg0+zTPMfn6rtOg/l362yFQiY3fs Imported-Openssh-Key: C:\Users\<username>\DOCUME~1\MobaXterm\home\.ssh\id_rsa (RSA)
        ```

    **Exercise 4D:** Login without a passphrase, but with a passphrase protected key.

    ```sh
    $ ssh <hpcuser>@binfservms01.unibe.ch
    ```

    ??? done "Result"
        ```
         ___ ____  _   _   _     _                         _           _            
        |_ _| __ )| | | | | |   (_)_ __  _   ___  __   ___| |_   _ ___| |_ ___ _ __
         | ||  _ \| | | | | |   | | '_ \| | | \ \/ /  / __| | | | / __| __/ _ \ '__|
         | || |_) | |_| | | |___| | | | | |_| |>  <  | (__| | |_| \__ \ ||  __/ |   
        |___|____/ \___/  |_____|_|_| |_|\__,_/_/\_\  \___|_|\__,_|___/\__\___|_|   

        Last login: Wed Oct 21 18:47:46 2020 from dhcp-99-231.vpn.unibe.ch
        [<hpcuser>@binfservms01 ~]$
        ```
