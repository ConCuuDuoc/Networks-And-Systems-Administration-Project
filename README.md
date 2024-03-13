```
VIETNAM NATIONAL UNIVERSITY HOCHIMINH CITY
```
## UNIVERSITY OF INFORMATION TECHNOLOGY

## FACULTY OF COMPUTER NETWORKS AND

## COMMUNICATIONS

DOAN HAI DANG **–** TRAN THI MY HUYEN **–** PHAN THI HONG NHUNG

# NETWORKS AND SYSTEMS

# ADMINISTRATION PROJECT

# NAT + DHCP ON LINUX SERVER

# FILE SERVER: NFS, SMB, FTP ON LINUX

```
CLASS: NT132.O11.ATCL
```


```
NATIONAL UNIVERSITY HOCHIMINH CITY
```
## UNIVERSITY OF INFORMATION TECHNOLOGY

## FACULTY OF COMPUTER NETWORKS AND

## COMMUNICATIONS

```
DOAN HAI DANG – 21520679
TRAN THI MY HUYEN – 21520269
PHAN THI HONG NHUNG - 21521250
```
# NETWORKS AND SYSTEMS

# ADMINISTRATION PROJECT

# NAT + DHCP ON LINUX SERVER

# FILE SERVER: NFS, SMB, FTP ON LINUX

```
CLASS: NT132.O11.ATCL
```
```
PROJECT ADVISOR
MSc TRAN THI DUNG
```
### HO CHI MINH CITY, 2023


```
i
```
## ACKNOWLEDGMENTS

First and foremost, we would like to express our gratitude to our advisor, MSc Tran
Thi Dung, for her guidance and consistent supervision, as well as for providing crucial
project information and assisting us in completing the research.

Our gratitude and appreciation also extend to our colleagues and lecturers who assisted
us in the development of the project, as well as to those who have volunteered their
time and skills to assist us.


## ii

TABLE OF CONTENTS

ACKNOWLEDGMENTS ......................................................................................... i

TABLE OF CONTENTS .......................................................................................... ii

LIST OF FIGURES ................................................................................................. iv



iv



