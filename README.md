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

<h2>Actions and Observations</h2>
<p>
  Greetings and welcome to my tutorial on inspecting network protocols and network security groups. You have to initially create two virtual machines (VMs) on Azure. There will be two machines: one running Linux(Ubuntu server) and the other Windows 10. They both need to have two CPUs and be connected to the same VNET. After that is step, launch Wireshark on the Windows computer.Here is a download link for Wireshark.https://www.wireshark.org. After installation, use Wireshark and limit it to ICMP traffic only. A network layer protocol called ICMP is used for sending messages about problems with network connections. Ping tests host connectivity by utilizing this protocol. When we ping our Linux machine's private IP address and filter Wireshark to only collect ICMP packets, we see the packets on Wireshark.
</p>
<p>
<img width="714" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/b0330574-a5c1-4ddb-a0e3-907ff4b9f15d">
<img width="581" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/c5445f95-3d88-4bc9-adc6-8348dce020ea">
</p>

<p>
The image below shows each individual packet and the actual data that is being sent in each ping.
</p>
<p>
  <img width="1440" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/a4b8f479-f743-4c7b-b4cc-eb2fd059a82d">
</p>

<br />

<p>
  We will use the command ping -t to continuously ping the Linux machine in this section of the lab. While the Windows system is pinging the Linux machine, we will go to the Linux machine and block incoming ICMP traffic on its firewall. This will continue to ping the machine until we decide to stop it. We will no longer get echo responses from the Linux machine when we accomplish that. By creating a new Network Security Group on the Linux computer and configuring it to block ICMP, we will be able to block ICMP. By enabling ICMP on the Linux Network Security Groups tab on Azure, we can enable the traffic.
</p>
<img width="425" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/63234731-f95a-4604-a709-bdcf8a93aeb6">
<img width="851" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/33a0b214-f7c1-4dac-a209-ef7a97c44238">
<img width="1435" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/c00090e2-eed7-4081-86db-cd64bbfa94c9">

<br />

<p>
Next, we will use our Windows machine SSH command line (which gives you access to the computer's command line you use when connecting to a Linux box or when connecting to switches) to connect to the Linux machine. Secure Shell has no GUI unlike RDP it just gives the user access to the machine's CLI. We will set the wireshark filter to capture SSH packets only. When we SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that Wireshark starts to immediately capture SSH packets.
Then, we will request a new IP address with the command "ipconfig /renew" Once we enter the command wireshark will get DHCP traffic. Then use Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67 & 68. It is used to assign IP addresses to machines.
</p>

<p>
<img width="814" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/aa584ae6-bf54-4698-bfb8-782062c436a7">
<img width="624" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/8225eeaf-8090-421c-b4c1-cdbefb540f60">
<img width="651" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/8ab36bad-4a7b-418c-90b4-90b657602d6c">
<img width="645" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/38a7b99b-b669-4d01-9d8b-9aa01cb24b68">
</p>

<p>
Wireshark will be configured to filter DNS traffic. Entering the command "nslookup www.google.com" will start DNS traffic. In basic terms, this operation queries our DNS server for Google's IP address. Finally, we'll apply an RDP traffic filter. Because we are connecting to our virtual machine using Remote Desktop Protocol, traffic is constantly emitted when we enter tcp.port==3389.
</p>
<p>
  <img width="1392" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/59c122a0-d61c-408d-88e2-2981eaf7d7ff">
  <img width="1440" alt="image" src="https://github.com/abdijalilimam/azure-network-protocols/assets/137457871/60171dd3-2370-45ee-9aef-c259eddc6980">
</p>
<br />
