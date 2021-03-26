---
title : "IPV6"
author : Seif-Allah
description : "IPV6 is the latest version of the internet Protocol, people refer to IPV6 as 'the next-generation Internet Protocol' and it was originally created as the answer to IPv4's inevitable, looming address-exhaustion crisis."
layout : post
category: Networking
image : assets/images/networking/todo.png
toc : true
---




### Introduction

IPV6 is the latest version of the internet Protocol, people refer to IPV6 as "the next-generation Internet Protocol" and it was originally created as the answer to IPv4's inevitable, looming address-exhaustion crisis.

The IPv6 header and address structure has been completely overhauled, and many of the features that were basically just afterthoughts and addendums in IPv4 are now included as full-blown standards in IPv6. It's seriously well equiped, poised, and ready to manage the mind-blowing demands of the Internet to come.


### Why Do We Need IPv6 ? 

Well, the short answer is, because we need to communicate, and our current system isn't really cutting it anymore, kind of like how the Pony Express can't compete with airmail. Just look at how much time and effort we've invested in coming up with slick new ways to conserve bandwidth and IP addresses. We've even come up with Variable Length Subnet Masks (VLSMs) in our struggle to overcome the worsening address drought.


### The Benefits and Uses of IPv6 

So what's so fa

--------------------------------
TO DO 



### IPv6 Addressing and Expressions 

Just as understanding how IP addresses are structured and used is critical with IPv4 addressing, it's also vital when it comes to IPv6. You've already read about the fact that at 128 bits, and IPv6 address is much larger than an IPv4 address.

Here's an example of an IPv6 address : 

2001:0db8:3c4d:0012:0000:0000:1234:56ab

Breaking down this IPv6 address into 3 sections : 

1. **Global Prefix** : 2001:0db8:3c4d
2. **Subnet** : 0012
3. **Interface ID** : 0000:0000:1234:56ab

So as you can now see, the address is truly much larger, but what else is different? Well, first, notice that it has eight groups of numbers instead of four and also that those groups are separated by colons instead of periods. And hey wait a second.. There are letters in that address! Yep, the address is expressed in hexadecimal just like a MAC address is, so you could say this address has eight 16-bit hexadecimal colon-delimited blocks. That's already quite a mouthful, and you probably haven't even tried to say the address out loud yet!

One other thing I want to point out us for when you set up your test network to play with IPv6, is when you use a web browser to make an HTTP connection to an IPv6 device, you have to type the address into the browser with brackets [] around the literal address. Why ? Well, a colon is already being used by the browser for specifying a port number. So basically, if you don't enclose the address in brackets, the browser will have no way to identify the information.

### Shortened Expression 

The good news is there are a fex tricks to help rescue us when writing these monster addresses. For one thing, you can actually leave out parts of the address to abbreviate it, but to get away with doing that you have to follow a couple of rules. 
First, you can drop any leading zeros in each of the individual blocks.    

After you do that, the sample address from earlier would then look like this:

2001:db8:3c4d:12:0:0:1234:56ab

Okay, that's a definite improvement, at least we don't have to write all of those extra zeros!
But what about whole blocks that don't have anything in them except zeros? Well, we can kind of lose those too, at least some of them. Again referring to our sample address, we can remove the two blocks of zeros by replacing them with double colons, like this: 

2001:db8:3c4d:12:0:0:1234:56ab

Cool, we replaced the blocks of all zeros with double colons. The rule you have to follow to get away with this is that **you can only replace one contiguous block of zeros** in an address. So if my address has four blocks of zeros and each of them were separated, I just don't get to replace them all; remember the rule is that you can only replace one contiguous block with a double colon. Check out this example : 

2001:0000:0000:0012:0000:1234:56ab

And just know that **you can't do this**: 

2001::12::1234:56ab

Instead, this is the best that you can do:

2001::12:0:0:1234:56ab

The reason why the above example is our best shot is that if we remove two sets of zeros, the device looking at the address will have no way of knowing where the zeros go back in. Basically, the router would look at the incorrect address and say, "Well, do I place two blocks into the first set of double colons and two into the second st, or do I place three blocks into the first set and one block into the second set?" and on and on it would go because the information the router needs just isn't there.

### Address Types 

We're all familiar with IPv4's unicast, broadcast, and multicast addresses that basically define who or at least how many other devices we're talking to. But, the IPv6 adds to that trio and introduces the anycast. Broadcasts, as we know them, have been eliminated in IPv6 because of their cumbersome inefficiency.

So let's find out what each of these types of IPv6 addressing and communication methods do for us.

* **Unicast** : Packets addressed to a unicast address are delivered to a single interface. For load balancing, multiple interfaces can use the same address. There are a few different types of unicast addresses, but we don't need to get into here.

* **Global Unicast Addresses** These are your typical publicly routable addresses, and they're the same as they are in IPv4.