- HO CHI MINH CITY,
- Introduction LIST OF TABLES vii
- 1.1 General information
- 1.1.1 Linux Server
- 1.1.2 NAT and DHCP network services
- 1.1.3 File Server on Linux
- 1.1.4 Ansible
- 1.2 Component
- 1.2.1 NAT and DHCP
- 1.2.2 File-sharing protocols: NFS, SMB, FTP
- 1.2.3 Ansible
- 1.3 Operation
- 1.3.1 NAT
- 1.3.2 DHCP
- 1.3.3 File-sharing protocols: NFS, SMB, FTP
- 1.3.4 Ansible
- Implementation
- 2.1 Topology
- 2.2 Installation
- 2.2.1 Server side................................................................................................
- 2.2.2 Client side
- 2.2.3 Install packages on Server using Ansible
- 2.3 Configuration
- 2.3.1 DHCP iii
- 2.3.2 NAT
- 2.3.3 NFS
- 2.3.4 SMB
- 2.3.5 FTP
- 2.3.6 Ansible
- Result and Conclusion
- 3.1 Result
- 3.1.1 DHCP
- 3.1.2 NAT
- 3.1.3 NFS
- 3.1.4 SMB
- 3.1.5 FTP
- 3.2 Conclusion
- REFERENCES
- APPENDIX
- Task Assignment
- Self Assessment
- Figure 1: Static NAT LIST OF FIGURES
- Figure 2: Dynamic NAT
- Figure 3: Port Address Translation (PAT)
- Figure 4: DHCP Components
- Figure 5: Ansible components
- Figure 6: NAT operation
- Figure 7: DHCP operation
- Figure 8: NFS operation
- Figure 9: SMB operation
- Figure 10: FTP Active mode
- Figure 11: FTP Passive mode
- Figure 12: Ansible operation
- Figure 13: Project topology
- Figure 14: Install NAT: iptables
- Figure 15: Install NAT: iptables-persistent [1]
- Figure 16: Install NAT: iptables-persistent [2]
- Figure 17: Install isc-dhcp-server
- Figure 18: Install nfs-kernel-server
- Figure 19: Install samba on server
- Figure 20: Install vsftpd on server
- Figure 21: Install nfs-common on client
- Figure 22: Install cifs-utils on client
- Figure 23: Install filezilla on client
- Figure 24: Install server’s packages using Ansible
- Figure 25: Ubuntu server netplan
- Figure 26: Ubuntu server network interfaces
- Figure 27: Define DHCP interfaces
- Figure 28: DHCP configuration
- Figure 29: DHCP server status
- Figure 30: Enable forwarding in sysctl.conf
- Figure 31: Enables the changes
- Figure 32: Iptables policies.......................................................................................................
- Figure 33: Network interfaces
- Figure 34: Save iptables policies v
- Figure 35: NFS Server status
- Figure 36: Create root NFS directory
- Figure 37: Set NFS folder permissions
- Figure 38: Define access for 192.168.23.11
- Figure 39: NFS Server status after restarting
- Figure 40: Create Group and User in SMB Server
- Figure 41: SMB Server status
- Figure 42: Create root SMB directory and change permissions
- Figure 43: Edit smb.conf file [1]
- Figure 44: Edit smb.conf file [2]
- Figure 45: SMB Server status after restarting
- Figure 46: FTP Server status
- Figure 47: FTP Server configuration Firewall
- Figure 48: FTP Server create User
- Figure 49: Set up OpenSSL protocol
- Figure 50: Create FTP folder and set permissions
- Figure 51: Edit vsftpd.conf file [1]
- Figure 52: Edit vsftpd.conf file [2]
- Figure 53: Edit vsftpd.conf file [3]
- Figure 54: Create SSH keys in control node
- Figure 55: Distribute public key to managed nodes
- Figure 56: Edit inventory file [1]..............................................................................................
- Figure 57: Edit inventory file [2]..............................................................................................
- Figure 58: Create Ansible playbook.........................................................................................
- Figure 59: Running playbook
- Figure 60: Client side IP
- Figure 61: Test DHCP
- Figure 62: Ip route list on client
- Figure 63: Test NAT
- Figure 64: Create a NFS test file in server
- Figure 65: Create a NFS local directory
- Figure 66: Running NFS mount command
- Figure 67: Client edits the NFS test file
- Figure 68: Changes in NFS test file vi
- Figure 69: Create a SMB test file in server
- Figure 70: Connect to SMB share folder
- Figure 71: Sign in with user created
- Figure 72: Find the SMB test file
- Figure 73: Edit SMB test file
- Figure 74: Changes in SMB test file
- Figure 75: The packet has been encrypted
- Figure 76: Server signing test
- Figure 77: Log in to FileZilla
- Figure 78: Create new folder on client
- Figure 79: Move to the new folder
- Figure 80: Test with Wireshark


```
vii
```
LIST OF TABLES

Table 1: Topology details ......................................................................................................... 12
Table 2: NFS permission table ................................................................................................. 21
Table 3: Task assignment ......................................................................................................... 38
Table 4: Self assessment ........................................................................................................... 38


## Introduction

## 1.1 General information

## 1.1.1 Linux Server

A Linux server is a powerful server that operates using a Linux-based open-source
operating system (OS). It can serve a wide range of functions, including hosting
websites and applications, managing network services, and providing file storage.

## 1.1.2 NAT and DHCP network services

### 1.1.2.1 NAT

- Network Address Translation (NAT) is a service that enables private IP
    networks to use the internet and cloud. It’s a method to map multiple private
    addresses inside a local network to a public IP address before transferring the
    information onto the internet.
- NAT offers the ability to access the internet with more security and privacy by
    hiding the device IP address from the public network, even when sending and
    receiving traffic.
