---
title: SSHFS 使用
date created: 2022-06-18 17:53
status: note
tags:
  - sshfs
  - fstab
  - linux
  - 挂载
  - fuse
  - group
aliases: SSHFS 使用
---
# SSHFS 使用

## 安裝

## fuse group

umount sshfs-dir

https://www.linode.com/docs/guides/using-sshfs-on-linux/

### fuse: failed to open mountpoint for reading: Too many levels of symbolic links
umount 即可

### Group 詳解

[[group-and-user]]


## 掛載指令

```
sshfs -p <port> user@example.com:/remote/mnt /local/mount -o
```
其中 -p `<port>` 也可換爲 -o port=`<port>`

-o 后加需要的参数

## 参数说明（SSHFS 文檔）

usage: sshfs [user@]host:[dir] mountpoint [options]

general options:

    -o opt,[opt...]        mount options
    -h   --help            print help
    -V   --version         print version

SSHFS options:

    -p PORT                equivalent to '-o port=PORT'
    -C                     equivalent to '-o compression=yes'
    -F ssh_configfile      specifies alternative ssh configuration file
    -1                     equivalent to '-o ssh_protocol=1'
	
    -o reconnect           reconnect to server
    -o delay_connect       delay connection to server
    -o sshfs_sync          synchronous writes
    -o no_readahead        synchronous reads (no speculative readahead)
    -o sync_readdir        synchronous readdir
    -o sshfs_debug         print some debugging information
    -o cache=BOOL          enable caching {yes,no} (default: yes)
    -o cache_max_size=N    sets the maximum size of the cache (default: 10000)
    -o cache_timeout=N     sets timeout for caches in seconds (default: 20)
    -o cache_X_timeout=N   sets timeout for {stat,dir,link} cache
    -o cache_clean_interval=N
                           sets the interval for automatic cleaning of the
                           cache (default: 60)
    -o cache_min_clean_interval=N
                           sets the interval for forced cleaning of the
                           cache if full (default: 5)
    -o workaround=LIST     colon separated list of workarounds
             none             no workarounds enabled
             [no]rename       fix renaming to existing file (default: off)
             [no]truncate     fix truncate for old servers (default: off)
             [no]buflimit     fix buffer fillup bug in server (default: on)
             [no]fstat        fix fstat for old servers (default: off)
    -o idmap=TYPE          user/group ID mapping (default: none)
             none             no translation of the ID space
             user             only translate UID/GID of connecting user
             file             translate UIDs/GIDs contained in uidfile/gidfile
    -o uidfile=FILE        file containing username:remote_uid mappings
    -o gidfile=FILE        file containing groupname:remote_gid mappings
    -o nomap=TYPE          with idmap=file, how to handle missing mappings
             ignore           don't do any re-mapping
             error            return an error (default)
    -o ssh_command=CMD     execute CMD instead of 'ssh'
    -o ssh_protocol=N      ssh protocol to use (default: 2)
    -o sftp_server=SERV    path to sftp server or subsystem (default: sftp)
    -o directport=PORT     directly connect to PORT bypassing ssh
    -o slave               communicate over stdin and stdout bypassing network
    -o disable_hardlink    link(2) will return with errno set to ENOSYS
    -o transform_symlinks  transform absolute symlinks to relative
    -o follow_symlinks     follow symlinks on the server
    -o no_check_root       don't check for existence of 'dir' on server
    -o password_stdin      read password from stdin (only for pam_mount!)
    -o SSHOPT=VAL          ssh options (see man ssh_config)

FUSE options:

    -d   -o debug          enable debug output (implies -f)
    -f                     foreground operation
    -s                     disable multi-threaded operation

    -o allow_other         allow access to other users
    -o allow_root          allow access to root
    -o auto_unmount        auto unmount on process termination
    -o nonempty            allow mounts over non-empty file/dir
    -o default_permissions enable permission checking by kernel
    -o fsname=NAME         set filesystem name
    -o subtype=NAME        set filesystem type
    -o large_read          issue large read requests (2.4 only)
    -o max_read=N          set maximum size of read requests

    -o hard_remove         immediate removal (don't hide files)
    -o use_ino             let filesystem set inode numbers
    -o readdir_ino         try to fill in d_ino in readdir
    -o direct_io           use direct I/O
    -o kernel_cache        cache files in kernel
    -o [no]auto_cache      enable caching based on modification times (off)
    -o umask=M             set file permissions (octal)
    -o uid=N               set file owner
    -o gid=N               set file group
    -o entry_timeout=T     cache timeout for names (1.0s)
    -o negative_timeout=T  cache timeout for deleted names (0.0s)
    -o attr_timeout=T      cache timeout for attributes (1.0s)
    -o ac_attr_timeout=T   auto cache timeout for attributes (attr_timeout)
    -o noforget            never forget cached inodes
    -o remember=T          remember cached inodes for T seconds (0s)
    -o nopath              don't supply path if not necessary
    -o intr                allow requests to be interrupted
    -o intr_signal=NUM     signal to send on interrupt (10)
    -o modules=M1[:M2...]  names of modules to push onto filesystem stack

    -o max_write=N         set maximum size of write requests
    -o max_readahead=N     set maximum readahead
    -o max_background=N    set number of maximum background requests
    -o congestion_threshold=N  set kernel's congestion threshold
    -o async_read          perform reads asynchronously (default)
    -o sync_read           perform reads synchronously
    -o atomic_o_trunc      enable atomic open+truncate support
    -o big_writes          enable larger than 4kB writes
    -o no_remote_lock      disable remote file locking
    -o no_remote_flock     disable remote file locking (BSD)
    -o no_remote_posix_lock disable remove file locking (POSIX)
    -o [no_]splice_write   use splice to write to the fuse device
    -o [no_]splice_move    move data while splicing to the fuse device
    -o [no_]splice_read    use splice to read from the fuse device

Module options:

[iconv]

    -o from_code=CHARSET   original encoding of file names (default: UTF-8)
    -o to_code=CHARSET      new encoding of the file names (default: UTF-8)

[subdir]

    -o subdir=DIR           prepend this directory to all paths (mandatory)
    -o [no]rellinks         transform absolute symlinks to relative
	
	
-o umask=M
    set file permissions (octal) 
	
umask 选项，这个选项用于设置挂载后的文件的权限，如果使用这个选项，那么挂载后, 在client端的权限无法进行修改，如果用chmod在客户端修改了，那么客户端显示的权限依然不变，但是server end 可能就发生了变化，这一点非常奇葩,也非常需要注意，别以为客户端没有改变，服务端也不会改变； 另外这个选项在 mount 之后在内存是看不到这个选项的，也就是会生效，但是不体现在内存里面保存的挂载参数里面


## 開機啓動設置（fstab）

### 格式

```
username@example.com:/home/mnt /path/to/mount fuse.sshfs 参数 0 0
```

参数中不要加 `noauto`，会导致挂载指令被忽视

### 验证并挂载

`mount -fav

- f fake
- v say what is being done
- a all

无误后 `mount -av`

參考： 
- https://blog.csdn.net/richerg85/article/details/17917129
- https://wiki.archlinux.org/title/SSHFS#fstab_mounting_issues
- https://unix.stackexchange.com/a/519820 （idmap）







