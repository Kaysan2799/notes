<span style="color:#00FC00">Abdul Malik</span>
### <span style="color:red">Link to website: </span> https://tryhackme.com/room/nmap01

## Introduction

When we want to target a network, we want to find an efficient tool to help us handle repetitive tasks and answer the following questions:

	1. Which systems are up?
	2. What services are running on these systems?

This room is the first of four in this Nmap series. These four rooms are also part of the Network Security module.

	1. [Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)
	2. [Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
	3. [Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
	4. [Nmap Post Port Scans](https://tryhackme.com/room/nmap04)

We present the different approaches that Nmap uses to discover live hosts. In particular, we cover:

	1. ARP scan: This scan uses ARP requests to discover live hosts
	2. ICMP scan: This scan uses ICMP requests to identify live hosts
	3. TCP/UDP ping scan: This scan sends packets to TCP ports and UDP ports to determine live hosts.

We also introduce two scanners, <span style="color:#00FC00">arp-scan</span> and <span style="color:#00FC00">masscan</span>, and explain how they overlap with part of Nmap’s host discovery.

![[Pasted image 20231226124245.png]]

## Subnetworks:
A _network segment_ is a group of computers connected using a shared medium. For instance, the medium can be the Ethernet switch or WiFi access point. In an IP network, a _subnetwork_ is usually the equivalent of one or more network segments connected together and configured to use the same router. The network segment refers to a physical connection, while a subnetwork refers to a logical connection.
![[Pasted image 20231226124518.png]]

The figure above shows two types of subnets:

- Subnets with <span style="color:#00FC00">/16</span>, which means that the subnet mask can be written as <span style="color:#00FC00">255.255.0.0</span>. This subnet can have around 65 thousand hosts.
- Subnets with <span style="color:#00FC00">/24</span>, which indicates that the subnet mask can be expressed as <span style="color:#00FC00">255.255.255.0</span>. This subnet can have around 250 hosts.

## Enumerating Target
We mentioned the different _techniques_ we can use for scanning in Task 1. Before we explain each in detail and put it into use against a live target, we need to specify the targets we want to scan. Generally speaking, you can provide a list, a range, or a subnet. Examples of target specification are:

- list: <span style="color:#00FC00">MACHINE_IP scanme.nmap.org example.com</span> will scan 3 IP addresses.
- range: `10.11.12.15-20` will scan 6 IP addresses: `10.11.12.15`, `10.11.12.16`,… and `10.11.12.20`.
- subnet: `MACHINE_IP/30` will scan 4 IP addresses.

You can also provide a file as input for your list of targets, 
```sh
nmap -iL list_of_hosts.txt
```

If you want to check the list of hosts that Nmap will scan, you can use 
```sh
nmap -sL TARGETS
```
This option will give you a detailed list of the hosts that Nmap will scan without scanning them; however, Nmap will attempt a reverse-DNS resolution on all the targets to obtain their names. Names might reveal various information to the pentester. (If you don’t want Nmap to the DNS server, you can add `-n`.)

## Discovering Live Hosts
Starting from bottom to top, we can use:

	- ARP from Link Layer
	- ICMP from Network Layer
	- TCP from Transport Layer
	- UDP from Transport Layer

![[Pasted image 20231226125132.png]]
Before we discuss how scanners can use each in detail, we will briefly review these four protocols. 
### ARP has one purpose:
sending a frame to the broadcast address on the network segment and asking the computer with a specific IP address to respond by providing its MAC (hardware) address.

### ICMP 
has [many types](https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml). ICMP ping uses `Type 8 (Echo) and Type 0 (Echo Reply)`.
If you want to ping a system on the same subnet, an ARP query should precede the ICMP Echo.

### Although TCP and UDP are transport layers, 
for network scanning purposes, a scanner can send a specially-crafted packet to common TCP or UDP ports to check whether the target will respond. This method is efficient, especially when ICMP Echo is blocked.

## Nmap host discovery using `ARP`:
How would you know which hosts are up and running? It is essential to avoid wasting our time port-scanning an offline host or an IP address not in use. There are various ways to discover online hosts. When no host discovery options are provided, Nmap follows the following approaches to discover live hosts:

1. When a _privileged_ user tries to scan targets on a local network (Ethernet), Nmap uses _ARP requests_. A privileged user is <span style="color:#00FC00">root</span> or a user who belongs to <span style="color:#00FC00">sudoers</span> and can run <span style="color:#00FC00">sudo</span>.

2. When a _privileged_ user tries to scan targets outside the local network, Nmap uses ICMP echo requests, TCP ACK (Acknowledge) to port 80, TCP SYN (Synchronize) to port 443, and ICMP timestamp request.
3. When an _unprivileged_ user tries to scan targets outside the local network, Nmap resorts to a TCP 3-way handshake by sending SYN packets to ports 80 and 443.

Nmap, by default, uses a ping scan to find live hosts, then proceeds to scan live hosts only. If you want to use Nmap to discover online hosts without port-scanning the live systems, you can issue 
```bash
nmap -sn TARGETS
```
. Let’s dig deeper to gain a solid understanding of the different techniques used.

ARP scan is possible only if you are on the same subnet as the target systems. On an Ethernet (802.3) and WiFi (802.11), you need to know the MAC address of any system before you can communicate with it. The MAC address is necessary for the link-layer header; the header contains the source MAC address and the destination MAC address among other fields. To get the MAC address, the OS sends an ARP query. A host that replies to ARP queries is up. The ARP query only works if the target is on the same subnet as yourself, i.e., on the same Ethernet/WiFi. You should expect to see many ARP queries generated during a Nmap scan of a local network. If you want Nmap only to perform an ARP scan without port-scanning, you can use 
```bash
nmap -PR -sn TARGETS
```
where `-PR` indicates that you only want an ARP scan. The following example shows Nmap using ARP for host discovery without any port scanning. We run 
![[Pasted image 20231226131916.png]]
```bash
nmap -PR -sn MACHINE_IP/24
```
to discover all the live systems on the same subnet as our target machine.


If we look at the packets generated using a tool such as tcpdump or Wireshark, we will see network traffic similar to the figure below. In the figure below
![[Pasted image 20231226132023.png]]
Talking about ARP scans, we should mention a scanner built around ARP queries: `arp-scan`; it provides many options to customize your scan. Visit the [arp-scan wiki](http://www.royhills.co.uk/wiki/index.php/Main_Page) for detailed information. One popular choice is ```
```bash
arp-scan --localnet
# or simply 
arp-scan -l
```
This command will send ARP queries to all valid IP addresses on your local networks. Moreover, if your system has more than one interface and you are interested in discovering the live hosts on one of them, you can specify the interface using `-I`. For instance, ```
```bash
sudo arp-scan -I eth0 -l
```
will send ARP queries for all valid IP addresses on the `eth0` interface.

Note that `arp-scan` is not installed on the AttackBox; however, it can be installed using ```

```bash
apt install arp-scan
```
In the example below, we scanned the subnet of the AttackBox using 
```bash
arp-scan ATTACKBOX_IP/24
```
Since we ran this scan at a time frame close to the previous one 
```bash
nmap -PR -sn ATTACKBOX_IP/24
```
we obtained the same three live targets.
>[!Tip] ### Terminal
>```
10.10.210.75 02:83:75:3a:f2:89 (Unknown) 
10.10.210.100 02:63:d0:1b:2d:cd (Unknown) 
10.10.210.165 02:59:79:4f:17:b7 (Unknown)
4 packets received by filter, 0 packets dropped by kernel
>```


Similarly, the command <span style="color:#00FC00">arp-scan</span> will generate many ARP queries that we can see using tcpdump, Wireshark, or a similar tool. We can notice that the packet capture for <span style="color:#00FC00">arp-scan , nmap -PR -sn </span>yield similar traffic patterns. Below is the Wireshark output.

![[Pasted image 20231226132605.png]]

## Nmap Host Discovery Using ICMP
We can ping every IP address on a target network and see who would respond to our `ping` (ICMP Type 8/Echo) requests with a ping reply (ICMP Type 0). Simple, isn’t it? Although this would be the most straightforward approach, it is not always reliable. Many firewalls block ICMP echo; new versions of MS Windows are configured with a host firewall that blocks ICMP echo requests by default. Remember that an ARP query will precede the ICMP request if your target is on the same subnet.

To use ICMP echo request to discover live hosts, add the option <span style="color:#00FC00">-PE</span>. (Remember to add <span style="color:#00FC00">-sn</span> if you don’t want to follow that with a port scan.) As shown in the following figure, an ICMP echo scan works by sending an ICMP echo request and expects the target to reply with an ICMP echo reply if it is online.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/25fb5fd5d2009cf69d7aae40e8fde2ec.png)

In the example below, we scanned the target’s subnet using 
```bash
nmap -PE -sn MACHINE_IP/24
```
This scan will send ICMP echo packets to every IP address on the subnet. Again, we expect live hosts to reply; however, it is wise to remember that many firewalls block ICMP. The output below shows the result of scanning the virtual machine’s class C subnet using 
```sh
sudo nmap -PE -sn MACHINE_IP/24
```
from the AttackBox.
>[!Tip] ### Terminal
>```python
pentester@AbdulMalik$ sudo nmap -PE -sn 10.10.68.220/24 
```
Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 10:16 BST 
Nmap scan report for ip-10-10-68-50.eu-west-1.compute.internal (10.10.68.50) 
Host is up (0.00017s latency). 
MAC Address: 02:95:36:71:5B:87 (Unknown) 
Nmap scan report for ip-10-10-68-52.eu-west-1.compute.internal (10.10.68.52) 
Host is up (0.00017s latency). 
MAC Address: 02:48:E8:BF:78:E7 (Unknown) 
Nmap scan report for ip-10-10-68-77.eu-west-1.compute.internal (10.10.68.77) 
Host is up (-0.100s latency). 
MAC Address: 02:0F:0A:1D:76:35 (Unknown) 
Nmap scan report for ip-10-10-68-110.eu-west-1.compute.internal (10.10.68.110) 
Host is up (-0.10s latency). 
MAC Address: 02:6B:50:E9:C2:91 (Unknown) 
Nmap scan report for ip-10-10-68-140.eu-west-1.compute.internal (10.10.68.140) 
Host is up (0.00021s latency). 
MAC Address: 02:58:59:63:0B:6B (Unknown) 
Nmap scan report for ip-10-10-68-142.eu-west-1.compute.internal (10.10.68.142) 
Host is up (0.00016s latency). 
MAC Address: 02:C6:41:51:0A:0F (Unknown) 
Nmap scan report for ip-10-10-68-220.eu-west-1.compute.internal (10.10.68.220) 
Host is up (0.00026s latency). 
MAC Address: 02:25:3F:DB:EE:0B (Unknown) 
Nmap scan report for ip-10-10-68-222.eu-west-1.compute.internal (10.10.68.222) 
Host is up (0.00025s latency). 
MAC Address: 02:28:B1:2E:B0:1B (Unknown) 
Nmap done: 256 IP addresses (8 hosts up) scanned in 2.11 seconds
```
>```

The scan output shows that eight hosts are up; moreover, it shows their <span style="color:#00FC00">MAC addresses</span>. Generally speaking, we don’t expect to learn the MAC addresses of the targets unless they are on the same subnet as our system. The output above indicates that Nmap didn’t need to send ICMP packets as it confirmed that these hosts are up based on the ARP responses it received.

`We will not see their MAC address it hosts are in different network.`

Because ICMP echo requests tend to be blocked, you might also consider ICMP Timestamp or ICMP Address Mask requests to tell if a system is online. Nmap uses timestamp request (ICMP Type 13) and checks whether it will get a Timestamp reply (ICMP Type 14). Adding the <span style="color:#00FC00">-PP</span> option tells Nmap to use ICMP timestamp requests. As shown in the figure below, you expect live hosts to reply.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/06443faaa41a349ff46732d60e2e3bcd.png)

In the following example, we run 
###  ICMP Timestamp
```sh
nmap -PP -sn MACHINE_IP/24
```
to discover the online computers on the target machine subnet.

 Nmap uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18). This scan can be enabled with the option <span style="color:#00FC00">-PM</span>. As shown in the figure below, live hosts are expected to reply to ICMP address mask requests.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/14c31c66e002e2f50b0f8525c8d8e456.png)

In an attempt to discover live hosts using ICMP address mask queries, we run the command 
```sh
nmap -PM -sn MACHINE_IP/24
```
Although, based on earlier scans, we know that at least eight hosts are up, this scan returned none. The reason is that the target system or a firewall on the route is blocking this type of ICMP packet. Therefore, it is essential to learn multiple approaches to achieve the same result. If one type of packet is being blocked, we can always choose another to discover the target network and services.

>[!Tip] Terminal
>```sh
pentester@AbdulMalik$ sudo nmap -PM -sn 10.10.68.220/24 
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 12:13 EEST 
Nmap done: 256 IP addresses (0 hosts up) scanned in 52.17 seconds
>```

Although we didn’t get any reply and could not figure out which hosts are online, it is essential to note that this scan sent ICMP address mask requests to every valid IP address and waited for a reply. Each ICMP request was sent twice.

## Nmap Host Discovery Using TCP and UDP

### **TCP SYN Ping**

We can send a packet with the SYN (Synchronize) flag set to a TCP port, 80 by default, and wait for a response. An open port should reply with a SYN/ACK (Acknowledge); a closed port would result in an RST (Reset). In this case, we only check whether we will get any response to infer whether the host is up. The specific state of the port is not significant here. The figure below is a reminder of how a TCP 3-way handshake usually works.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/23e7f481f78de8d3e89ef845b747002d.png)

