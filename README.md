<p align="center">
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

Create Resources
Observe ICMP Traffic
Observe SSH Traffic)
Observe DHCP Traffic
Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>Resource Groups (similar to a file system) logical collections of virtual machines, storage accounts, virtual networks, web apps, databases, and/or database servers.  
Virtual Machines (VM) allow you to more easily scale your applications by adding more physical or virtual servers to distribute the workload across multiple VMs. We created two Virtual Machines (pictured below) of differing Operating Systems (Windows 10 21H2 & Ubuntu Server 20.04) that will be used for Remote Deskop and to observe network traffic between the two devices. 
</p>

<p>
<img src="https://i.imgur.com/BifIhxG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>Remote desktop allows the user to connect to a computer in another location, see that computer's desktop and interact with it as if it were local. A quick search for "remote desktop connection" will allow the user to access the VM. Here we will be entering the details of the public IP address for VM1 (Windows 10 21H2) to install Wireshark (packet analysis software) instead of using our local machine. (below pictured search of remote desktop and the result to enter IP address)</p>

<p>
<img src="https://i.imgur.com/PEeQyYV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />
<p>
After retrieving the private IP address from VM2 (Linux Ubutu Server 20.04) we can now ping the private address from VM (Windows 10 21H2) that we've used to remote into. So we can now view the traffic travel from VM1 to VM2 by filtering the ICMP packets in Wireshark. We can use the ping command to test the connection between machines for connectivity. The filter traffic is shown below in Wireshark is pictured (left) and Powershell (right):
</p>
<p>
<img src="https://i.imgur.com/dVGKgAq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />
