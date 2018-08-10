# The 
1st payload mirai 2nd eternalrock 3rd notpetya then a miner
gathering of potential new infection targets is done in a separate thread that is created by the malware after the MBR is infected and the shutdown task scheduled:notpetya-code-1.png
IP addresses or hostnames to infect can be passed in via the -h command-line switch:notpetya-code-2.png
NotPetya enumerates all local network interfaces and scans the directly connected subnets for hosts that accept TCP connections on port 139 or 445.  Any host that answers is added to the target list.:notpetya-code-3.png
For any DHCP-enabled interface, the address of the DHCP server is added to the target list:notpetya-code-4.png
NotPetya uses the NetServerGetInfo API to determine if it is running on a server or domain controller.  If so, it will then use the DhcpEnumSubnets, DhcpGetSubnetInfo and DhcpEnumSubnetClients API to enumerate all clients for which this server is acting as a DHCP server.  It will then try to make a TCP connection to the SMB ports of each DHCP client.  Any host that answers is added to the target list.:notpetya-code-5.png
Every 3 minutes, NotPetya fetches a list of active TCP connections via the GetExtendedTcpTable API and adds the remote IP addresses of these connections to the target list.  This is one way for hosts outside of the victim’s local Windows network can be selected as propagation targets.:notpetya-code-6.png
IP addresses found in the local ARP cache are also added to the target list.  The contents of the ARP cache are fetched every 3 minutes via the GetIpNetTable API.:notpetya-code-7.png
Finally, NotPetya uses the browser service to list all servers visible in the Active Directory domain (NetServerEnum API).  Any that are running on Windows 2000 or later are added to the target list.:notpetya-code-8.png
Once the list of targets is gathered, the malware creates a network propagation thread that attempts to connect to the admin$ share of each target (using dumped credentials if available) and upload a copy of itself to it:notpetya-code-9.png
It will then try to remotely execute its copy, first via PsExec, and failing that, via WMIC:notpetya-code-10.png
