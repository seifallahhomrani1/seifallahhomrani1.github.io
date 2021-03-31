---
title : "Switching Services"
author : Seif-Allah
description : "Introduction to Raw Sockets, and using it to develop a ping program in C." 
layout : post
category: Networking
image : /assets/images/networking/todo.png
toc : true
---


### Introduction 
Unlike [bridges](https://www.lifewire.com/how-network-bridges-work-816357), which use software to create and manage a filter table, switches use application specific integrated circuits [ASICs](https://en.wikipedia.org/wiki/Application-specific_integrated_circuit)
to build and maintain their filter tables. But it's still okay to think of a layer 2 switch as a multiport bridge because their basic reason for being is the same: to break up collision domains. 

Layer 2 switches and bridges are faster than routers becuase they don't take up time looking at the Network layer header information, Instead, they look at the frame's hardware addresses before deciding to either forward, flood, or drop the frame.

Switches create private, dedicated collision domains and provide independent bandwidth on each port, unlike hubs.
Take an example of multiple hosts connected with a switch to a server using 100Mbps Half-duplex link, unlike with a hub, each host has 10 Mbps dedicated communication to the server.

Layer 2 switching provides the following : 
* Hardware-based bridging (ASIC). 
* Wire speed
* Low latency
* Low cost

What makes layer 2 switching so efficient is that no modification to the data packet takes place. The device only reads the frame encapsulating the packet, which makes the switching process considerably faster and less error-prone than routing processes are.


### Bridging vs LAN Switching


It's true - layer 2 switches really are pretty much just bridges that give us a lot more ports, but there are some important differences you should always keep in mind: 
- Bridges are software based, while switches are hardware based because they use ASIC chips to help make filtering decisions. 
- A switch can be viewed as a multiport bridge. 
- There can be only one spanning-tree instance per bridge, while switches can have many. 
- Switches have a higher number of ports than most bridges. 
- Both bridges and switches forward layer 2 broadcasts. 
- Bridges and switches learn MAC addresses by examining the sources address of each frame received. 
- Both bridges and switches make forwarding decisions based on layer 2 addresses.

### Three Switch Functions at Layer 2 

There are three distinct functions of layer 2 switching (you need to remember these!):
*Address learning, forward/filter decisions, and loop avoidance.*


1. **Address learning** : Layer 2 switches and bridges remember the source hardware address of each frame received on an interface, and they enter this information into a MAC database called a *forward/filter table*. 

2. **Forward/Filter decisions** : When a frame is received on an interface, the switch looks at the destination hardware address and finds the exit interface in the MAC database. The frame is only forwarded out the specified destination port.

3. **Loop avoidance** : If multiple connections between switches are created for redundancy purposes, network loops can occur. Spanning Tree Protocol (STP) is used to stop network loops while still permitting redundancy. 

### Address Learning 
When a switch is first powered on, the MAC forward/filter table is empty. 

![MAC Forward/Filter Table](/assets/images/networking/forwardtable.png)

When a device  transmits and an interface receives a frame, the switch places teh fram's source address in the MAC forward/filter table, allowing it to remember which interface teh sending device is locate on. The switch then has no choice but to flood the network with this frame out of every port except the source port because it has no idea where the destination device is actually located. 

If a device answers this flooded frame and sends a frame back, then the switch will take the source address from that frame and place that MAC address in its database as well, association this MAC address with the interface that received the frame. Since the switch now has both of the relevant MAC adresses in its filtering table, the two devices can now make a point-to-point connection. The switch doesn't need to flood the frame as it did the time because now the frames can and will be forwarded only between the two devices. This is exactly the thing that makes layer 2 switches better than hubs. 


### Forward/Filter Decisions

When a frame arrives at a switch interface, the destination hardware address is compared to the forward/filter MAC database. If the destination hardware address is known and listed in the database, the frame is only sent out the correct interface. The switch doesn't transmit the frame out any interface except for the destination interface. This preserves bandwidth on the other network segments and is called *frame filtering*. 

But if the destination hardware address is not listed in the MAC database, then the frame is flooded out all active interfaces except the interface the frame was received on. If a device answers the flooded frame, the MAC database is updated with the device's location (interface). 

If a host or server sends a broadcast on the LAN, the switch will flood the frame out all active ports except the source port by default. Remember, the switch creates smaller collision domains, but it's still one large broadcast domain by default.

### Port Security 

So just how do you stop someone from simply plugging a host into one of your switch ports, or worse, adding a hub, switch, or access point into the Ethernet jack in their office? By default, MAC addresses will just dynamically appear in your MAC forward/filter database. You can stop them in their tracks by using port security. 


### Loop Avoidance

Redundant links between switches are a good idea because they help prevent complete network failures in the event one link stops working. 

Sounds great, but even though redundant links can be extremely helpful, they often cause more problems than they solve. This is because frames can be flooded down all redundant link simultaneously, creating network loops as well as other evils. Here's a list of the ugliest problems: 

- If no loop avoidance schemes are put in place, the switches will flood broadcasts endlessly throughout the internetwork. This is sometimes referred to as a *broacast storm*. 

- A device can receive multiple copies of the same frame since that frame can arrive from different segments at the same time. 

- You may have thought of this one: The MAC address filter table could be totally confused about the device's location because the switch can receive the frame from more than one link. And what's more, the bewildered switch could get so caught up in constantly updating the MAC filter table with source hardware address locations that it will fail to forward a frame! This is called thrashing the MAC table.

- One of the nastiest things that can happen is multiple loops generating throughout a network. This means that loops can occur within other loops, and if a broadcast storm were to also occur, the network wouldn't be able to perform frame switching ! 

All of these problems spell disaster (or at least close to it) and are decidedly evil situations that must be avoided, or at least fixed somehow. That's where the **Spanning Tree Protocol** comes into the game. It was developed to solve each and every on the problems I just told you about.
