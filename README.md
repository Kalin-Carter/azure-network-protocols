# azure-network-protocols<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Windows and Linux virtual machine
- Install wireshark
- Configure firewall and observe DHCP, DNS, and RDP Traffic
  

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Deploying Windows and Linux VMs in a Shared Azure Environment
Create a Resource Group

In the Azure Portal, create a new Resource Group to manage all related resources.
Deploy a Windows 10 Virtual Machine
Create a Windows 10 VM, and during setup:
Select the Resource Group you just created.
Allow Azure to automatically create a new Virtual Network and Subnet for this VM.
Deploy a Linux (Ubuntu) Virtual Machine
Create a Linux VM (Ubuntu), and during setup:
Choose the same Resource Group used for the Windows VM.
Select the existing Virtual Network and Subnet created with the Windows VM—this is essential to ensure both VMs can communicate.
Set the Authentication Type to Username and Password.
Final Check
Confirm that both VMs are deployed within the same Virtual Network and Subnet to enable internal communication between them.


</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Connect to the Windows 10 VM
If you're using a Mac, install Microsoft Remote Desktop from the App Store.
Use it to connect to your Windows 10 Virtual Machine in Azure.
Install Wireshark
Within the Windows 10 VM, download and install Wireshark.
Launch Wireshark and begin a packet capture on the active network interface.
Filter for ICMP Traffic
In the Wireshark filter bar, enter ICMP. This will show only ping (ICMP) traffic.
  Test Internal Ping
From the Windows 10 VM, retrieve the private IP address of your Ubuntu (linux) VM from the Azure Portal.
Open Command Prompt or PowerShell and ping the Ubuntu VM using its private IP.
Observe the ICMP Echo Requests and Echo Replies in Wireshark.
Test External Ping
In the same Windows 10 session, try pinging a public website (e.g., www.google.com) from the Command Prompt or PowerShell.
Watch the traffic in Wireshark to observe how external ICMP traffic is handled.
Note that ICMP to public addresses may be blocked by Azure by default, so you might not see replies, but requests will still show in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Firewall Configuration: Controlling ICMP Traffic via Network Security Group (NSG)
Start a Continuous Ping Test
From the Windows 10 VM, initiate a continuous ping to the private IP address of the Ubuntu VM
Block ICMP Traffic
In the Azure Portal, navigate to the Network Security Group (NSG) attached to the Ubuntu VM’s network interface or subnet.
Add or modify an inbound security rule to deny ICMP traffic (Protocol: ICMP, Source: Any, Destination: Any, Action: Deny).
Observe the Effect
Back in the Windows 10 VM, monitor the ping output and Wireshark capture.
You should see that ICMP echo replies stop, indicating the traffic is being blocked.
Re-enable ICMP Traffic
Return to the NSG settings and remove or disable the deny rule, allowing ICMP traffic again.
Verify Restoration
Back in the Windows 10 VM, observe that ping replies resume in both the command line and Wireshark.
Stop the Ping Activity
Press Ctrl + C in the command prompt to stop the continuous ping.
</p>

Observe SSH Traffic from Windows to Ubuntu
Start a New Wireshark Capture
On the Windows 10 VM, open Wireshark and begin a new packet capture.
Use the filter ssh
Establish an SSH Connection
Open PowerShell and SSH into the Ubuntu VM using its private IP ssh labuser@<Ubuntu-Private-IP-Address> Enter the password when prompted, and interact with the Linux shell.
Monitor Traffic
Observe the steady stream of SSH packets in Wireshark as you type commands.
Exit SSH Session
To disconnect, type exit and press Enter.

Observe DHCP Traffic
Filter for DHCP in Wireshark
Use the following Wireshark filter dhcp
Renew the IP Address
In PowerShell (run as Administrator) on the Windows 10 VM, issue
Watch the Traffic
Observe DHCP discovery, offer, request, and acknowledgment packets in Wireshark as the VM requests a new IP.

Observe DNS Traffic
Apply DNS Filter in Wireshark

Use the following Wireshark filter dns 
Perform DNS Lookups
In Command Prompt or PowerShell, use nslookup to resolve domain names nslookup google.com / disney.com
Monitor DNS Queries
Watch the DNS request and response packets in Wireshark

Observe RDP Traffic
Filter RDP in Wireshark
Use the following filter to isolate Remote Desktop traffic tcp.port == 3389
Observe Traffic Behavior

You’ll notice a constant stream of RDP packets, even when idle.
Why the Continuous Traffic?
Answer: RDP maintains a live visual connection between client and host. The session continuously transmits screen updates, cursor movements, and heartbeat signals, resulting in non-stop traffic to ensure a real-time experience.
