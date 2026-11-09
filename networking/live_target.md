# Nmap – Live Host Discovery

## Introduction to Nmap

Nmap (Network Mapper) is an open-source network reconnaissance tool used to discover devices, identify open ports, detect running services, and gather information about network infrastructure.

It is widely used by penetration testers, security researchers, and network administrators because it automates tasks that would otherwise require manually checking individual systems.

Nmap can also use its Scripting Engine (NSE) to perform additional security checks, such as service enumeration, vulnerability detection, and configuration analysis.

### Why Do We Use Nmap?

During the reconnaissance phase of a penetration test, we need to understand the environment before investigating individual systems.

Two important questions guide this process:

- Which devices are currently reachable on the network?
- What services and ports are exposed by those devices?

Live host discovery addresses the first question. Once we identify reachable systems, we can investigate their exposed services through port scanning.

### Main Learning Areas

- Discovering active devices within a network.
- Understanding how ARP identifies systems on a local network.
- Using ICMP messages to check host availability.
- Performing TCP SYN and ACK-based host discovery.
- Using UDP probes to identify reachable systems.
- Working with IP ranges, subnets, and target lists.
- Understanding reverse DNS and hostname resolution.

---

## Understanding Network Segments and Subnets

Before performing host discovery, we need to understand how devices are organized within a network.

### Network Segment

A network segment is a group of devices connected through a shared network medium, such as an Ethernet switch or wireless access point.

Devices within the same segment can communicate directly at the link layer.

### Subnet

A subnet is a logical division of an IP network. It defines which IP addresses belong to the same network and which devices need a router to communicate with each other.

For example:

- `10.1.100.0/24` represents a subnet with approximately 254 usable host addresses.
- `10.1.0.0/16` represents a much larger subnet with approximately 65,534 usable host addresses.

The `/24` and `/16` values represent CIDR prefix lengths, which determine how many bits belong to the network portion of an IP address.

### Why Does This Matter for Nmap?

The location of your scanning machine determines which discovery techniques can reach the target.

Imagine two networks:

- Network A: `10.1.100.0/24`
- Network B: `10.1.200.0/24`

If your AttackBox is connected to Network A and wants to discover devices in Network B, traffic must pass through a router.

ARP requests cannot cross that router because ARP operates at Layer 2.

For remote subnets, we need other discovery techniques, such as ICMP, TCP, and UDP probes.

**Important distinction:** ARP discovers devices on the local network segment, while IP-based discovery methods can be routed to other networks if routing and firewall policies allow them.

---

## Protocols Used for Host Discovery

Nmap uses different network protocols to determine whether a target system is reachable.

Understanding their behaviour makes it easier to interpret scan results.

### ARP – Address Resolution Protocol

ARP is used to discover the MAC address associated with an IPv4 address on a local network.

When a device wants to communicate with another device on the same subnet, it needs the destination MAC address.

If that address is unknown, the device broadcasts an ARP request.

The request essentially asks:

"Which device owns this IP address? Please provide your MAC address."

The device with the matching IP responds with an ARP reply containing its MAC address.

**How Nmap uses it:**

- Sends ARP requests to target IP addresses.
- Waits for ARP replies.
- Treats responding devices as reachable.

ARP is particularly useful for local network discovery because it does not depend on ICMP echo requests being allowed.

### ICMP – Internet Control Message Protocol

ICMP is a network-layer protocol used for network diagnostics and error reporting.

It is commonly associated with the `ping` command.

For example, a normal ICMP echo exchange works like this:

- Type 8: Echo Request.
- Type 0: Echo Reply.

A device sends an Echo Request, and a reachable target may respond with an Echo Reply.

Nmap can also use other ICMP message types when ordinary ping requests are blocked.

### TCP – Transmission Control Protocol

TCP is a connection-oriented transport protocol.

It normally establishes communication through a three-way handshake:

- SYN: Initiates the connection.
- SYN/ACK: Acknowledges the request and indicates willingness to connect.
- ACK: Completes the handshake.