If you want Nmap to use TCP SYN ping, you can do so via the option <span style="color:#00FC00">-PS</span> followed by the port number, range, list, or a combination of them. For example, <span style="color:#00FC00">-PS21</span> will target port 21, while <span style="color:#00FC00">-PS21-25</span> will target ports 21, 22, 23, 24, and 25. Finally <span style="color:#00FC00">-PS80,443,8080</span> will target the three ports 80, 443, and 8080.

Privileged users (root and sudoers) can send TCP SYN packets and don’t need to complete the TCP 3-way handshake even if the port is open, as shown in the figure below. Unprivileged users have no choice but to complete the 3-way handshake if the port is open.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/168d48701c5f872cf1930e08b32bcd6f.png)

We will run 
>[!TIP] ### TCP SYN Ping
>```sh
nmap -PS -sn MACHINE_IP/24
>```

to scan the target VM subnet. As we can see in the output below, we were able to discover five hosts.

>[!Tip] Terminal
>```
>pentester@AbdulMalik$ sudo nmap -PS -sn 10.10.68.220/24 
>Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:45 EEST 
>Nmap scan report for 10.10.68.52 
>Host is up (0.10s latency). 
>Nmap scan report for 10.10.68.121 
>Host is up (0.16s latency). 
>Nmap scan report for 10.10.68.125 
>Host is up (0.089s latency). 
>Nmap scan report for 10.10.68.134 
>Host is up (0.13s latency). 
>Nmap scan report for 10.10.68.220 
>Host is up (0.11s latency). 
>Nmap done: 256 IP addresses (5 hosts up) scanned in 17.38 seconds
>```

