<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used

- Windows 10 (21H2)
- Linux Ubuntu Server 20.04

## High-Level Steps

- Create Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe RDP Traffic

## Actions and Observations


|Terms | Descriptions|
|-------|------------|
|Resource Groups| Resource Groups (similar to a file system) logical collections of virtual machines, storage accounts, virtual networks, web apps, databases, and/or database servers.  
|Virtual Machines (VM)| Virtual Machines (VM) allow you to more easily scale your applications by adding more physical or virtual servers to distribute the workload across multiple VMs.
|Remote Desktop| Remote desktop allows the user to connect to a computer in another location, see that computer's desktop and interact with it as if it were local.
|Tags| Tags are metadata elements that you apply to your Azure resources. They're key-value pairs that help you identify resources based on settings that are relevant to your organization
|Region| Each Azure region features datacenters deployed within a latency-defined perimeter. They're connected through a dedicated regional low-latency network. This design ensures that Azure services within any region offer the best possible performance and security.

### Step 1
The first step will be creating a Resource group that will house our two Virtual Machines to observe the traffic being sent between the two machines. To Create the Resource Group you can do a quick search for `Research Group ` at the top of Azure or you can select `Create a Resource` and then choose to create the Resource group from the Azure Market Place. 

<p align="center">
  <img src="https://i.imgur.com/GKUzeBu.png" height="70%" width="70%" alt="search resource group" /> 
  </p>
  
After `Research Group` is entered and the results are returned, you can now select the `+ Create` button. This will begin the process of creating the resource group that will have eaach of our resources such as the virtual machines.  
<p align="center">
<img src="https://i.imgur.com/Tqk3vLR.png" height="80%" width="80%" alt="search and create resource group" />
</p> 

You will select the `subscription name`, enter your custom created `resource group` (here enter RG-LAB-2) name and select the preferred `region` that is the nearest to you that would assist saving on cost. As you are creating the resource group, you will see the option to create  tags as well; however, in this lab, the tag is not needed.

<p align="center">
<img src="https://i.imgur.com/Kwqp98O.png" height="60%" width="60%" alt="create resource group name" />
  </p>

The two virtual machines that we create will allow us to send traffic between the two machines. You can name the Virtual Machine whatever you prefer to name them and can be easy to remember. 

The first virtual machine that we will create will be Windows Operating System and will be named VM1, so we can do a quick search at the top of Azure for `Virtual Machine` then select Virtual machines from the search results. 

Once that is done, we can then choose to `+ Create` from the top left or at the center of the page. You will then select the `create a virtual machine hosted by azure` option. 

<p align="center">
 <img src="https://i.imgur.com/zzBqzpk.png" height="80%" width="80%" alt="create virtual machine" />
 </p> 

Below you will select the `subscription`, the same `resource group` (RG-LAB-2), name the `virtual machnine` (VM1), select the same `Region` as the resource group as selected previously and the others that are pictured below. 
<p align="center">
  <img src="https://i.imgur.com/oddn03a.png" height="70%" width="70%" alt="create virtual machine name" />
  </p>

The remaining portion of the basics section for the virtual machine consist of the virtual machine size, the username, password, inbound port rules and licensing. We allow port 3389 so that we can later remote desktop into the virtual machine. 

   >**Note**: Be sure to select the check box for licensing otherwise you will be prompting with an error message when the virtual machine is validation at time of creation.

<p align="center">
 <img src="https://i.imgur.com/U0QyvqS.png" height="70%" width="70%" alt="create size of windows vm"/>
  </p>

We created two Virtual Machines (pictured below) of differing Operating Systems (Windows 10 21H2 & Linux Ubuntu Server 20.04) that will be used for Remote Deskop and to observe network traffic between the two devices. 


<p align="center">
<img src="https://i.imgur.com/BifIhxG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

A quick search for "remote desktop connection" will allow the user to access the VM. Here we will be entering the details of the public IP address for VM1 (Windows 10 21H2) to install Wireshark (packet analysis software) instead of using our local machine. (below pictured search of remote desktop and the result to enter IP address)

<p align="center">
<img src="https://i.imgur.com/PEeQyYV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

On the virtual machine with Windows 10, download Wireshark (Windows Installer 64-bit) and continue with all the default options.

Npcap will pop up to install, go ahead and install that with defaults and wireshark will continue to install after.

Open Wirehsark in the VM, click Ethernet and then click the blue fin at the top left under 'File' to begin capturing packets. Notice all the traffic already happening that happens in the background.

<br />

After retrieving the private IP address from VM2 (Linux Ubutu Server 20.04) we can now ping that private IP address from VM1 (Windows 10 21H2) that we've used to remote into. We can use the ping command to test the connection between machines for connectivity. So we can now view the traffic travel from VM1 to VM2 by filtering the ICMP packets in Wireshark. We can also ping other IP address or a domain names (www.google.com). The filtered traffic that is requested and its corresponding reply is shown below in Wireshark is pictured (left) and Powershell (right):

<p align="center">
<img src="https://i.imgur.com/SriHAg2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

If we want to deny the ping request we can add this rule to our Network Security Group inside the Virtual Machine and once we've added this rule to VM2, we can see that the traffic times out in PowerShell along with Wireshark longer displaying a reply to this request. 
 
  <p align="center">
  <img src="https://i.imgur.com/mTtkBrg.png" height="80%" width="80%" alt="ping traffic"/>
  </p>
  <br/>
  <p>Wireshark and PowerShell timed out after denying icmp (ping) traffic</p>
  <p align="center">
<img src="https://i.imgur.com/8epnq3H.png" height="80%" width="80%" alt="icmp traffic deny"/>
</p>
</br>
We can filter in Wireshark, with a the filter functionality we can filter SSH only traffic. From the Windows 10 VM, we enter SSH into Linux Virtual Machine (VM2) (using "ssh username@ip address" its private IP address). When we use commands such as touch, pwd (print working directory) or ls (list), into the linux SSH was used to connect. SSH traffic is observed spamming in WireShark. The SSH connection can be exited, by typing ‘exit’ and pressing [Enter].

<p align="center">
<img src="https://i.imgur.com/DpJdcZl.png" height="80%" width="80%" alt="ssh traffic"/>
</br>
<p> We can filter in Wireshark for "DHCP traffic only". From VM1 (Windows 10 21H2), a new IP address was issued from the command line (ipconfig /renew). Now DHCP traffic can be observed in WireShark.</p>
<p align="center">
<img src="https://i.imgur.com/GLm7cMT.png" height="80%" width="80%" alt="dhcp traffic"/>
</p>
</br>
<p> In Wireshark, we can filter for RDP traffic only (tcp.port == 3389) because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is consistently being transmitted </p>
<p align="center">
<img src="https://i.imgur.com/rB07Fw7.png" height="80%" width="80%" alt="tcp 3389"/>
</p>
<p align="center"><i><b>🌤️"Learn something new, or a new way of approaching something old because there are a few skills that are as valuable as the art of learning.”🌤️</p></i></b>