For host discovery, Nmap can send TCP packets to selected ports and interpret the responses.

An open port may respond with SYN/ACK, while a closed port typically responds with RST.

Both responses indicate that the target is reachable.

### UDP – User Datagram Protocol

UDP is a connectionless transport protocol. Unlike TCP, it does not establish a connection through a handshake.

When a UDP packet reaches an open port, the application may respond, but a response is not guaranteed.

When a UDP packet reaches a closed port, the target may return an ICMP Destination Unreachable message.

For a closed UDP port, the relevant response is:

- ICMP Type 3: Destination Unreachable.
- ICMP Code 3: Port Unreachable.

This error message can indirectly confirm that the target system is reachable.

---

## Specifying Targets in Nmap

Before starting a scan, we need to tell Nmap which systems or network ranges should be examined.

Nmap supports individual IP addresses, multiple targets, ranges, subnets, and input files.

### Individual Targets and Lists

To scan multiple systems, provide their addresses separated by spaces.

```bash
nmap 192.168.1.10 192.168.1.20 192.168.1.30
```

This instructs Nmap to examine three separate targets.

### IP Address Range

You can specify a range of addresses using a hyphen.

```bash
nmap 192.168.1.10-20
```

This covers addresses from `192.168.1.10` through `192.168.1.20`, inclusive.

### Multiple Ranges

Nmap also supports ranges in different octets.

```bash
nmap 192.168.1-2.1-5
```

This expands into two groups of addresses:

- `192.168.1.1` through `192.168.1.5`
- `192.168.2.1` through `192.168.2.5`

### Scanning a Subnet

CIDR notation allows us to specify an entire subnet.

```bash
nmap 192.168.1.0/24
```

This covers the 256 IPv4 addresses in that subnet.

### Reading Targets From a File

For larger assessments, maintaining a list of targets in a text file is more convenient.

```bash
nmap -iL targets.txt
```

The `-iL` option tells Nmap to read its targets from the specified file.

### Listing Targets Without Scanning Them

Sometimes we want to verify which addresses Nmap will consider before sending any probes.

```bash
nmap -sL 192.168.1.0/24
```

The `-sL` option performs a list scan. It displays the target addresses without performing a port scan or sending host-discovery probes.

Nmap may perform reverse DNS lookups while listing targets.

To prevent DNS lookups:

```bash
nmap -sL -n 192.168.1.0/24
```

---

## Nmap Default Host Discovery Behaviour

When you execute Nmap without specifying a particular discovery technique, it uses its default host-discovery methods.

The behaviour depends on whether the scanner has elevated privileges and whether the targets are on the local network.

### Privileged User Scanning a Local Network

When running with root or equivalent privileges, Nmap normally uses ARP requests to identify reachable systems on a directly connected Ethernet network.

### Privileged User Scanning a Remote Network

For targets outside the local network, Nmap normally uses a combination of:

- ICMP Echo Request.
- ICMP Timestamp Request.
- TCP SYN probe to port 443.
- TCP ACK probe to port 80.

### Unprivileged User Scanning a Remote Network

Without the privileges needed for raw packet operations, Nmap generally uses operating-system connection attempts to TCP ports 80 and 443.

This can involve completing the TCP handshake when a port is open.

### Host Discovery Only

By default, Nmap discovers hosts and then proceeds with port scanning on those considered online.

If we only want to identify reachable systems without checking their ports, we use:

```bash
nmap -sn 192.168.1.0/24
```

The `-sn` option disables port scanning and performs host discovery only.

**Remember:** `-sn` means discover hosts, not scan their ports.

---

## ARP Host Discovery

ARP discovery is one of the most useful techniques when examining a local network.

### How It Works

Suppose our AttackBox is connected to `192.168.1.0/24` and wants to identify active devices.

The process is:

- Nmap generates an ARP request for a target IP.
- The request is broadcast across the local network.
- Devices check whether the requested IP belongs to them.
- The matching device sends an ARP reply containing its MAC address.
- Nmap identifies the responding device as reachable.

The scanner does not need to know the destination MAC address beforehand because the request is broadcast.

### Command

```bash
sudo nmap -PR -sn 192.168.1.0/24
```

Options:

- `-PR`: Uses ARP requests for host discovery.
- `-sn`: Disables port scanning.
- `sudo`: Provides the privileges required for raw packet operations.

### Example

```text
Nmap scan report for 192.168.1.10
Host is up.

Nmap scan report for 192.168.1.25
Host is up.

Nmap done: 256 IP addresses (2 hosts up)
```

This indicates that two devices responded to the discovery process.

### Important Limitations

ARP is restricted to the local network segment.

If the target belongs to another subnet, the router will not forward the ARP request.

ARP discovery is useful during internal network enumeration, especially after obtaining access to a machine inside an organization's network.

A compromised internal host may provide a useful starting point for identifying other devices on its local segment.

### Viewing ARP Traffic

You can inspect ARP requests and replies using:

```bash
sudo tcpdump -i eth0 arp
```

Or use Wireshark with the display filter:

```text
arp
```

---

## ICMP Host Discovery

ICMP is another method for identifying reachable systems, particularly when targets belong to remote subnets.

### ICMP Echo Discovery

The familiar `ping` command uses ICMP Echo Request and Echo Reply messages.

Nmap can use the same approach to discover live systems.

### Packet Flow

- Nmap sends an ICMP Echo Request.
- The target receives the request.
- If permitted, the target responds with an ICMP Echo Reply.
- Nmap marks the host as reachable.

The relevant ICMP message types are:

- Type 8: Echo Request.
- Type 0: Echo Reply.

### Command

```bash
sudo nmap -PE -sn 192.168.1.0/24
```

Options:

- `-PE`: Uses ICMP Echo Requests.
- `-sn`: Performs host discovery without port scanning.

### Example

```text
Nmap scan report for 192.168.1.10
Host is up.

Nmap scan report for 192.168.1.50
Host is up.

Nmap done: 256 IP addresses (2 hosts up)
```

### Limitation

ICMP Echo Requests are frequently blocked by host-based and network firewalls.

For example, a Windows machine might be running normally while its firewall blocks incoming ICMP Echo Requests.

Therefore, no ping response does not automatically mean that the system is offline.

This is why alternative discovery techniques are important.

---

## ICMP Timestamp Discovery

Blocking ordinary ping requests does not necessarily mean that every ICMP message type is blocked.

Nmap can also use ICMP Timestamp Requests.

### How It Works

- Nmap sends an ICMP Timestamp Request.
- The target may respond with an ICMP Timestamp Reply.
- Nmap interprets the response as evidence that the host is reachable.

Relevant ICMP types:

- Type 13: Timestamp Request.
- Type 14: Timestamp Reply.

### Command

```bash
sudo nmap -PP -sn 192.168.1.0/24
```

Options:

- `-PP`: Uses ICMP Timestamp Requests.
- `-sn`: Performs host discovery only.

### Why Is It Useful?

A network might block ordinary Echo Requests while allowing other ICMP messages.

In that situation, timestamp discovery may identify systems that a normal ping scan misses.

However, modern systems and firewalls frequently block or ignore timestamp requests, so this technique is not universally reliable.

---

## ICMP Address Mask Discovery

Another less commonly used technique is ICMP Address Mask discovery.

Historically, devices could request their subnet mask through ICMP Address Mask messages.

Although DHCP largely replaced this functionality, Nmap still supports these requests.

### How It Works

- Nmap sends an ICMP Address Mask Request.
- A supporting target may return an Address Mask Reply.
- The reply indicates that the target is reachable.

Relevant ICMP types:

- Type 17: Address Mask Request.
- Type 18: Address Mask Reply.

### Command

```bash
sudo nmap -PM -sn 192.168.1.0/24
```

Options:

- `-PM`: Uses ICMP Address Mask Requests.
- `-sn`: Disables port scanning.

### Practical Limitation

Most modern operating systems do not respond to these requests.

Therefore, it is useful to understand this technique, but it should not be your primary host-discovery method.

A failed scan may simply indicate that the target or an intermediate firewall is blocking the request.

---

## TCP SYN Ping

TCP SYN Ping uses the SYN flag to check whether a target responds to a connection request.

Instead of performing a complete port scan, Nmap sends a SYN packet to a selected TCP port and observes the response.

### How It Works

Consider a target with TCP port 80.

If the port is open:

- Nmap sends SYN.
- The target responds with SYN/ACK.
- Nmap identifies the host as reachable.

If the port is closed:

- Nmap sends SYN.
- The target responds with RST.
- Nmap still identifies the host as reachable.

The important point is that we are interested in the response, not whether the port is open.

### Command

```bash
sudo nmap -PS80 -sn 192.168.1.0/24
```

Options:

- `-PS`: Uses TCP SYN probes.
- `80`: Specifies the destination TCP port.
- `-sn`: Performs host discovery only.

### Specifying Multiple Ports

```bash
sudo nmap -PS21,22,80,443 -sn 192.168.1.0/24
```

You can also specify a range:

```bash
sudo nmap -PS21-25 -sn 192.168.1.0/24
```

Or a custom list:

```bash
sudo nmap -PS80,443,8080 -sn 192.168.1.0/24
```

### Privileged vs. Unprivileged Behaviour

There is an important difference depending on the permissions available to Nmap.

**Privileged execution:**

Nmap can construct and transmit raw TCP SYN packets.

When an open port responds with SYN/ACK, the scanner's operating system typically sends an RST to terminate the incomplete connection.

The three-way handshake is not completed.

**Unprivileged execution:**

Nmap cannot use the same raw-packet approach and generally relies on the operating system's `connect()` functionality.

If the port is open, the operating system completes the TCP handshake.

This means an unprivileged scan can establish a full TCP connection.

**Key takeaway:** Both approaches can discover a reachable host, but privileged execution allows Nmap to perform the SYN probe without completing the connection.

---

## TCP ACK Ping

TCP ACK Ping uses an ACK packet instead of a SYN packet.

Normally, the ACK flag acknowledges data received during an existing TCP connection.

However, Nmap can send an ACK packet without first establishing a connection.

### How It Works

- Nmap sends a TCP packet with the ACK flag set.
- The target receives an ACK that does not belong to an existing connection.
- The target normally responds with an RST packet.
- Nmap interprets the response as evidence that the host is reachable.

### Command

```bash
sudo nmap -PA80 -sn 192.168.1.0/24
```

Options:

- `-PA`: Uses TCP ACK probes.
- `80`: Specifies the destination TCP port.
- `-sn`: Disables port scanning.

### Custom Ports

```bash
sudo nmap -PA21,22,80,443 -sn 192.168.1.0/24
```

### Why Use ACK Ping?

ACK probes can be useful when examining networks where different firewall rules affect SYN and ACK traffic.

The response can help determine whether a target is reachable through a particular network path.

However, firewall behaviour varies, and a missing response cannot conclusively establish that a system is offline.

---

## UDP Ping

UDP Ping is another technique for discovering reachable systems, particularly when TCP-based discovery is unsuccessful.

Unlike TCP, UDP does not require a three-way handshake.

### How It Works

Nmap sends a UDP datagram to a selected destination port.

The response depends on the state of that port.

**If the port is closed:**

- Nmap sends a UDP packet.
- The target determines that no application is listening on the destination port.
- The target may return ICMP Destination Unreachable, Port Unreachable.
- Nmap identifies the target as reachable.

**If the port is open:**

- Nmap sends a UDP packet.
- The application may respond.
- Alternatively, it may silently ignore the packet.
- No response does not automatically mean the host is offline.

### Command