- NAT inside and outside addresses:
    o Inside Local Address: An IP address within the local network, typically
       private, used for hosts within the network itself.
    o Inside Global Address: An IP address that represents one or more inside
       local addresses to the outside world, showing how the inside host is seen
       externally.
    o Outside Local Address: The real IP address of a destination host within
       the local network after translation.
    o Outside Global Address: The external IP address of a destination host
       as seen from the outside network, before any translation occurs.


### 1.1.2.2 DHCP

- Dynamic Host Configuration Protocol (DHCP) is a network protocol that
    automatically assigns IP addresses to devices on a network, enabling them to
    communicate using IP. It automates and centralizes this process, eliminating the
    need for manual IP address assignments by network administrators. DHCP is
    suitable for both small local networks and large enterprise networks.
- DHCP is a valuable tool in modern networking because it streamlines IP address
    management, enhances network efficiency, and reduces the potential for errors.
    It is especially crucial in large and dynamic network environments where
    manual IP address assignment would be impractical and error-prone.

## 1.1.3 File Server on Linux

- File Transfer Protocol (FTP) serves as a widely adopted network protocol for
    transferring files across networks, including the Internet. It adheres to a client-
    server architecture, where the client requests files from the server and the server
    delivers them. FTP is platform-agnostic, making it compatible with various
    operating systems.
- Network File System (NFS) is a distributed file system protocol specifically
    tailored for Unix and Unix-like systems. It allows files and directories to be
    shared seamlessly across a network. Remote clients can access and mount a file
    system on their local system, creating the illusion that the files are on their own
    machine. NFS offers users and applications a consistent and transparent view of
    the file system, regardless of the physical location of the files.
- Server Message Block (SMB) is a network protocol that enables users to
    communicate with remote computers and servers (e.g., to share resources or
    files). It’s also referred to as the server/client protocol because the server has a
    resource that it can share with the client.


## 1.1.4 Ansible

Ansible is an open-source IT automation platform that uses agentless architecture
to manage and configure IT infrastructure. Ansible can be used to automate a wide
range of IT tasks, including provisioning infrastructure, deploying software,
configuring systems, and managing applications.

## 1.2 Component

## 1.2.1 NAT and DHCP

### 1.2.1.1 NAT

```
There are three ways to configure NAT:
```
- Static NAT: This allows for a one-to-one mapping between local and global
    addresses. These mappings are configured by the network administrator and
    remained constant. Static NAT is typically used for servers and other devices
    that need to be consistently accessible from the public internet.

```
Figure 1 : Static NAT
```
- Dynamic NAT: A single public IP address can be shared by multiple private IP
    addresses. If the pool's IP addresses are all in use, any additional packets will be
    dropped. Dynamic NAT is typically used for home networks and small
    businesses, where there are only a few devices that need to access the public
    Internet.


## Figure 2: Dynamic NAT

- Port Address Translation (PAT) – Also known as NAT overload: One public
    IP address is used for all internal devices, but a different port is assigned to each
    private IP address. PAT is the most commonly used method as it is cost-
    effective.

## Figure 3: Port Address Translation (PAT)

### 1.2.1.2 DHCP

- DHCP Server: The DHCP server is a central component responsible for
    managing and distributing IP addresses and network configuration settings to
    clients. It maintains a pool of available IP addresses to allocate to clients.
- DHCP Client: The DHCP client is a device that requests and obtains IP
    addresses and other network configuration parameters from a DHCP server.


- DHCP Relay: The DHCP relay agent is an optional device that forwards DHCP
    messages between DHCP clients and DHCP servers. It is often used to extend
    the reach of a DHCP server beyond a single subnet.
- Others: IP address pool, Subnets, DNS Server, Lease time, ....

## Figure 4: DHCP Components

## 1.2.2 File-sharing protocols: NFS, SMB, FTP

### 1.2.2.1 NFS

