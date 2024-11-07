<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this home lab, I observe different networking activities/traffic using multiple ports such as ICMP, SSH, RDP, and DNS through the use of Wireshark and Powershell on a Windows and Linux VM.<br />

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

Part 1 (Create our Resources)
1.	Create a Resource Group
2.	Create a Windows 10 Virtual Machine (VM)
3.	While creating the VM, select the previously created Resource Group ![image](https://github.com/user-attachments/assets/bcfe5971-0a91-47a8-8028-4e42e2efe1c9)

4.	While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet ![image](https://github.com/user-attachments/assets/f88448be-4b33-4195-bb62-aa19d65a81f0)

5.	Create a Linux (Ubuntu) VM ![image](https://github.com/user-attachments/assets/6b128cfd-2ea6-4960-813c-93130d98e123)

6.	While create the VM, select the previously created Resource Group and Vnet  ![image](https://github.com/user-attachments/assets/130b8970-52d5-4a1b-a616-96aaaab416ee)![image](https://github.com/user-attachments/assets/be3d9782-84c7-4590-871f-dee49b55ffb7)


7.	Observe Your Virtual Network within Network Watcher




Part 2 (Observe ICMP Traffic)
1.	Use Remote Desktop to connect to your Windows 10 Virtual Machine

2.	![image](https://github.com/user-attachments/assets/9beedd20-fcb4-4fe6-9197-f66de1db5037)

3.	Within your Windows 10 Virtual Machine, Install Wireshark

4.	![image](https://github.com/user-attachments/assets/76b18670-cb2d-4dfe-a955-3cf23ad2c764)

5.	Open Wireshark and filter for ICMP traffic only
   
6.	![image](https://github.com/user-attachments/assets/f694f4d6-4c6a-4072-ac9d-3f4745f7c0d6)![image](https://github.com/user-attachments/assets/3295c8d5-cf75-42d3-bc92-337971734a38)


7.	Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM

a.	Observe ping requests and replies within WireShark 

![image](https://github.com/user-attachments/assets/04d699d2-65f8-4bad-8d69-075962fd372d)
8.	From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark 

![image](https://github.com/user-attachments/assets/a75193f9-32c2-4a42-b0bb-a8018f1c5db5)

9.	Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM
    
![image](https://github.com/user-attachments/assets/704346b0-8e9c-45d6-b3e6-aeb3858092b9)

a.	Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic 

![image](https://github.com/user-attachments/assets/1eb91e3b-1aed-4d15-8402-d2f12f43dc17)

b.	Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
c.	Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
d.	Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
e.	Stop the ping activity

Part 2 (Observe SSH Traffic)
1.	Back in Wireshark, filter for SSH traffic only
2.	From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address)
a.	Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
b.	Exit the SSH connection by typing ‘exit’ and pressing [Enter]

![image](https://github.com/user-attachments/assets/98506428-2d79-4886-b8b1-1cf7e171036a)


Part 2 (Observe DHCP Traffic)
1.	Back in Wireshark, filter for DHCP traffic only
2.	From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
a.	Observe the DHCP traffic appearing in WireShark

![image](https://github.com/user-attachments/assets/9549dbae-fa51-4243-bb07-bc031f601cd5)

(11/4/24 8:58AM Comments: Now here I received this error but I will try again later at my home computer and document my findings)
Part 2 (Observe DNS Traffic)
1.	Back in Wireshark, filter for DNS traffic only
2.	From your Windows 10 VM within a command line, use nslookup to see what any website’s IP addresses are
a.	Observe the DNS traffic being shown in WireShark

![image](https://github.com/user-attachments/assets/0e32494b-3551-46d3-8b39-44f89a8f563d)


Part 2 (Observe RDP Traffic)
1.	Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
2.	Observe the immediate non-stop spam of traffic? Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?

Answer: Because the RDP (protocol) is constantly showing you a live stream from one computer to another, traffic is always being transmitted as opposed to SSH, where only inputting keystrokes
will send traffic.
![image](https://github.com/user-attachments/assets/dcbdcecc-4847-4cdf-813c-a623185a4b8a)

<h2>Takeaways and Key Skills Developed</h2>
In this lab, I explored network security and traffic monitoring between Azure VMs using various protocols and tools like Wireshark and PowerShell. I created both Windows and Ubuntu VMs and monitored traffic for ICMP, SSH, DNS, RDP, and DHCP. By filtering traffic in Wireshark, I observed how different protocols behave, such as viewing ICMP traffic during ping tests, SSH traffic during remote connections, and RDP traffic for remote desktop sessions. I also configured Network Security Groups (NSGs) to control traffic, such as blocking ICMP and monitoring its impact in real-time. This lab helped me understand how different protocols interact in a cloud environment and the role of NSGs in securing network traffic.