```bash
sudo nmap -PU53 -sn 192.168.1.0/24
```

Options:

- `-PU`: Uses UDP probes.
- `53`: Specifies the destination UDP port.
- `-sn`: Performs host discovery only.

### Multiple Ports

```bash
sudo nmap -PU53,161,162 -sn 192.168.1.0/24
```

### Important Observation

An ICMP Port Unreachable response is particularly useful because it tells us that the target received and processed the UDP probe, even though the destination port was closed.

This is an example of using an error response as evidence of host availability.

---

## Masscan

Masscan is another network scanning tool designed for extremely fast scanning across large IP ranges.

It supports scanning specific ports, lists, and ranges.

### Examples

Scan port 443:

```bash
masscan 192.168.1.0/24 -p443
```

Scan HTTP and HTTPS:

```bash
masscan 192.168.1.0/24 -p80,443
```

Scan a port range:

```bash
masscan 192.168.1.0/24 -p22-25
```

### Nmap vs. Masscan

Nmap is commonly used when detailed reconnaissance and service enumeration are required.

Masscan is optimized for high-speed port discovery across large address spaces.

Its aggressive packet transmission rate can generate significant network traffic, so scanning rates should be controlled during authorized assessments.

---

## Reverse DNS Lookup

DNS normally translates a hostname into an IP address.

Reverse DNS (rDNS) performs the opposite operation by looking up a hostname associated with an IP address.

For example:

Normal DNS:

```text
server.example.com → 192.168.1.10
```

Reverse DNS:

```text
192.168.1.10 → server.example.com
```

### Nmap Options

**Default behaviour**

Nmap normally attempts reverse DNS lookups for hosts it discovers.

**Force reverse DNS lookups:**

```bash
nmap -R 192.168.1.0/24
```

The `-R` option requests reverse DNS resolution for every target address, including addresses that do not respond to host discovery.

**Disable DNS resolution:**

```bash
nmap -n 192.168.1.0/24
```

The `-n` option prevents DNS resolution.

**Use a specific DNS server:**

```bash
nmap --dns-servers 8.8.8.8 192.168.1.0/24
```

### Why Is Reverse DNS Useful?

Hostnames can reveal information about the infrastructure.

For example:

- `dc01.company.local` may indicate a domain controller.
- `mail.company.local` may identify a mail server.
- `backup.company.local` may indicate a backup system.

These names can provide useful context during reconnaissance.

However, reverse DNS records are not always configured, accurate, or up to date.

---

## Host Discovery Command Reference

The following commands summarize the discovery techniques covered in this section.

**ARP Discovery**

```bash
sudo nmap -PR -sn 192.168.1.0/24
```

Uses ARP requests to identify reachable devices on the local network.

**ICMP Echo Discovery**

```bash
sudo nmap -PE -sn 192.168.1.0/24
```

Uses ICMP Echo Requests.

**ICMP Timestamp Discovery**

```bash
sudo nmap -PP -sn 192.168.1.0/24
```

Uses ICMP Timestamp Requests.

**ICMP Address Mask Discovery**

```bash
sudo nmap -PM -sn 192.168.1.0/24
```

Uses ICMP Address Mask Requests.

**TCP SYN Ping**

```bash
sudo nmap -PS22,80,443 -sn 192.168.1.0/24
```

Uses TCP SYN packets.

**TCP ACK Ping**

```bash
sudo nmap -PA22,80,443 -sn 192.168.1.0/24
```

Uses TCP ACK packets.

**UDP Ping**

```bash
sudo nmap -PU53,161,162 -sn 192.168.1.0/24
```

Uses UDP datagrams.

**Other Useful Options**

```bash
nmap -sn TARGETS
```

Performs host discovery without port scanning.

```bash
nmap -n TARGETS
```

Disables DNS resolution.

```bash
nmap -R TARGETS
```

Forces reverse DNS lookups.

```bash
nmap -sL TARGETS
```

Lists target addresses without performing a host-discovery scan.