```
The main components of NFS are:
```
- NFS server: responsible for exporting file systems to NFS clients.
- NFS client: responsible for mounting NFS file systems from NFS servers.

1.2.2.2 SMB
The main components of SMB are:

- SMB server: individual or multiple network servers that store SMB resources
    and grant or deny client access.
- SMB client: main device that accesses files and folders on an SMB server.
- SMB port: the system on a server or other device used by SMB servers to
    communicate; modern server variations use port 445, and older variations use
    port 139.
- SMB share: any resource housed on an SMB server; can also be referred to as
    SMB file shares.


### 1.2.2.3 FTP

```
The main components of FTP are:
```
- FTP server: Manages files and folders, authenticates and authorizes users.
- FTP client: Browses and transfers files to and from the server.
- Control connection: Negotiates transfer mode, authenticates and authorizes
    users, lists files and folders.
- Data connection: Transfers files between the client and server.

## 1.2.3 Ansible

```
The main components of Ansible are:
```
- Control node: The controller is the machine that runs Ansible and manages the
    nodes.
- Managed nodes: The managed nodes are the machines that Ansible controls.
- Inventory: The inventory is a list of the managed nodes.
- Playbooks: Playbooks are YAML files that describe the desired state of the
    managed nodes.
- Modules: Modules are Python code that Ansible uses to perform tasks on the
    managed nodes.

## Figure 5: Ansible components


## 1.3 Operation

## 1.3.1 NAT

## Figure 6: NAT operation

- NAT works on a router or gateway and interconnects two networks with each
    other. If a packet traverses outside the local (inside) network, NAT works by
    translating the private addresses into the public addresses before the data being
    transmitted to the global (outside) network. Vice versa when packet enters the
    local network from the global network.
- If NAT runs out of addresses, i.e., no address is left in the pool configured,
    incoming packets will be discarded, and the destination will receive an Internet
    Control Message Protocol (ICMP) host unreachable packet as a response.

## 1.3.2 DHCP

## Figure 7: DHCP operation


Step 1: DHCP Discover

- When connecting a new device to a network, it still does not have an IP address.
    It will ask for an IP address. It calls over the network for a DHCP server. This
    request will arrive to all of the devices (broadcast), and the DHCP server will
    also get it.
Step 2: DHCP Offer
- The DHCP hears the call, and answers with an IP address, which it оffers to the
    newly connected device.
Step 3: DHCP Request
- The IP address arrives at the device. The device will accept it and will send a
    request to use it.
Step 4: DHCP Ack
- Upon receiving the acknowledgment from the device, the server assigns an IP
    address along with the subnet mask and DNS server details to the device.
- It then logs the connected device's information, typically including the MAC
    address, the assigned IP address, and the IP address's expiration date.
- The DHCP allocates IP addresses for a specific duration. Once this period
    elapses, the IP address returns to the pool of available addresses and can be
    assigned to a different device.
- Typically, client communication occurs through UDP port 68, while servers use
    port 67. There might be slight variations based on network equipment vendors,
    but this is the general operational pattern.


## 1.3.3 File-sharing protocols: NFS, SMB, FTP

### 1.3.3.1 NFS

## Figure 8: NFS operation

- NFS operates on a client-server model where one computer serves as the server,
    and others act as clients. Clients send data requests, including read and write
    requests, to the server, which retrieves the data from storage and sends it back
    to the clients.
- However, if multiple computers or threads attempt to access the same file
    simultaneously, shared file access functionality might not function properly.

1.3.3.2 SMB

## Figure 9: SMB operation


- SMB protocol works by sending and receiving messages from a user to another
    device or file.
- A user, or SMB client, must use an SMB port for making requests. The server
    evaluates the request and can either grant or deny access. If access is permitted,
    the client gains entry to SMB shares.
- After gaining access to a SMB share, clients can modify, print, work together
    on, remove, and distribute files across a network without having to download
    the files to their individual devices

1.3.3.3 FTP

