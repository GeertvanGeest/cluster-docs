## Linux

1.  Install `sshfs`: `sudo apt install sshfs` or `sudo dnf install sshfs`
2.  Create a mount directory: `mkdir ~/cluster`
3.  Connect: `sshfs username@binfservms01.unibe.ch:/ ~/cluster`
4.  Unmount: `fusermount -zu ~/cluster`

!!! note 
    Add these options if you experience performance problems: `-oauto_cache,reconnect`

## Mac

1.  Install [sshfs and FUSE](https://osxfuse.github.io/)
2.  Create a mount directory `mkdir myMountDir`
3.  Connect: `sshfs -o defer_permissions username@binfservms01.unibe.ch:/ myMountDir/`

!!! note 
    `-o defer_permissions` might be needed to get access to folders outside of your home dir, see this post: [https://github.com/fabiokr/vagrant-sshfs/issues/33](https://github.com/fabiokr/vagrant-sshfs/issues/33)

## Windows

1.  Download the latest version of [winfsp](https://github.com/billziss-gh/winfsp/releases) and install it. (Tested: `WinFsp 2018.2 B4`)
2.  Download the latest version of [sshfs-win](https://github.com/billziss-gh/sshfs-win/releases) and install it. (Tested: `SSHFS-Win 3.2 BETA`)
3.  In PowerShell, type `net use X: \\sshfs\username@binfservms01.unibe.ch\..\..` this will mount the root of `binfservms01` as a network drive under Windows.

!!! warning "Deleting files on Windows"
    Be careful when deleting files with `sshfs`! Let's say you have these files and folders:

    ```text
    .
    |-- test
    |   `-- file.txt -> ../test2/file.txt
    `-- test2
        `-- file.txt
    ```

    After `rm -rf test`, you get the expected result:

    ```text
    .
    `-- test2
        `-- file.txt
    ```

    However, if you delete the folder `test` via sshfs on windows 10, you get:

    ```text
    .
    |-- test
    |   `-- file.txt -> ../test2/file.txt
    `-- test2
    ```

    The folder `test` and its link were not deleted, **sshfs deleted the target file only**! (WTF?!)
