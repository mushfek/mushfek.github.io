---
layout: page
title: Distributed File Sharing System
description: A distributed file sharing system based on Google File System
img: /assets/img/gfs.png
importance: 1
github: https://github.com/mushfek/distributed-file-system
---

This software was developed as the course final project of Distributed Systems Lab.

It was a conceptual implementation of a distributed file sharing system based on the [Google File System](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf). Source code is available on [Github](https://github.com/mushfek/distributed-file-system).  

I simulated the situation using n number of servers (indexing from 0 to n-1) and m number of clients (indexing from 0 to m-1). Communication between servers and clients were done using Java sockets. 

The file system has one directory and a number of text files in that directory. A file in this file system can be of any size. However, a file is logically divided into chunks, with each chunk being at most 8192 bytes (8 kilobytes) in size. 
Chunks of files in the filesystem are actually stored as Linux files on the n servers. All the chunks of a given file need not be on the same server. In essence, a file in this filesystem is a concatenation of multiple Linux files. If the total size of a file in this file system is 8 x + y kilobytes, where y < 8 , then the file is made up of x + 1 chunks, where the first x chunks are of size 8 kilobytes each, while the last chunk is of size y kilobytes.

In future, I intend to develop a Java FX based GUI to emulate a cloud based file storage. There are obviously a lot of improvements needed to me made such as:
1. Add a watcher/heartbeat server to monitor health of the file and metadata servers
2. A pub-sub based architecture to notify clients on any change in server
3. Add a centralized server discovery mechanism so that each time a new server instance is introduced, replication is done for fault-tolerance etc.
