### What is an inode?

Linux must allocate an index node (inode) for every file and directory in the filesystem. Inodes do not store actual data. Instead, they store the metadata where you can find the storage blocks of each file’s data.

The following metadata exists in an inode:

- File type

- Permissions

- Owner ID

- Group ID

- Size of file

- Time last accessed

- Time last modified

- Soft/Hard Links

- Access Control List (ACLs)

  

### Check the inode number in a specific file

```bash
# stat <filename>
File: mytestfile
Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: ca01h/51713d    Inode: 13          Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:etc_runtime_t:s0
Access: 2021-03-26 15:51:27.036124392 -0500
Modify: 2021-03-26 15:51:27.036124392 -0500
Change: 2021-03-26 15:51:27.036124392 -0500
Birth: -
# ls -lhi <filename>
71935327 -rw-r--r-- 1 root root 0 Jun 19 15:10 testfile

```

### Check the inode usage on filesystems

```bash
# df -ih
Filesystem     Inodes IUsed IFree IUse% Mounted on
devtmpfs          98K   318   98K    1% /dev
tmpfs            102K     1  102K    1% /dev/shm
tmpfs            102K   500  102K    1% /run
tmpfs            102K    17  102K    1% /sys/fs/cgroup
/dev/xvda1       1.3M   46K  1.3M    4% /
```

**When you copy a file, Linux assigns a different inode to the new file.  Things are different when moving a file. As long as the file does not change filesystems**



### Best practices to keep the inode usage low (solution)

You should check your inode usage because excessive usage can lead to issues when creating newer files. Perform the following steps to keep your usage low:

1. ***Delete unnecessary files and directories.***
2. Delete cache files.
3. Delete old email files.
4. Delete temporary files.

#### [Docker is using all my inodes (solution)](https://blog.jroddev.com/docker-is-using-all-my-inodes/)

### What could happen if you never attend to inodes?

Even though the server has free disk space, the server can run out of inodes, which can result in the following consequences when the server does not have enough inodes when creating more files:

- You might lose data.
- The applications might crash.
- The server might restart.
- Processes might not restart.
- Scheduled tasks might not run.