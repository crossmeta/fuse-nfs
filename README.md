#  NFS for Windows using Crossmeta FUSE
This is a port of [@sahlberg/fuse-nfs](https://github.com/sahlberg/fuse-nfs) project to Windows using Crossmeta FUSE interface. Once you have the [latest Crossmeta FUSE](https://github.com/crossmeta/cxfuse/releases/latest) installed the fuse-nfs can be used as described
## How to Use?
1. Start the necessary Crossmeta drivers 
```
c:\program files\crossmeta> service_crossmeta start
c:\program files\crossmeta> net start cxfuse
```
2. Create the mount point where the NFS share will be accessible locally.
```
mkdir v:\mnt\nfsdir
```
3. Mount the NFS share using fuse-nfs program
```
c:\program files\crossmeta> fuse-nfs.exe -m nfs://10.138.0.5/home/user -m /mnt/nfsdir
```
4. Now the NFS share is available from any Windows program as V:\mnt\nfsdir


## NFSv4 support:
NFSv4 is supported when used with a recent enough version of libnfs.To enable NFSv4 support you need to specify version=4 as an URL argument:
```
fuse-nfs.exe -n nfs://127.0.0.1/data/tmp?version=4 -m /my/mountpoint
```



## Porting Notes 
The project has two main components:
* lib-nfs
 The library that provides NFS V3/V4
* fuse-nfs
The FUSE wrapper

By using cross compile environment provided by Mingw32 on Linux it is very easy to port to Crossmeta Windows.
1. Compile lib-nfs to produce libnfs.a (Just used setup.sh from fuse-nfs subdir)
 ```
 ../fuse-nfs/setup.sh
 ./configure  --host=i686-w64-mingw32 
 make
 ```
 2. The library archive will be in `lib/.libs/libnfs.a`. Copy it to the toplevel of libnfs subdir.
 ```
 cp lib/.libs/libnfs.a  .
 ```
 3. The fuse-nfs contains single source file and it is trivial to use our own Makefile than to get buried in automake tool chain. With this fuse-nfs/nfs/Makefile fuse-nfs.exe is built that can work with Crossmeta FUSE.
 
