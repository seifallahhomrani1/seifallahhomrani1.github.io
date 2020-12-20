---
title : "What are Sockets ?"
author : Seif-Allah
description : "Introduction to Sockets and how they work. " 
layout : post
category: Networking
tags : [networking,socket]
image : /assets/images/networking/socket.png
---


In C, sockets behavee a lot like files since they use file descriptors to identify themselves. Sockets behave a lot like files that you can actually use the *read()* and *write()* functions to receive and send data using socket file descriptors. However, there are several functions specifically designed for dealing with sockets. 

### socket(int domain, int type, int protocol)
Used to create a new socket, returns a *file descriptor* for the socket or -1 on error. 
### connect(int fd, struct sockaddr *remote_host, socklen_t add_length)
Connects a socket (described by *file descriptor fd*) to a remote host. Returns 0 on success and -1 on error.
### bind(int fd, struct sockaddr *local_addr, socklen_t addr_length)
Binds a socket to a local address so it can listen for incoming connections. Returns 0 on success and -1 on error. 
### listen(int fd, int backlog_queue_size)
Listens for incoming connections and queues connection requests up to *backlog_queue_size*. Returns 0 on success and -1 or error.
### accept(int fd, sockaddr *remote_host, socklen_t *addr_length)
Accepts an incoming connection on a bound socket. The address information from the remote host is written into the *remote_host* structure and the actual size of the address structure is written into *addr_length. This function returns a new socket file descriptor to identify the connected socket or -1 on error. 
### send(int fd, void *buffer, size_t n, int flags)
Sends *n* bytes from *buffer to socket fd; returns the number of bytes or -1 on error. 
### recv(int fd, void *buffer, size_t n, int flags)
Receives *n* bytes from socket fd into *buffer; returns the number of bytes received or -1 on error.

When a socket is created with the *socket()* function, the domain, type, and protocol of the socket must be specified. 
The *domain* refers to the protocol family of the socket. A socket can be used to communicate using a variety of protocol, from the standard Internet protocol used when you browse the Web to amateur radio protocols such as [AX.25](https://www.youtube.com/watch?v=lx6cm1rNDLM)



These are the first 11 protocol families aka domain taken from : /usr/include/x86_64-linux-gnu/bits

/* Protocol families.  */
#define PF_UNSPEC       0       /* Unspecified.  */
#define PF_LOCAL        1       /* Local to host (pipes and file-domain).  */
#define PF_UNIX         PF_LOCAL /* POSIX name for PF_LOCAL.  */
#define PF_FILE         PF_LOCAL /* Another non-standard name for PF_LOCAL.  */
#define PF_INET         2       /* IP protocol family.  */
#define PF_AX25         3       /* Amateur Radio AX.25.  */
#define PF_IPX          4       /* Novell Internet Protocol.  */
#define PF_APPLETALK    5       /* Appletalk DDP.  */
#define PF_NETROM       6       /* Amateur radio NetROM.  */
#define PF_BRIDGE       7       /* Multiprotocol bridge.  */
#define PF_ATMPVC       8       /* ATM PVCs.  */
#define PF_X25          9       /* Reserved for X.25 project.  */
#define PF_INET6        10      /* IP version 6.  */
#define PF_ROSE         11      /* Amateur Radio X.25 PLP.  */


As mentioned before, there are several types of sockets, although stream sockets and datagram socket are the most commonly used.
The types of sockets are defined in socket_type.h file in the same directory (/usr/include/x86_64-linux-gnu/bits). 
> Note : File locations may vary over systems. 


/* Types of sockets.  */  
enum __socket_type
{
  SOCK_STREAM = 1,              /* Sequenced, reliable, connection-based
                                   byte streams.  */  
#define SOCK_STREAM SOCK_STREAM
  SOCK_DGRAM = 2,               /* Connectionless, unreliable datagrams
                                   of fixed maximum length.  */ 



The final argument for the socket() function is the protocol, which should almost always be 0. The specification allows for multiple protocols within a protocol family, so this argument is used to select one of the protocols from the family. In practice however, most protocol families only have one protocol which means this should usually be set for 0.