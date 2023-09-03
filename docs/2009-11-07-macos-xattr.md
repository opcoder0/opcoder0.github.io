---
layout: post
author: opcoder0
---

## Extended Attributes On MacOS X And File Transfer

I recently encountered an interesting issue on Mac OS X. One of the applications I was working on, transfers files to a server. The transfer program first compresses the file using zip algorithms (zlib) and then sends the files to the server via a socket connection. Everything worked well except for some files. These files had extended attributes. HFS/HFS+ filesystems on Mac OS support extended attributes for files. These attributes are metadata that the application creator adds to the files. You can read more about extended attributes from the wikipedia link above.

The following commands to view the [extended attributes](http://en.wikipedia.org/wiki/Extended_file_attributes) on files –

```
$ ls -l@
```

The above command displays the extended attribute names (if they exist) for each of the files in the directory.

Another command that can be used is –

```
$ xattr <filename>
```

The above command can be used to list/add/delete extended attributes.

For some applications stripping off the extended attributes doesn’t make a difference to their functionality. But for others these are essential. Examples of some extended attributes are “com.apple.FinderInfo”, “com.apple.ResourceFork“. The FinderInfo is used by the Mac OS Finder.

Some applications don’t handle extended attributes well one such application being zip. Copying such files to file systems that don’t support extended attributes also result in loss of information. “rsync” has options to copy files with extended attributes. One other thing that is misleading is that these extended attributes don’t contribute to the file-size when viewed via a shell (ls command, stat command etc.). I could only see the difference when viewed via spotlight information. And the checksum of the file remains the same with and without the extended attributes.

However tar doesnt have a problem in keeping the attributes intact.

One way to circumvent the stripping of these attributes is to tar and then zip the files before transfer or write a program that uses getattrlist and setattrlist to handle these attributes separately.

Other useful references for extended attributes and acls –

- [http://forthescience.org/blog/2007/12/11/macosx-leopard-extended-ls/](http://forthescience.org/blog/2007/12/11/macosx-leopard-extended-ls/)
- [http://www.macgeekery.com/tips/the_new_resource_fork](http://www.macgeekery.com/tips/the_new_resource_fork)
- [http://lists.apple.com/archives/Filesystem-dev/2009/jul/msg00011.html](http://lists.apple.com/archives/Filesystem-dev/2009/jul/msg00011.html)
- [http://lists.samba.org/archive/rsync/2007-March/017528.html](http://lists.samba.org/archive/rsync/2007-March/017528.html)
