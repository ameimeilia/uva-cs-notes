---
layout: default
title: Protocols
parent: Readings
nav_order: 12
---
# Protocols
https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html

## 1 Packets with headers

In general, any piece of communication to be handled by intermediaries needs two pieces of information: the contents of the communication and the routing information needed to reach the intended destination.

### 1.1 Paper

For paper communication, these would be the letter and the envelope, each made of paper and each with information written on them, but one (the letter) containing the contents and the other (the envelope) containing the routing information. We know which one is which by how they are arranged: the envelope is outside the letter.

If we wanted to send an entire book we could invest in a different sending protocol such as a box instead of an envelope, but we could also take a few pages at a time and put each in an envelope. As long as the pages are numbered, the recipient could reassemble the book no matter what order the envelopes arrived in. If we had a book with unnumbered pages, we could add the ordering to the envelope instead to still permit reassembling the book.

### 1.2 Digital

Common network communications use many of the same ideas as paper post.

Data

The contents you want to send is the message or data.

Packet / Segment / Frame

Many protocols split the message into packets of some maximum (or in a few cases, fixed) size, sending each separately with some sort of reassembly packet order numbers.

The name for the chopped-up data depends on the [layer](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#layers).

Header / Footer

To get information where it needs to go, a header with routing information needs to be provided first. It is followed by the packet of data to be transmitted. Some protocols add a footer as well to mark the end of the packet.

### 1.3 Layers

Paper post is typically transmitted part of the way by a letter inside an envelope inside a crate inside a truck, another part by a letter inside an envelope inside a mail pouch, etc. Each of these outer containers has routing information, like the envelope does: the carrier route of the pouch, the post office destination of the crate, etc.

Digital communication also puts containers inside containers and the set of containers used are called layers. There are different numbers of layers possible, but [the OSI model](https://en.wikipedia.org/wiki/OSI_model#Description_of_OSI_layers) is often seen as the default set, even though [the Internet Protocol Suite](https://en.wikipedia.org/wiki/Internet_protocol_suite#Key_architectural_principles) is better known. Note that OSI numbers its layers, where containers have smaller numbers than their contents.

Consider using a `SOCK_STREAM` `socket`, as you did in [COA1](https://www.cs.virginia.edu/luther/COA1/F2019/lab09-sockets.html). When you `write` a message, the result is

- The message you sent over the socket, wrapped in
- a TCP header with the port number to get it to the right program, wrapped in
- an IP header with the IP address to get it to the right computer, wrapped in
- an IEEE 802.11 header and footer to get it over the Wireless LAN to the Internet

That last layer will be stripped off after traversing the WLAN to be replaced by a different outer envelope to guide it over the next link on the way to its destination.

Note that because of the header/footer design, every layer can slice the data provided by the other layers freely. Thus, a message could be split into 4 TCP segments; those each split into 2 IP packets to make 8 total packets. Multiple messages can also be placed into a single envelope, though that is less common in practice.

## 2 TCP/IP and friends

While there are many, many protocols out there, three are pervasive enough to be worth ensuring every CS major has at least some understanding of them: TCP, IP, and UDP. These sit in the middle of the network layer stack: they transmit higher-level protocols like HTTP, SMTP, SSH, etc.; and they are transmitted by lower-level protocols like Wi-Fi, ethernet, etc.

|Layer|Protocols|Use|Message Name|
|---|---|---|---|
|Application|HTTP, SSH, SMTP, [etc.](https://en.wikipedia.org/wiki/Category:Application_layer_protocols)|programmer-visible messages||
|Transport|**TCP**, **UDP**|reach correct program|datagram (UDP), segment (TCP)|
|Internet|**IP**, which has two versions: IPv4 and IPv6|reach correct computer||
|Link|MAC, Wi-Fi, [etc.](https://en.wikipedia.org/wiki/Category:Link_protocols)|frame|traveling over wires and the air|

### 2.1 IP

IP, or Internet Protocol, has two versions in use (IPv4 and IPv6). There are some important technical updates in version 6, and several other nuanced ideas this write-up ignores.

IP (both versions) works on the following principles:

- Every computer has a unique numeric IP Address.
    
    IPv4 typically writes its 32-bit addresses as 4 8-bit decimal values with periods in between them, as in `255.127.63.31` for `0xFF7F3F1F`.
    
    IPv6 typically writes its 128-bit addresses as 8 16-bit hexadecimal values separated with colons in square brackets, with a run of all-zero values omitted, as in `[c0a:2::2020]` for `0x0c0a0002000000000000000000002020`. Sometimes the square brackets are omitted, as in `coa:2::2020`.
    
    The fact that there are more than 232 computers is part of the motivation to switch from IPv4 to IPv6.
    
    Typically, all the machines on a local network will have IP addresses from the same range. Machines repsonsible for forwarding messages from one network to another, called _routers_, can take of advantage of this to track what messages go to which links.
    
- IP headers contain the IP address of both the originating and end-destination computer.
    
- Messages are delivered via best-effort delivery. Each computer involved in delivering a packet sends the packet to the computer that it has a link to and that it believes is most likely to get the message where it is going, but there is no guarantee that:
    
    - the packet will arrive at all,
    - that the sender will know if it arrived,
    - what order the message will be received in relative to other messages, and
    - that the message data won’t be corrupted in transit

### 2.2 UDP

UDP, or User Datagram Protocol, is a lightweight transport protocol. It adds 16-bit source and destination port numbers that the OS of the involved computers can use to ensure it gets to the correct program and a checksum to detect errors that might arise during transmission.

UDP adds no other features on top of the underlying IP: messages are not guaranteed to arrive at all, nor in any particular order; cannot be dynamically split into different sizes; etc. As such, it adds little if any delay on top of IP and is used for speed-sensitive messages like clock synchronization and streaming media. Software that uses UDP generally has to be designed on the assumption that some packets that are sent will never arrive.

### 2.3 TCP

TCP, or Transmission Control Protocol, is a reliable, ordered, connection-oriented transport protocol. It creates this over IP by a somewhat complicated state machine; a core idea in this is that each received message must be acknowledged by the recipient and will be re-sent by the sender otherwise.

To deal with splitting the possibly very large stream of data into a connection into smaller messages and preserving order, each message has a _sequence number_, letting the receiver’s OS ensure that messages are given to the receiving program in order even if they are not received from the network in order.

Like UDP, TCP includes 16-bit port numbers to let the OSes of the involved computers identify which program and connection packets are associated with. ALso like UDP, it uses checksums to detect when messages are corrupted.

![Simplified TCP state diagram for how to establish and terminate a connection (from Wikimedia)](https://upload.wikimedia.org/wikipedia/commons/f/f6/Tcp_state_diagram_fixed_new.svg)

Simplified TCP state diagram for how to establish and terminate a connection (from [Wikimedia](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#/media/File:Tcp_state_diagram_fixed_new.svg))

Suppose two computers PP and QQ have established a TCP connection with one another. PP might send QQ the message This is a triumph! as follows, using (contents, sequence number) to represent a TCP segment:

|PP|IP|QQ|
|---|---|---|
|sends (This, 1)|||
||→||
|||receives (This, 1)|
|||sends (ACK, 2)|
||←||
|receives (ACK, 2)|||
|sends (” is a “, 2)|||
||(dropped)||
|sends (triumph!, 3)|||
||→||
|||receives (triumph!, 3)|
|||expected message number 2, not received|
|||resends (ACK, 2)|
||←||
|receives (ACK, 2), which means segment 2 never arrived…|||
|resends (” is a “, 2)|||
||→||
|resends (triumph!, 3) as it came after segment 2|||
||→||
|||receives (triumph!, 3)|
|||receives (” is a “, 2)|
|||sends (ACK, 4)|
||←||
|receives (ACK, 4)|||
|sends (FIN) to begin closing connection|||

In general, either side may re-send a message if they have not received the expected response, and neither needs to wait for the other’s message to send the next. This way, if messages seem to be arriving well, more messages may be sent in groups; if acknowledgments are not arriving, transmission may be slowed down to wait for ACKs before the next sending.

TCP is significantly slower than UDP, requiring overhead to establish and shut down a connection, but provides reliable delivery over unreliable IP. The TCP over IP combination is so prevalent that TCP/IP is sometimes used as a term for the entire Internet protocol suite.

## 3 URLs

We are accustomed to navigating the web with Universal Resource Locators (URLs). A URL is a special type of Uniform Resource Indicator (URI)[1](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#fn1) and has [many components](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Generic_syntax), but three will suffice for this introduction.

- A _scheme_ like HTTP or HTTPS; this identifies the protocol used at the application layer.
    
- A _hostname_, which (conceptually) identifies a particular computer.
    
- A _path_, which is given to the computer to identify a specific resource.
    

Note that the hostname here performs a similar function as an IP address, but for different audiences. IP addresses are organized to help computers locate one another. Hostnames are organized to help humans locate computers.

In the URL [https://www.cs.virginia.edu/COA2](https://www.cs.virginia.edu/COA2),

- The _scheme_ is `https`
- The _hostname_ is `www.cs.virginia.edu`
- the _path_ is `/COA2`

### 3.1 The value of a mapping

When you visit a URL, your browser first needs to convert the hostname in the URL to the IP address of a computer so that it can use TCP/IP to route your request to the appropriate server. It does this by consulting a special mapping data structure, which is maintained online and can be updated by the party owning the hostname. That means it first queries the Internet for the IP address of the hostname before querying it again with the website request itself.

Having such a distributed mapping between what users type and what computers do allows many conveniences, such as the ability to change servers without the need to have everyone learn the new server’s IP addresses. However, it also comes at a price: someone has to decide who gets to change the IP address of `www.cs.virginia.edu`, which means someone has to know who owns each address and prevent others from taking it. This work requires resources and human judgment calls, meaning it requires money, meaning it costs money to register a hostname.

### 3.2 From files to DNS

In the earliest days of the ARPANET, the mapping between hostnames and IP addresses was a single file, `HOSTS.TXT`. Each line contained a hostname and an IP address[2](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#fn2). This was initially stored on one computer, and users learned its contents by calling up an operator via telephone and asking the operator for the IP address of a given host. Later these telephone calls were moved to a digital protocol, `WHOIS`, but it remained initially on one computer owned by one company, SRI.

As this directory grew in importance, SRI needed to decide who could have which hostnames. [Elizabeth Feinler](https://en.wikipedia.org/wiki/Elizabeth_J._Feinler) and her team managed `WHOIS` and created the idea of domain names to help organize requests. A domain name is a hierarchical sequence of strings separated with periods, with the last string (called the top-level domain) being the most important. SRI decided that top-level domains would be allocated based on the type of business, with `.com` for commercial entities, `.edu` for educational, `.gov` for government, etc.

In the hostname `www.cs.virginia.edu`,

- `edu` is the top-level domain
- `virginia.edu` is a subdomain of `edu`
- `cs.virginia.edu` is a subdomain of `virginia.edu`
- `www.cs.virginia.edu` is a subdomain of `www.virginia.edu`
- `edu`, `virginia`, `cs`, and `www` are called _labels_

As the Internet grew and a single file on a single server became unwieldy, various proposals were created for how to distribute the load. After many iterations, this has grown into the current Domain Name System (DNS).

### 3.3 DNS

DNS is a fairly complicated set of concepts, but the iconic and most important core is the address resolution mechanism.

DNS address resolution uses two basic ideas:

1. Requests start at a top-level DNS server, which replies with the DNS server of the top-level domain; that DNS server is then asked about the next part of the domain, and so on until the full domain is handled.
    
2. Replies are cached (stored locally) so that each query may be sent just once.
    

Generally when I ask my browser to visit `www.cs.virginia.edu`, it just contacts `128.143.67.11` without any DNS queries because it has it in its cache.

But if there was no cache, the request would do something like the following:

|Request|To|Reply|
|---|---|---|
|`.`|my ISP’s DNS server|a list of 13 known top-level-domain DNS servers; we randomly select `202.12.27.33`.|
|`edu.`|`202.12.27.33`|a list of 13 .edu-domain DNS servers; we randomly select `192.54.112.30`.|
|`virginia.edu.`|`192.54.112.30`|a list of 4 .virginia.edu-domain DNS servers; we randomly select `128.143.22.119`.|
|`cs.virginia.edu.`|`128.143.22.119`|a list of 2 cs.virginia.edu-domain DNS servers; we randomly select `128.143.136.15`.|
|`www.cs.virginia.edu.`|`128.143.136.15`|The IP address `128.143.67.11`|

There are many additional components to the DNS protocol, including digital signatures to validate that they are not spoofed, messages for registering new DNS entries and changing old ones, [etc.](https://en.wikipedia.org/wiki/List_of_DNS_record_types)

As DNS has grown in complexity, DNS servers have grown into ever more-complicated database engines. However, the core DNS lookup process has not changed since the first DNS specifications[3](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#fn3) were released in 1983.

## 4 DHCP

Some IP addresses are specified statically, procured from the set of all possible IP addresses by a single computer and used only by that computer. Others are assigned dynamically as needed, picking an IP address when a computer is placed on the Internet and removing it when it disconnects. The most common way of obtaining an IPv4 address is governed by the Dynamic Host Control Protocol, or DHCP.

The most common model for DHCP is as follows:

1. The computer sends
    
    1. its location on the connection (a MAC address)
    2. in a UDP packet from port 68 to port 67
    3. over IP from address 0.0.0.0 (no one) to address 255.255.255.255 (everyone)
    4. on the newly-connected connection, to everyone on that channel
    
    Most computers seeing this message ignore it.
    
2. The server owning the connection sends
    
    1. an offered IP address
    2. in a UDP packet from port 67 to port 68
    3. over IP from the server’s IP address to 255.255.255.255
    4. on the channel, directly to the computer[4](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#fn4)
3. The computer sends
    
    1. thanks, I’ll take that IP address
    2. in a UDP packet from port 68 to port 67
    3. over IP from 0.0.0.0 to the server’s IP address
    4. on the channel, directly to the server
4. The server sends
    
    1. great, it’s yours
    2. in a UDP packet from port 67 to port 68
    3. over IP from the server’s IP address to 255.255.255.255
    4. on the channel, directly to the computer

For IPv6, similar protocols exist. Since IPv6 has much larger addresses, often the server on the local network will only choose the first part of the IPv6 address and each host will select the rest of the address (in some way that ensures that there are no duplicate addresses).

## 5 Network address translation

Because IPv4 addresses are scarce, often organizations and households and even some ISPs (Internet Service Providers) will have IP addresses on their internal networks that are not valid elsewhere. Several ranges of IP addresses[5](https://www.cs.virginia.edu/~cr4bd/3130/F2024/readings/protocols.html#fn5) are reserved for this purpose. Probably the most well known are addresses starting with 192.168. or with 10..

When machines with these private addresses access the public Internet, these private addresses need to be translated to a publicly-accessible address. This process is called [network address translation](https://en.wikipedia.org/wiki/Network_address_translation) and results in several machines sharing a single public address. This type of configuration is, by far, the most common for home networks when using IPv4.

## 6 Selected special IP addresses

Some IP addresses have special uses, here is a partial list:

- Anything starting with `127.` (IPv4) and the IPv6 address `::1` always represent the current machine. (They won’t use the network.)
    
- Anything starting with `169.254.` (IPv4) or `fe80:` (IPv6) is a [_link-local_ address](https://en.wikipedia.org/wiki/Link-local_address), which is only valid on a local network and assigned automatically.
    
- Anything starting with `192.168.` or `10.` or `172.16.` are reserved from private networks, typically used with network address translation (described above).