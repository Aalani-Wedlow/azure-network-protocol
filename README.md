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

<h2>Summarized Steps</h2>

- Create Virtual Machines (Windows 10 and Linux/Ubuntu)
- Install Wireshark within the Virtual Machine 
- Observe Traffic

<h2>Create Virtual Machines</h2>

<p>
<img src="https://github.com/user-attachments/assets/cd4ae80f-a898-4345-8905-009f67de7e20" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within Azure, create two virtual machines. One will be a Windows 10 VM and the other will be a Linux (Ubuntu) VM. To complete this task:

  - Create a Resource Group within Azure (this can be done while creating the virtual machine)
  
  - Go to the Virtual Machine service. Select create in the top right corner and select the resource group that was created.

  - Name the virtual machine, select your desired subscription, and select either Ubuntu Server or Windows 10 Pro (you'll have to make both of these but they follow the same process)

  - Select a size of at least 2 vcpus and 8 GiB memory, create a username/password, and check off the box at the bottom of the page

  - After finishing the basics, go to the Networking tab and ensure that the subnet is set to the default (10.0.0.0/24)

  - Now select review and create

  - Repeat the same process for the second VM, but ensure that they remain in the same Virtual Network   
</p>
<br />

<p> <h2>Installing Wireshark</h2>
<img src="https://github.com/user-attachments/assets/efdfc17c-60e5-453c-99be-b79d4ec40032" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Within the Windows 10 VM that was created, Wireshark will be installed. To complete this:

  - Grab the Windows 10 public IP address and log into the VM using a Remote Desktop Application. Enter the username and password you created while making the VM to log into the VM.

  - Once opened, go into Microsoft Edge and <a href="https://www.wireshark.org/download.html">install Wireshark</a>.

  - From there, open the downloaded file and go through the instructions till Wireshark is fully installed.
</p>
<br />

<p> <h2>Observe ICMP Traffic</h2>
<img src="https://github.com/user-attachments/assets/4bfe0ca2-7e53-4f3f-918c-684017c79449" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To observe ICMP Traffic:

- Open Wireshark

- Highlight ethernet and click the sharkfin in the upper left corner (This allows us to see how the computer is communicating on the back end and inspect traffic on the VM)

- Filter for ICMP Traffic by typing ICMP in the Wireshark search bar

- Now, attempt to ping the Linux VM by grabbing it's private IP address (ex. 10.0.0.4)

- Open PowerShell and type ping follwed by the private IP address of the Linux VM (or any other system you're using)

- If there's a desire to create a perpetual ping, type ping followed by the private IP address "-t"


  <p> <h2>Observe SSH Traffic</h2>
<img src="https://github.com/user-attachments/assets/a0cd2829-9682-4bf2-8ce5-29221c5bc961" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 - Filter for SSH traffic in the search bar in Wireshark 

 - Within Powershell, type "username"@"linux priavte IP address" and press enter. It will ask if you're sure if you want to enter, type yes.

 - From here it will ask for a password, use the one created while making the VM's

 - With this, you are able to see various means of traffic through the SSH connection in Wireshark.

  <p> <h2>Observe DHCP Traffic</h2>
<img src="https://github.com/user-attachments/assets/eb1a24b5-b680-4119-ae02-3e6042acae4a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Within PowerShell, type ipconfig /renew

- Filter the traffic to DHCP traffic on Wireshark and you'll be able to observe the process of the Windows VM requesting a new IP address on the Virtual Network. We are able to see the response from Azure, providing an IP address to the Windows VM. 

  <p> <h2>Observe DNS Traffic</h2>
<img src="https://github.com/user-attachments/assets/3ca5905f-80ec-4ec5-a5ab-f785f7951f5d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Within Wireshark, type dns or udp.port == 53 to filter for DNS traffic

- Within PowerShell, type nslookup www.google.com. This will allow you to recieve a response form the DNS server with the IPv4 address of google.com. DNS traffic is also able to be observed within Wireshark, providing a highlight of the different lookups preformed in the attempt to gather information about google.com.

  <p> <h2>Observe RDP Traffic</h2>
<img src="https://github.com/user-attachments/assets/9d771f34-1820-49bf-bbfc-fc7cea6bf082" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  - Within Wireshark, type rdp or tcp.port == 3389 to filter for RDP traffic

  - By entering this in, we're able to observe a continuous stream of data. This is a representation of the constant flow of RDP traffic being exchanged between the local computer and the Windows VM. There's non-stop traffic due to the lack of activity by the user. 
<br />
 <h4>Side Note</h4>
<p> Be sure to delete any Resource Group upon completion to ensure the avoidance of extra charges.
</p>

  