- Normally, users are required to log in to an FTP server, although certain servers
    allow access to their content without authentication, a method called anonymous
    FTP.
- When an FTP connection is set up, it creates two communication channels: the
    command channel and the data channel. The command channel facilitates the
    transfer of commands and responses between the client and server in both
    directions.
- Control connection communication occurs through port number 21. On the
    other hand, the data channel is responsible for transferring the actual data
    between the client and server and operates through port number 20.
- FTP enables clients to perform various actions like uploading, downloading,
    deleting, renaming, moving, and copying files on the server.
FTP sessions work in active or passive modes:
- In active mode, when a client starts a session through a command channel
    request, the server establishes a data connection back to the client and starts
    transferring data.


## Figure 10: FTP Active mode

- In passive mode, the server provides the client with the details necessary to
    establish a data channel through the command channel. This method is effective
    across firewalls and network address translation gateways because it allows the
    client to initiate all connections.

## Figure 11: FTP Passive mode

## 1.3.4 Ansible

- Ansible reads information about which servers to be managed from inventory
    file.
- Ansible uses SSH protocol to connect to servers and run tasks.
- Once it has connected, Ansible transfers playbook to the remote machine(s) for
    execution.

## Figure 12: Ansible operation


## Implementation

## 2.1 Topology

## Figure 13: Project topology

```
Device IP address Installed software, service
```
```
Ubuntu Server 22.04
```
### 192.168.123.10

### 192.168.23.10

```
NAT: iptables, iptables-persistent
DHCP: isc-dhcp-server
nfs-kernel-server, samba, vsftpd
Ubuntu 22.04 192.168.23.11 nfs-common, cifs-utils, FileZilla
Table 1 : Topology details
```
## 2.2 Installation

Install NAT, DHCP and file-sharing protocols in Ubuntu Server using following
commands:

## 2.2.1 Server side................................................................................................

### 2.2.1.1 NAT

```
+ “sudo apt-get install iptables”
```
## Figure 14: Install NAT: iptables


```
+ “sudo apt-get install iptables-persistant”
```
## Figure 15: Install NAT: iptables-persistent [1]

## Figure 16: Install NAT: iptables-persistent [2]

### 2.2.1.2 DHCP

```
“sudo apt install isc-dhcp-server”
```
## Figure 17: Install isc-dhcp-server

### 2.2.1.3 NFS

```
“sudo apt install nfs-kernel-server”
```
## Figure 18: Install nfs-kernel-server

### 2.2.1.4 SMB

```
“sudo apt install samba”
```

## Figure 19: Install samba on server

### 2.2.1.5 FTP

```
“sudo apt install vsftpd”
```
## Figure 20: Install vsftpd on server

## 2.2.2 Client side

```
Install file-sharing protocols in Ubuntu Client using following commands:
```
2.2.2.1 NFS
“sudo apt install nfs-common”

## Figure 21: Install nfs-common on client

### 2.2.2.2 SMB

```
“sudo apt install cifs-utils”
```
## Figure 22: Install cifs-utils on client


### 2.2.2.3 FTP

```
“sudo apt install filezillia”
```
## Figure 23: Install filezilla on client

## 2.2.3 Install packages on Server using Ansible

## Figure 24: Install server’s packages using Ansible


## 2.3 Configuration

Set static IP for 2 Network Interface Card use for DHCP Server and NAT Server
using netplan:

## Figure 25: Ubuntu server netplan

## Figure 26: Ubuntu server network interfaces

### 2.3.1 DHCP

- Step 1: Define which interface the DHCP server should listen to in the
    “etc/default/isc-dhcp-server” file.


## Figure 27: Define DHCP interfaces

- Step 2: Assign IP address pool, domain name, default gateway, lease time, ...
    in the “/etc/dhcp/dhcpd.conf” file.

## Figure 28: DHCP configuration

- Step 3: Restart the DHCP service with the following command: “sudo systemctl
    restart isc-dhcp-server.service”.


