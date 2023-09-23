<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

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

- Create Our Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic
- Lab Cleanup

<h2>Actions and Observations</h2>

<p>
Create Your Resources

  First things first let's create our resource group to put two Virtual Machines (VM) in. Create a Windows 10 VM and put it into the resource group we just created. The usernames and passwords do not matter for both machines, just make sure to remember them. Give enough time to allow the VM to create a Virtual Network (Vnet) and Subnet. After that create a Linux (Ubuntu) VM and put it into the same resource group and Vnet. Observe your virtual network within Network Watcher. 
  
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Observe ICMP Traffic

  Use Remote Desktop to connect to your Windows 10 VM. Within the Windows 10 VM, install Wireshark from your browser. Open Wireshark and filter for ICMP traffic only. Retrieve the private IP address of the Ubuntu VM and attempt to ping it in the Windows 10 VM's command prompt. Observe ping requests and replies within Wireshark. Then, from the Windows 10 VM, attempt to ping a public website, like google.com, and observe the traffic in Wireshark. 

  Next, initiate a perpetual ping from your Windows 10 VM to your Ubuntu VM. You will need to open the Network Security Group your Ubuntu VM is using in the Azure Portal and disable incoming (inbound) ICMP traffic. Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity. After that, re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using. Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity, it should start working. Stop the ping activity.
  
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Observe SSH Traffic

  Back in Wireshark, filter for SSH traffic only. From the Windows 10 VM, "SSH into" your Ubuntu VM via its private IP address. Type commands (username, pwd, etc.) into the linux SSH connection and observe SSH traffic spam in Wireshark. Exit the SSH connection by typing 'exit' and pressing [Enter]. 
  
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Observe DHCP Traffic

  Back in Wireshark, filter for DHCP traffic only. From the Windows 10 VM, attempt to issue your VM a new IP address from the command line using, ipconfig /renew. Observe the DHCP traffic appearing in Wireshark. 
  
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Observe DNS Traffic

  Back in Wireshark, filter for DNS traffic only. From the Windows 10 VM within a command line, use nslookup to see what a couple of websites' IP addresses are, like google.com and youtube.com. Observe the DNS traffic being shown in Wireshark.
  
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Observe RDP Traffic

  Back in Wireshark, filter for RDP traffic only using 'tcp.port == 3389'. Observe the immediate non-stop spam of traffic. Why do you think it's non-stop spamming vs only showing traffic when you do an activity? It's because the RDP protocol is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Lab Cleanup

  Close your Remote Desktop connections and delete the Resource Group created at the beginning of this lab. Verify it is deleted and congrats on completing all the steps!
  
</p>