```bash
nmap -iL targets.txt
```

Reads target addresses from a file.

---

## Practical Reconnaissance Workflow

Let's combine these techniques into a realistic internal network assessment.

Assume a penetration tester has been authorized to assess the subnet:

`192.168.1.0/24`

The objective is to identify reachable systems before investigating their exposed services.

### Initial Target Verification

First, inspect the addresses that will be included in the assessment.

```bash
nmap -sL -n 192.168.1.0/24
```

This avoids accidentally scanning an incorrect target range.

### Local Network Discovery

If the scanner is connected directly to the target subnet, begin with ARP discovery.

```bash
sudo nmap -PR -sn 192.168.1.0/24
```

Record the devices that respond.

### Remote Network Discovery

If the target network is routed, use IP-based discovery techniques instead.

```bash
sudo nmap -PE -sn 192.168.1.0/24
```

If ICMP Echo Requests are blocked, test alternative probes:

```bash
sudo nmap -PS80,443 -sn 192.168.1.0/24
```

```bash
sudo nmap -PA80,443 -sn 192.168.1.0/24
```

```bash
sudo nmap -PU53,161 -sn 192.168.1.0/24
```

Compare the results to understand which systems respond to different protocols.

### Investigating Discovered Systems

After identifying reachable hosts, the next stage is port scanning.

For example:

```bash
sudo nmap -sS -p- 192.168.1.10
```

This performs a TCP SYN scan across all 65,535 TCP ports on the selected host.

The discovered ports can then be investigated further through service detection and enumeration.

### Defensive Perspective

From a blue-team perspective, host discovery activity can reveal reconnaissance against internal infrastructure.

Security analysts can investigate:

- Repeated ARP requests across a subnet.
- Large numbers of ICMP Echo Requests.
- Sequential TCP SYN or ACK probes.
- UDP probes sent to multiple destination ports.
- Unusual scanning activity originating from internal workstations.

Useful investigation tools include:

- Wireshark for packet inspection.
- tcpdump for command-line traffic capture.
- Suricata for network intrusion detection.
- Zeek for network traffic analysis.
- SIEM platforms for correlating network and endpoint telemetry.

A single discovery packet is not necessarily malicious. The pattern, source, frequency, target range, and surrounding activity help determine whether the behaviour is legitimate or suspicious.

---

## Important Concepts to Remember

- Host discovery identifies reachable systems before deeper enumeration.
- ARP is restricted to the local network segment and cannot cross routers.
- ICMP Echo uses Type 8 requests and Type 0 replies.
- ICMP Timestamp uses Type 13 requests and Type 14 replies.
- ICMP Address Mask uses Type 17 requests and Type 18 replies.
- TCP SYN Ping can receive either SYN/ACK or RST as evidence of reachability.
- TCP ACK Ping commonly receives RST responses.
- UDP Ping can identify a reachable system through an ICMP Port Unreachable response.
- A missing response does not automatically mean that a host is offline.
- Firewalls can block particular probes, which is why multiple discovery methods are useful.
- `-sn` performs host discovery without proceeding to port scanning.
- `-n` disables DNS resolution, whereas `-R` forces reverse DNS lookups.

## Memory Summary

Think of Nmap host discovery as trying different ways to knock on the doors of devices across a network.

- **ARP:** Asking a neighbour on the same local network for their MAC address.
- **ICMP Echo:** Ringing the doorbell and waiting for someone to answer.
- **ICMP Timestamp:** Knocking using a different message when the usual doorbell is blocked.
- **ICMP Address Mask:** Asking a device for its old-style subnet mask information.
- **TCP SYN:** Trying to start a TCP connection and checking whether the target responds.
- **TCP ACK:** Sending an unexpected acknowledgement and observing the response.
- **UDP:** Sending a datagram and using an ICMP error message as evidence that the device is reachable.

The central idea is simple: **Different discovery techniques use different protocols, but they all help us determine which systems are reachable before moving on to deeper network enumeration.**