- Step 4: Check whether the server is running with: “sudo systemctl status isc-
    dhcp-server.service”.

## Figure 29: DHCP server status

## 2.3.2 NAT

```
Port Address Translation (PAT) configuration
```
- Step 1: Open the “etc/sysctl.conf” file and set the “net.ipv4.ip_forward”
    parameter to 1 by uncommenting it.

## Figure 30: Enable forwarding in sysctl.conf

- Step 2: Enable the changes to above file using “sudo sysctl -p”.

## Figure 31: Enables the changes


- Step 3: List the already configured iptable policies by using the command “sudo
    iptables -L”.

## Figure 32: Iptables policies.......................................................................................................

- Step 4: Check network interface cards.

## Figure 33: Network interfaces

- Step 5: Configure NAT.
On server, add these three instructions:
1. iptables -t nat -A POSTROUTING -o ens33 - j MASQUERADE
2. iptables -A FORWARD -i ens33 - o ens34 - m state --state
RELATED,ESTABLISHED -j ACCEPT
3. iptables -A FORWARD -i ens34 - o ens3 3 - j ACCEPT
- where ens33 is the interface to the internet (NAT)
- where ens34 is the interface to LAN, also DHCP-server interface (host-only)
line1: enable NAT on interface ens33, use ens33 for outgoing packets


```
line2: forward IP-packets from ens33 to ens34 where there is an established initial
request
line3: forward packages from ens34 to ens33
```
- Step 6: Save the config into /etc/iptables/rules.v4.

```
Figure 34 : Save iptables policies
```
## 2.3.3 NFS

- Step 1: Check the NFS server status.

## Figure 35: NFS Server status

- Step 2: Create root NFS directory, also known as an export folder.

## Figure 36: Create root NFS directory


- Step 3: Set permissions so that any user on the client machine can access the
    folder.

## Figure 37: Set NFS folder permissions

- Step 4 : Define access for NFS Clients in “etc/exports” file.
To enable access
to a single client

```
/mnt/myshareddir {clientIP}(rw,sync,no_subtree_check)
```
```
To enable access
to several clients
```
```
/mnt/myshareddir {clientIP- 1}(rw,sync,no_subtree_check)
{clientIP-2}(...)
{clientIP-3}(...)
To enable access
to an entire subnet
```
```
/mnt/myshareddir
{subnetIP}/{subnetMask}(rw,sync,no_subtree_check)
Table 2 : NFS permission table
Define access for IP 192.168.23.11:
```
## Figure 38: Define access for 192.168.23.11

- Step 5 : Make the NFS share available to Clients.
Make the shared directory available to clients using the ”exportfs” command.
Then restart NFS Kernel to apply changes.


## Figure 39: NFS Server status after restarting

## 2.3.4 SMB

- Step 1: In order to add a folder that is password protected, create a new group
    “group2” and a new user “shares” with password.

## Figure 40: Create Group and User in SMB Server

- Step 2 : Check the status of SMB server.

## Figure 41: SMB Server status

- Step 3 : Create a SMB share directory and change permissions.

## Figure 42: Create root SMB directory and change permissions


- Step 4 : Edit the “/etc/samba/smb.conf” file.

## Figure 43: Edit smb.conf file [1]

Set “security = user” for user-level authentication.
Server signing: ensures the integrity and authenticity of transmitted data.
“smb encryption = on”: encryption during transmission.

## Figure 44: Edit smb.conf file [2]

- Step 5 : Restart Samba to apply changes.

## Figure 45: SMB Server status after restarting


## 2.3.5 FTP

```
Configure FTP server:
```
- Step 1: Check the status of FTP with“ sudo service vsftpd status”.

## Figure 46: FTP Server status

- Step 2: Configure Firewall.
FTP uses port 20 for active mode, port 21 for commands, and a range of ports for
passive mode.
“sudo ufw allow 20/tcp”
“sudo ufw allow 21/tcp”
“sudo ufw allow 990/tcp” (for TLS: optional)
“sudo ufw allow 5000:10000/tcp”