>[!FAQ] Wireshark
>Let’s take a closer look at what happened behind the scenes by looking at the network traffic on Wireshark in the figure below. Technically speaking, since we didn’t specify any TCP ports to use in the TCP ping scan, Nmap used common ports; in this case, it is TCP port 80. Any service listening on port 80 is expected to reply, indirectly indicating that the host is online.
![[Pasted image 20231226135900.png]]

### **TCP ACK Ping**

As you have guessed, this sends a packet with an ACK flag set. You must be running Nmap as a privileged user to be able to accomplish this. If you try it as an unprivileged user, Nmap will attempt a 3-way handshake.

By default, port 80 is used. The syntax is similar to TCP SYN ping. <span style="color:#00FC00">-PA</span> should be followed by a port number, range, list, or a combination of them. For example, consider <span style="color:#00FC00">-PA21, -PA21-25</span> and <span style="color:#00FC00">-PA80,443,8080</span>. If no port is specified, port 80 will be used.

The following figure shows that any TCP packet with an ACK flag should get a TCP packet back with an RST flag set. The target responds with the RST flag set because the TCP packet with the ACK flag is not part of any ongoing connection. The expected response is used to detect if the target host is up.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/db5ab44a8c700c4ab0603e85e456040d.png)

In this example, we run 

>[!TIP] ### TCP ACK Ping
>```sh
sudo nmap -PA -sn MACHINE_IP/24
>```

to discover the online hosts on the target’s subnet. We can see that the TCP ACK ping scan detected five hosts as up.

>[!tip] Terminal
>```
>pentester@TryHackMe$ sudo nmap -PA -sn 10.10.68.220/24 
>Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:46 EEST 
>Nmap scan report for 10.10.68.52 
>Host is up (0.11s latency). 
>Nmap scan report for 10.10.68.121 
>Host is up (0.12s latency). 
>Nmap scan report for 10.10.68.125 
>Host is up (0.10s latency). 
>Nmap scan report for 10.10.68.134 Host is up (0.10s latency). 
>Nmap scan report for 10.10.68.220 
>Host is up (0.10s latency). 
>Nmap done: 256 IP addresses (5 hosts up) scanned in 29.89 seconds

>[!faq] Wireshark
>If we peek at the network traffic as shown in the figure below, we will discover many packets with the ACK flag set and sent to port 80 of the target systems. Nmap sends each packet twice. The systems that don’t respond are offline or inaccessible.
>![[Pasted image 20231226140227.png]]

### **UDP Ping**
Finally, we can use UDP to discover if the host is online. Contrary to TCP SYN ping, sending a UDP packet to an open port is not expected to lead to any reply. However, if we send a UDP packet to a closed UDP port, we expect to get an ICMP port unreachable packet; this indicates that the target system is up and available.

In the following figure, we see a UDP packet sent to an open UDP port and not triggering any response. However, sending a UDP packet to any closed UDP port can trigger a response indirectly indicating that the target is online.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/1b827ef60c39619e281c4ca51a6d57b6.png)

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/c8b2d403667487322058619e561186d2.png)

The syntax to specify the ports is similar to that of TCP SYN ping and TCP ACK ping; Nmap uses <span style="color:#00FC00">-PU</span> for UDP ping. In the following example, we use a UDP scan, and we discover five live hosts.
>[!tip] ### UDP Ping
>```
>sudo nmap -PU -sn 10.10.68.220/24

