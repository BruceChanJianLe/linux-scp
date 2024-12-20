# Linux scp

This repository demonstrates the usage of scp command in Ubuntu.
Alternatively, please also consider using `rsync`, especially for system backup.

## Basic usage

```bash
usage: scp [-346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           [-l limit] [-o ssh_option] [-P port] [-S program]
           [[user@]host1:]file1 ... [[user@]host2:]file2
```

## Tips

### Basics

Copy from remote host to local host.
```bash
scp remote_username@192.168.168.168:/home/hostname/somefiles.txt .
```

Copy to remote host from local host.
```bash
scp file.txt remote_username@10.10.0.2:/remote/directory
```

### Non Default Port

Default ports is 22, you can change it as below
```bash
scp -P 1022 -r 192.168.1.123:/remote/directory /tmp
```

### Fast Copy

Use RC4 cipher for speed, but only with local trusted network.
```bash
scp -c arcfour -r 192.168.1.123:/remote/directory /tmp
```

### Preserve Permission

```bash
scp -p file.txt remote_username@10.10.0.2:/remote/directory
```

## Reference
- https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/

# RSYNC

rsync also uses ssh for copying!

Flag | Description
---| ---
`-a` | flag for rsync recursive copy with permissions, ownership, symbolic link
`-H` | flag for hard links
`-A` | flag for ACLs
`-X` | flag for extended attributes and SELinux security context
`-z` | flag for compression of files during transfer

```bash
# Your typical daily task
rsync -vaz -HAX --progress /etc 192.168.1.123:/tmp
# Intensely, when you have many rosbags (remote -> local)
ssh remote@192.168.10.103 "find /home/developer/bags -name '3d_lidars*.bag'" | parallel -j 8 rsync -vaz -HAX --progress remote@192.168.10.103:'{}' .
# (local -> remote)
ls 3d_lidars*.bag | parallel -j 8 -vaz -HAX --progress '{}' remote@192.168.10.103:~/Downloads/bags
```

## Note

`rsync /etc` will copy the entire directory while `rsync /etc/` will copy the files within the directory.
To ensure how it works, you can always do a dry run before the actual copy.

```bash
rsync -vaz -HAX --progress --dry-run /etc 192.168.1.123:/tmp
```

## Using Non Default Ports

```bash
rsync -va -e "ssh -p 1022" /etc 192.168.1.123:/tmp
```