## Figure 47: FTP Server configuration Firewall

- Step 3: Create user.
    “sudo adduser rosy”


## Figure 48: FTP Server create User

- Step 4: Set up OpenSSL protocol

## Figure 49: Set up OpenSSL protocol

- Step 5 : Create the folder and set permissions.
    Change this directory owner to admin user (rosy)

## Figure 50: Create FTP folder and set permissions


- Step 6 : Edit “/etc/vsftpd.conf” file, uncomment and add following lines.

## Figure 51: Edit vsftpd.conf file [1]

## Figure 52: Edit vsftpd.conf file [2]

## Figure 53: Edit vsftpd.conf file [3]


## 2.3.6 Ansible

- Step 1: Create SSH keys in control node.

## Figure 54: Create SSH keys in control node

- Step 2: Copy the Public Key to managed nodes using “ssh-copy-id”.

## Figure 55: Distribute public key to managed nodes

- Step 3: Create Inventory file.

## Figure 56: Edit inventory file [1]..............................................................................................


## Figure 57: Edit inventory file [2]..............................................................................................

- Step 4: Create an Ansible playbook.

## Figure 58: Create Ansible playbook.........................................................................................

- Step 5: Running the Playbook using the command:
    “ansible-playbook -i hosts setup_server.yaml”.

## Figure 59: Running playbook


## Result and Conclusion

## 3.1 Result

## 3.1.1 DHCP

```
Client side:
```
## Figure 60: Client side IP

## Figure 61: Test DHCP

## 3.1.2 NAT

## Figure 62: Ip route list on client

## Figure 63: Test NAT


## 3.1.3 NFS

```
Create a txt file in server to test:
```
## Figure 64: Create a NFS test file in server

```
In NFS client:
```
- Create a local directory to mount the NFS folder:

## Figure 65: Create a NFS local directory

- Mount the file share by running the mount command:

## Figure 66: Running NFS mount command

- Edit the file by client:

## Figure 67: Client edits the NFS test file

- Server can see the changes in test file:

```
Figure 68 : Changes in NFS test file
```

## 3.1.4 SMB

- Create a txt file in server to test:

## Figure 69: Create a SMB test file in server

```
In SMB Client
```
- Connect to SMB share folder:

## Figure 70: Connect to SMB share folder

- Sign in with user created:

## Figure 71: Sign in with user created


- Find the txt file in share-folder:

## Figure 72: Find the SMB test file

- Edit share file:

## Figure 73: Edit SMB test file


- Server can see the changes in the file:

## Figure 74: Changes in SMB test file

- Test with wireshark: The packet has been encrypted

## Figure 75: The packet has been encrypted

- Test smb status:

## Figure 76: Server signing test


## 3.1.5 FTP

```
In Client:
```
- Enter host IP, username, password of the user created:

## Figure 77: Log in to FileZilla

- Create a new folder on client side:

## Figure 78: Create new folder on client

- Server can move to the new folder:

### :

## Figure 79: Move to the new folder


- User Dazel can not edit because permission is not granted.

## Figure 80: Test with Wireshark

## 3.2 Conclusion

The integration of Network Address Translation (NAT) and Dynamic Host
Configuration Protocol (DHCP) services on a Linux server, along with the
implementation of file server protocols, results in the establishment of a resilient and
adaptable network environment. This project is particularly valuable for small to
medium-sized enterprises, home networks, and any environment where efficient
network management, reliable Internet access, and secure file sharing are essential. It
provides a foundation that can be further expanded and customized to meet specific
requirements, making it a valuable addition to any network infrastructure.


## REFERENCES

[1] W. by: S. Datta, “What is a network file system?,” Baeldung on Computer Science,
https://www.baeldung.com/cs/nfs (accessed Nov. 4, 2023).

[2] “What is SMB? what it decision makers need to know,” Visuality Systems,
https://visualitynq.com/resources/articles/what-is-smb-what-it-decision-makers-need-
to-know/ (accessed Nov. 4, 2023).

