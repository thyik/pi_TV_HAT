# Setup Raspberry Pi NFS share

* Install nfs server

```
$ sudo apt-get install nfs-kernel-server -y

# query pi uid and guid
$ id pi
```
* Take a note of the gid and uid values as you will need these for a step later on

* The Raspberry Pi NFS server software that we installed earlier reads from `exports` file to know what directories to share out over the NFS protocol.

```
$ sudo nano /etc/exports
```

* configure the nfs share folder

```text
/media/hdd_exfat *(rw,all_squash,insecure,async,no_subtree_check,anonuid=1000,anongid=1000)
```

* You can change this to allow specific IP’s or an IP range by changing the `*` to the IP.
* To allow all IP’s from`192.168.0.0` to `192.168.0.256`, we can replace the `*` with `192.168.0.0/24`.
* Options
  + rw – This option allows both read and write requests on the NFS volume.
  + all_squash – This option will map all uids and gids to the anonymous user.
  + insecure – This option allows clients with an NFS implementation that doesn’t use a reserved NFS port.
  + async – This option allows the NFS server to break the NFS protocol to improve performance at the cost of data potentially becoming corrupted if the server crashes.
  + no_subtree_check – This disables subtree checking, while it comes at a cost to security it can improve the reliability of the NFS server. You can read more about this on the exports documentation page we linked earlier.
  + anonuid – This is the UID to utilize for a user that is connecting anonymously.
  + anongid – This is the GID to use for a user that is connecting anonymously.

* Updates the current table of exports available to the NFS server

```
sudo exportfs -ra
```


## Windows NFS client

1. Turn on NFS feature
2. Map a drive
3. Set path to `\\192.168.1.111\media\hdd_exfat`