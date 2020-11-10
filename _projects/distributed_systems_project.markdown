---
layout: page
title: Distributed File Sharing System
description: A distributed file sharing system based on Google File System
img: /assets/img/gfs.png
importance: 1
---

This software was developed as the course final project of Distributed Systems Lab.

It was a conceptual implementation of a distributed file sharing system based on the GFS research paper. 

The system has two clients and a metadata server (M-server) to emulate a distributed file system. The program was extensible for any number of servers and clients. The file system has one directory and a number of text files in that directory. A file in this file system can be of any size. However, a file is logically divided into chunks, with each chunk being at most 8192 bytes (8 kilobytes) in size. Chunks of files in the filesystem are actually stored as Linux files on the three servers. All the chunks of a given file need not be on the same server. In essence, a file in this filesystem is a concatenation of multiple Linux files. If the total size of a file in this file system is 8 x + y kilobytes, where y < 8 , then the file is made up of x + 1 chunks, where the first x chunks are of size 8 kilobytes each, while the last chunk is of size y kilobytes.

For example, a file namedfilexmay be of length 20 kilobytes, and be made up of three chunks: the first chunk of size 8 kilobytes stored in serverS 1 with local file nameABC, the second chunk of size 8 kilobytes stored in server S 3 with local file nameDEF, and the third chunk of size 4 kilobytes stored in serverS 2 with local file nameGHI.

In steady state, the M-server maintains the following metadata about files in the file system: file name, names of Linux files that correspond to the chunks of the file, which server hosts which chunk, when was a chunk to server mapping last updated.

Initially, the M-server does not have the chunk name to server mapping, nor does it have the corresponding time of last mapping update. Every 5 seconds, the servers send heartbeat messages to the M-server with the list of Linux files they store. The M-server uses these heartbeat messages to populate/update the metadata. If the M-server does not receive a heartbeat message from a server for 15 seconds, the M-server assumes that the server is down and none of its chunks is available.

Clients wishing to create a file, read or append to a file in the file system send their request (with the name of the file) to the M-server. If a new file is to be created, the M-server randomly asks one of the servers to create the first chunk of the file, and adds an entry for that file in its directory. For read and append operations, based on the file name and the offset, the M-server determines the chunk, and the offset within that chunk where the operation has to be performed, and sends the information to the client. Then, the client directly contacts the corresponding server and performs the operations.

In effect, clients and servers communicate with each other to exchange data, while the clients and servers commu- nicate with the M-server to exchange metadata. The maximize amount of data that can be appended at a time is assumed 2048 bytes. If the current size of the last chunk of the file, where the append operation is to be performed, isSsuch that 8192 − S < appended data size then the rest of that chunk is padded with a null character, a new chunk is created and the append operation is performed there.