[3] “Dynamic Host Configuration Protocol (DHCP),” GeeksforGeeks,
https://www.geeksforgeeks.org/dynamic-host-configuration-protocol-dhcp/ (accessed
Nov. 4, 2023).

[4] “Network Address Translation (NAT),” GeeksforGeeks,
https://www.geeksforgeeks.org/network-address-translation-nat/ (accessed Nov. 4,
2023).

[5] NairaBytes, “Home,” N.A.I.R.A.B.Y.T.E.S, [http://www.nairabytes.net/linux/how-to-](http://www.nairabytes.net/linux/how-to-)
set-up-a-nat-router-on-ubuntu-server- 16 -
04?fbclid=IwAR03lzfhLjXTypdAzaFZ_POX4Kk3MCtU91XAqsq8Dh32ipOEzo0e
HeEkg7g (accessed Nov. 4, 2023).

[6] “How to setup and configure an FTP server in linux?,” GeeksforGeeks,
https://www.geeksforgeeks.org/how-to-setup-and-configure-an-ftp-server-in-linux-2/
(accessed Nov. 4, 2023).

[7] S. Zivanov, “How to install samba in ubuntu {+configuring and connecting},”
Knowledge Base by phoenixNAP, https://phoenixnap.com/kb/ubuntu-samba
(accessed Nov. 4, 2023).

[8] C. D. S. Jeff Whitaker, “Linux NFS server: How to set up server and client,” NetApp
BlueXP, https://bluexp.netapp.com/blog/azure-anf-blg-linux-nfs-server-how-to-set-
up-server-and-
client#:~:text=Network%20File%20Sharing%20(NFS)%20is,have%20access%20to
%20the%20folder (accessed Nov. 4, 2023).

[9] A. I. Nagori, How to configure Nat on ubuntu, https://linuxhint.com/configure-nat-on-
ubuntu/ (accessed Nov. 4, 2023).

[10] I. Arora, “A step-by-step guide to set up a DHCP server on ubuntu,” LinuxForDevices,
https://www.linuxfordevices.com/tutorials/ubuntu/dhcp-server-on-ubuntu (accessed
Nov. 4, 2023).

[1 1 ] “Ubuntu documentation,” NetworkConfigurationCommandLine - Community Help
Wiki, https://help.ubuntu.com/community/NetworkConfigurationCommandLine
(accessed Nov. 4, 2023).


[ 1 2] “Introduction#,” Introduction - Netplan documentation,
https://netplan.readthedocs.io/en/stable/examples/ (accessed Nov. 4, 2023).

[13] H.Maurya, Install and Configure Ansible on Ubuntu 22.04 Linux,
https://linux.how2shout.com/install-and-configure-ansible-on-ubuntu- 22 - 04 - linux/
(accessed Nov. 20 , 2023).


## APPENDIX

## Task Assignment

```
Member Task name Completion Percentage
```
```
Trần Thị Mỹ Huyền
```
- Operation of NAT, DHCP, NFS, SMB,
FTP, Ansible
- Configurate NAT and DHCP
- Result of NAT, DHCP, Ansible
- Presentation and write the project report

### 100%

```
Đoàn Hải Đăng
```
- Component of NAT, DHCP, NFS, SMB,
FTP, Ansible
- Configurate Ansible
- Demo videos
- Presentation and write the project report

### 100%

```
Phan Thị Hồng Nhung
```
- General infomation about NAT, DHCP,
NFS, SMB, FTP, Ansible
- Configurate NFS, SMB, FTP
- Result of NFS, SMB, FTP
- Presentation and write the project report

### 100%

```
Table 3 : Task assignment
```
## Self Assessment

```
Citeria 4 3 2 1
Report format (1 point) X
Presentation (1 point) X
Theory (2 point) X
Demonstration (5 point) X
Total 9
Table 4 : Self assessment
```

