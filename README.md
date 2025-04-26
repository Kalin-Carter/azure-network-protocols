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

  ![image](https://github.com/user-attachments/assets/50a5f828-35c5-4add-b7ea-f3c5581157df)

<p>
In the Azure Portal, create a new Resource Group to organize your resources. Deploy a Windows 10 VM in that group, allowing Azure to create a new Virtual Network and Subnet. Then, deploy a Linux (Ubuntu) VM in the same Resource Group, selecting the existing Virtual Network and Subnet created with the Windows VM. Finally, confirm both VMs share the same network settings to ensure they can communicate internally.

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/eec1c56e-3fc8-47b7-b09f-f770af211ced)

</p>
<p>
Within the Windows 10 VM, download and install Wireshark.

![image](https://github.com/user-attachments/assets/cdab9678-c762-44fb-b900-5000e3d88989)
launch Wireshark, then start a capture on the active network interface and filter for ICMP traffic. Ping the Ubuntu VM’s private IP from Command Prompt or PowerShell and observe the ICMP Echo traffic in Wireshark. 

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/8a0c3c98-511a-485f-9b62-2f5b3d3eef92)
![AB4CF983-8A80-40F8-8899-5D54F9322AC8_1_105_c](https://github.com/user-attachments/assets/23f3bbee-e15e-41a5-a23d-ce885b94a1c6)
</p>
<p>
Start a continuous ping from the Windows 10 VM to the Ubuntu VM’s private IP. In the Azure Portal, edit the NSG linked to the Ubuntu VM to deny inbound ICMP traffic. Watch the ping and Wireshark output—ICMP replies should stop. Then, remove or disable the deny rule to re-enable ICMP, and confirm replies resume. Press Ctrl + C to stop the ping.
</p>

![image](https://github.com/user-attachments/assets/9b095224-0671-4f2a-9d44-073e454f683b)
On the Windows 10 VM, start a new Wireshark capture and apply the filter ssh. Open PowerShell and connect to the Ubuntu VM using ssh labuser@<Ubuntu-Private-IP-Address>, entering the password when prompted. As you interact with the Linux shell, watch the SSH traffic appear in Wireshark. When done, type exit to end the session.


You can use Wireshark to see other filters such as dhcp, dns, and tcp.port == 3389 to observe DHCP, DNS, and RDP traffic on the Windows 10 VM. Renew the IP in PowerShell to see DHCP packets, and run nslookup to trigger DNS queries. RDP traffic appears as a constant stream, even when idle, due to ongoing screen updates and heartbeat signals that maintain the live session.
