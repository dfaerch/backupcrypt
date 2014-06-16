# Backupcrypt
*WARNING: This is alpha software*

## Description

This is a tool that will allow you to pipe plain-text in to STDIN and get
cipher-text out of STDOUT. The main feature of this encryption setup, is that
it allows for creation of client-side encrypted backups, that support deduplication and/or incremental backing
up.


## Setup

You need to create a "key-file". This is a simple text-file with your
encryption password/pass-phrase on the first line of the file (and the file
must contain nothing else).

Example:
    $ echo "S00perS3cr3tP@zzw0rt" > ~/.backupcrypt.key


## Usage examples:

Example of encrypting and decrypting in one go (just a simple test to see that everything works) 
    $ cat /etc/passwd | ./backupcrypt -f ~/.backupcrypt.key | ./backupcrypt -d

Encrypting and backing up /etc/ incrementally, to a remote server, using bup. (https://github.com/bup/bup)
    $ tar -cvf - /etc | ./backupcrypt -f ~/.backupcrypt.key |bup split -r dan@192.168.1.1: -n enc-local-etc
 
Restoring that /etc/ backup, to /tmp/etc
    $ bup join -r dan@195.69.128.162: enc-local-etc | ./backupcrypt -d |tar -xf - -C /tmp/