* **Link-Local addresses** These are like the private addresses in IPv4 in that they're not meant to be routed. Think of them as a handy tool that gives you the ability to throw a temporary LAN together or for creating a small LAN that's not going to be routed but still needs to share and access files and services locally.

* **Unique local addresses** These addresses are also intended for non-routing purposes, but they are nearly globally unique, so it's unlikely you'll ever have one of them overlap. Unique local addresses were designed to replace site-local addresses, so they basically do almost exactly what IPv4 private addresses do : allow communication throughout a site while being routable to multiple local networks. Site-local addresses were denounced as of September 2004. 

* **Multicast** Again, same as in IPv4, packets addresses to a multicast address are delivered to all interfaces identified by the multicast address. Sometimes people call them one-to-many addresses. It's really easy to spot a multicast address in IPv6 because they always start with FF. 

* **Anycast** Like multicast addresses, an anycast address identifies multiple interfaces, but there's a big difference: the any cast packet is only delivered to one address, actually, to the first one it finds defined in terms of routing distance. And again, this address is special because you can apply a single address to more than one interface. You could call them one-to-one-of-many addresses, but just saying "anycast" is a lot easier.



You're probably wondering if there are any special, reserved addresses in IPv6 because you know they're in IPv4. Well there are plenty of them ! Let's go over them now. 

### Special Addresses

I'm going to list some of the addresses and address ranges that you should definitely make a point to remember because you'll eventually use them. They're all special or reserved for specific use, but unlike IPv4, IPv6 gives us a galaxy of addresses, so reserving a few here and there doesn't hurt a thing! 

**0:0:0:0:0:0:0:0** Equals **::**. This is the equivalent of IPv4's 0.0.0.0, and is typically the source address of a host when you're using stateful configuration.
 
**0:0:0:0:0:0:0:1** Equals **::1**. The equivalent of 127.0.0.1 in IPv4. 

**0:0:0:0:0:0:192.168.100.1** This is how an IPv4 address would be written in a mixed IPv6/IPv4 network environment.

**2000::/3** The global unicast address range.

**FC00::/7** The unique local unicast range.

**FE80::/10** The link-local unicast range.

**FF00::/8** The multicast range.

**3FFF:FFFF::/32** Reserved for examples and documentation. 

**2001:0DB8::/32** Also reserved for examples and documentation

**2002::/16** Used with 6to4 , which is the transition system, the structure that allows IPv6 packets to be transmitted over an IPv4 network without the need to configure explicit tunnels.

### How IPv6 Works in an InternetWork

It's time to explore the finer points of IPv6. A great place to start is by showing you how to address a host and what gives it the ability to find other hosts and resources on a network. 

I'll also demonstrate a  device's ability to automatically address itself, something called stateless autoconfiguration, plus another type of autoconfiguration known as stateful. Keep in mind that stateful autoconfiguration uses a DHCP server in a very similar way to how it's used in an IPv4 configuration. I'll also show you how Internet Control Message Protocol (ICMP) and multicast works for us on an IPv6 network.

### Autoconfiguration

Autoconfiguration is an incredibly useful solution because it allows devices on a network to address themselves with a link-local unicast address. This process happens through first learning the prefix information from the router and then appending the device's own interface address as the interface ID. But where does it get that interface ID? Well, you know every device on an Ethernet network has a physical MAC address, and that's exactly what's used for the interface ID. But since the interface ID in an IPv6 is 64 bits length and a MC address is only 48 bits, where do the extra 16 bits come from? The MAC address is **padded** in the middle with the extra bits, it's **padded with FFFE**.

For example, let's say I have a device with a MAC address that looks like this: 00:60:d6:79:19:87

After it's been padded, it would look like this: 02:60:d6:FF:FE:73:19:87

So where did that 2 in the beginning of the address come from ? another good question.

You see, part of the process of padding (called modified eui-644 format) changes a bit to specify if the address is locally unique or globally unique. And the bit that gets changed is the seventh bit in the address. A bit value of 1 means globally unique, and a bit value of 0 means locally unique, so looking at this example, would you say that this address is globally or locally unique? If you answered that it's globally unique address, you're right! Trust me, this is going to save you time in addressing your hosts machines because they communicate with the router to make this happen.

To perform autoconfiguration, a host goes through a basic twi-step process: 

1. First, the host needs the prefix information (similar to the network portion of an IPv4 address) to configure its interface, so it sends a router solicitation (RS) request for it. This RS is then sent out as a multicast to each router's multicast address. The actual information being sent is a type of ICMP message, and like everything in networking, this ICMP message has a number that identifies it. The RS message is ICMP type 133.

2. The router answers back with the required prefix information  via a router advertisement (RA). An RA message also happens to be a multicast packet that's sent to each nod's multicast address and is ICMP type 134. RA messages are sent on a periodic basis, but the host sends the RS for an immediate response so it doesn't have to wait until the next scheduled RA to get what it needs. 

By the way, this type of autoconfiguration is also know as stateless autoconfiguration because it doesn't contact or connect any further information from the other device. 
