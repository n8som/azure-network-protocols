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

  First things first let's create our resource group to put two Virtual Machines (VM) in. Create a Windows 10 VM and put it into the resource group we just created. 
  
</p>
  
 <p> 
   
   ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/24251fd1-21e7-40ca-9327-560091e749ac)
 </p>
 
  The usernames and passwords do not matter for both machines, just make sure to remember them. Give enough time to allow the VM to create a Virtual Network (Vnet) and Subnet. After that create a Linux (Ubuntu) VM and put it into the same resource group and Vnet.
  <p>
    
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/32d1ce04-57e8-4476-95b2-3cfe362747bc)
    ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/8b26c011-f0f4-4df2-9b67-d761c27f8d78)
  </p> 
  
  <p>  Observe your virtual network within Network Watcher. </p>

  
</p>
<p>
  
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/68ee5c80-3f6b-4010-ab44-3dd54180a25d)

</p>
<br />

<p>
Observe ICMP Traffic

  Use Remote Desktop to connect to your Windows 10 VM using the public IP from Azure.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/023c858f-13f0-4bfe-8355-3a0470735131) 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/2973052c-0427-47cc-8ae8-6c17e8941c82)
  
  Within the Windows 10 VM, install Wireshark from your browser. Open Wireshark and filter for ICMP traffic only. Retrieve the private IP address of the Ubuntu VM and attempt to ping it in the Windows 10 VM's command prompt. Observe ping requests and replies within Wireshark.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/422e430d-517f-45d3-9859-43f83993ed06)
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/6acdf2ef-12b0-4770-9b66-03c7f93be455)

  Then, from the Windows 10 VM, attempt to ping a public website, like google.com, and observe the traffic in Wireshark. 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/56724ea5-6a09-43b3-8036-6fff25504ef8)
  
  Next, initiate a perpetual ping from your Windows 10 VM to your Ubuntu VM.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/1d9750f4-54a3-4a8d-be5f-7a228f0982dd)

  You will need to open the Network Security Group your Ubuntu VM is using in the Azure Portal and disable incoming (inbound) ICMP traffic.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/67a8cf3e-a4da-44a8-8a10-7d119cbc10e6)

  Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/51b4c183-4360-4fc8-832c-ef745edee4d8)

  After that, re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using. Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity, it should start working. Stop the ping activity.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/a2ab5be6-3599-43c3-8132-30dcef68ecb3)

  
</p>
<br />

<p>
Observe SSH Traffic

  Back in Wireshark, filter for SSH traffic only. From the Windows 10 VM, "SSH into" your Ubuntu VM via its private IP address. 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/0e863001-4f3e-48b4-a952-d85cad5ccdec)

  Type commands (username, pwd, etc.) into the linux SSH connection and observe SSH traffic spam in Wireshark. Exit the SSH connection by typing 'exit' and pressing [Enter]. 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/30a27a19-b689-494f-81f8-2bc54d9d05ef)

</p>
<br />

<p>
Observe DHCP Traffic

  Back in Wireshark, filter for DHCP traffic only. From the Windows 10 VM, attempt to issue your VM a new IP address from the command line using, ipconfig /renew. Observe the DHCP traffic appearing in Wireshark. 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/b8064cee-af22-4ad1-9c3c-fb4475e79b5b)

</p>
<br />

<p>
Observe DNS Traffic

  Back in Wireshark, filter for DNS traffic only. From the Windows 10 VM within a command line, use nslookup to see what a couple of websites' IP addresses are, like google.com and youtube.com. Observe the DNS traffic being shown in Wireshark.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/5bc26e09-4f01-4319-adba-5926d514b5b8)
 
</p>
<br />

<p>
Observe RDP Traffic

  Back in Wireshark, filter for RDP traffic only using 'tcp.port == 3389'. Observe the immediate non-stop spam of traffic.
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/0f40a7be-2c97-4215-8e6f-f7c1666d0672)

  Why do you think it's non-stop spamming vs only showing traffic when you do an activity? It's because the RDP protocol is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted. 
  
</p>
<br />

<p>
Lab Cleanup

  Close your Remote Desktop connections and delete the Resource Group created at the beginning of this lab. 
  ![image](https://github.com/n8som/azure-network-protocols/assets/110139109/c9f97640-7573-4f5f-8e92-c42a0d80211b)

  Verify it is deleted and congrats on completing all the steps!
  
</p>
