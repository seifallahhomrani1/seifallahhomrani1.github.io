---
layout : post
title : "What is the Internet?"
author : Seif-Allah
category : Networking
toc : true 
description : The bigger picture of the internet mechanism.
image : assets/images/networking/internet.png
---

### The bigger picture 

The Internet is a computer network that interconnects hundreds of millions of computing devices throughout the world. In Internet jargon, all of these devices are called **hosts** or **end systems**. End systems are connected together by a network of **communication links** and **packet switches**. Different links can transmit data at different rates, with the **transmission rate** of a link measured in bits/second. When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as **packets** in the jargon of computer networks, are then sent through the network to the destination end system, where they are reassembled into the original data.

A packets switch takes a packet arriving on one if its incoming communication links and forwards that packet on one of its outgoing communication links. Packets switches come in many shapes and flavors, but the two most prominent types in today's Internet are **routers** and **link-layer switches**. Both types of switches forward packets toward their ultimate destinations. Link-layer switches are typically used in access networks, while routers are typically used in the network core. The sequence of communication links and packets switched traversed by a packet from the sending end system to the receivinb end system is known as a **route** or **path** through the network. End systems, packet swithces, and other pieces of the Internet run **protocols** that control the sending and receiving of information within the Internet. THe **Transmission Control Protocol (TCP)** and the **Internet Protocol (IP)** are two of the most important protocols in the Internet. The IP protocol specifies the format of the packets that are sent and received among routers and end systems. The Internet's principak protocols are collectively know as **TCP/IP**. Given the importance of protocols to the Internet, it's important that everyone agree on what each and every protocol does, so that people can create systems and products that interoperate. This is where standards come into play. **Internet standards** are developed by the Internet Engineering Task Force (IETF). The IETF  standards documents are called **request for comments (RFCs)**. RFCs started out as general requests for comments (hence the name) to resolve network and protocol design problems that faced the precursor to the Internet. RFCs tend to be quite technical and detailed. They define protocols such as TCP, IP, HTTP(for the web), and SMTP(for e-mail). There are currently more than 6000 RFCs. Other bodies also specify standards for network components, most notably for network links. The IEEE 802 LAN/MAN Standards Committee, for example, specifies the Ethernet and Wireless WiFi standards.

### A Services description
Our discussion above has identified many of the pieces that muke up the Internet. But we can also describe the Internet from an entirely different angle - namely, as an infrastructure thata provides services to applications. These applications include electronuc mail, Web surfing, social networksn instant messaging, Voice-over-Ip (VoIP), and much, much more. The applications are said to be **distributed applications**, since they involve multiple end systems that exchange data with each other. Internet applications run on end systems - they do not run in the packets switches in the network core. Although packet switches facilitate the exchange of data among end systems, they are not concerned with the application that is the source of sink of date. End systems attached to the Internet provide an **Application Programming Interface (API)** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system. This Internet API is a set of rules that the sending program must follow so that the Internet can deliver the data to the destination program. 

### What is a Network Protocol?
A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

The Internet, and computer networks in general, make extensive use of protocols. Differenet protocols are used to accomplish different communication tasks.

