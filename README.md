# Linux scp

This repository demonstrates the usage of scp command in Ubuntu.

## Basic usage

```bash
usage: scp [-346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           [-l limit] [-o ssh_option] [-P port] [-S program]
           [[user@]host1:]file1 ... [[user@]host2:]file2
```

## Example

Copy from remote host to local host example
```bash
scp username@192.168.168.168:/home/user/somefiles .
```