>[!tip] terminal
>```
>pentester@TryHackMe$ sudo nmap -PU -sn 10.10.68.220/24 
>Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:45 EEST 
>Nmap scan report for 10.10.68.52 
>Host is up (0.10s latency). 
>Nmap scan report for 10.10.68.121 Host is up (0.10s latency). 
>Nmap scan report for 10.10.68.125 H
>ost is up (0.14s latency). 
>Nmap scan report for 10.10.68.134 
>Host is up (0.096s latency). Nmap scan report for 10.10.68.220 H
>ost is up (0.11s latency). 
>Nmap done: 256 IP addresses (5 hosts up) scanned in 9.20 seconds

>[!tip] Wireshark
>Let’s inspect the UDP packets generated. In the following Wireshark screenshot, we notice Nmap sending UDP packets to UDP ports that are most likely closed. The image below shows that Nmap uses an uncommon UDP port to trigger an ICMP destination unreachable (port unreachable) error.
>![[Pasted image 20231226140656.png]]

### **Masscan**

On a side note, Masscan uses a similar approach to discover the available systems. However, to finish its network scan quickly, Masscan is quite aggressive with the rate of packets it generates. The syntax is quite similar: `-p` can be followed by a port number, list, or range. Consider the following :
>[!faq] Example
>- `masscan MACHINE_IP/24 -p443`
>- `masscan MACHINE_IP/24 -p80,443`
>- `masscan MACHINE_IP/24 -p22-25`
>- `masscan MACHINE_IP/24 ‐‐top-ports 100`

Masscan can be installed using 
```sh
apt install masscan
```

## Using Reverse-DNS Lookup
Nmap’s default behaviour is to use reverse-DNS online hosts. Because the hostnames can reveal a lot, this can be a helpful step. However, if you don’t want to send such DNS queries, you use <span style="color:#00FC00">-n</span> to skip this step.

>[!tip] 
>By default, Nmap will look up online hosts; however, you can use the option <span style="color:#00FC00">-R</span> to query the DNS server even for offline hosts. If you want to use a specific DNS server, you can add the <span style="color:#00FC00">--dns-servers DNS_SERVER</span> option.

## Summary

You have learned how ARP, ICMP, TCP, and UDP can detect live hosts by completing this room. Any response from a host is an indication that it is online. Below is a quick summary of the command-line options for Nmap that we have covered.

|<span style="color:#00FC00">Scan Type</span>|<span style="color:#00FC00">Example Command</span>|
|---|---|
|ARP Scan|`sudo nmap -PR -sn MACHINE_IP/24`|
|ICMP Echo Scan|`sudo nmap -PE -sn MACHINE_IP/24`|
|ICMP Timestamp Scan|`sudo nmap -PP -sn MACHINE_IP/24`|
|ICMP Address Mask Scan|`sudo nmap -PM -sn MACHINE_IP/24`|
|TCP SYN Ping Scan|`sudo nmap -PS22,80,443 -sn MACHINE_IP/30`|
|TCP ACK Ping Scan|`sudo nmap -PA22,80,443 -sn MACHINE_IP/30`|
|UDP Ping Scan|`sudo nmap -PU53,161,162 -sn MACHINE_IP/30`|

Remember to add <span style="color:#00FC00">-sn</span> if you are only interested in host discovery without port-scanning. Omitting <span style="color:#00FC00">-sn</span> will let Nmap default to port-scanning the live hosts.

|<span style="color:#00FC00">Option</span>|<span style="color:#00FC00">Purpose</span>|
|---|---|
|`-n`|no DNS lookup|
|`-R`|reverse-DNS lookup for all hosts|
|`-sn`|host discovery only|

-------------------------------------------------------------------------------