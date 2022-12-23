# Active-Directory-Home-Lab
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>How to Setup a Basic Home Lab Running Active Directory Through Oracle Virtual Box</h1>
This tutorial outlines the implementation of Active Directory within Windows Servers using Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Oracle VirtualBox (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2019
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab we will create two Virtual Machines. One will be a Domain Controller using Windows Server 2019, and the other will be the Client machine using Windows 10. We will use two NICs on our Domain Controller, one will connected to our personal home network and we will use the internal network to establish a static IP, then download Active Directory Domain Services on our DC to connect to the client machine. Client machine will then be joined to the domain. We will establish a connection through the DNS settings on the client machine, the client machine will use the DC as its DNS server. We will also be adding 1,000 active users using a Powershell script that are able to login using our Client machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/lq7jPuC.png"/>
</p>
<p>
We will start by downloading Oracle VM VirtualBox, Windows Server 2019, and Windows 10.
</p>
<br />

<p>
<a href="https://www.virtualbox.org/wiki/Downloads/">Download VirtualBox</a>:
<img src="https://i.imgur.com/UYvqbj3.png"/>
</p>
<a href="https://www.microsoft.com/en-us/software-download/windows10ISO/">Download Windows 10 ISO</a>:
<img src="https://i.imgur.com/8vVq8Vy.png"/>
</p>
<a href="https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019/">Download Windows Server 2019 ISO</a>:
<img src="https://i.imgur.com/oPAOsDt.png"/>
<p>
Once we have all the necessary files donwloaded, we are going to create our Virtual Machines using VirtualBox. Once installed, open VirtualBox and we will select "New" and we're going to create our Windows Server 2019 computer first. To keep it simple we're going to name it "DC" for Domain Controller. On the OS "Version" drop down select "Windows 10 (64-bit)" and select continue through and create using the defaults. Once the DC has been created let's go to Settings > Network. Here we're going to create our second NIC for our internal network. Keep the default settings for adapter 1 since our NAT and select adapter 2, here we will check the "Enable Network Adapter" box > Select "Internal Network" from the drop down menu and press okay. We now have our VM configured and we're ready to begin installing our server. Double click on our "DC" and from the drop down locate the Windows Server 2019 ISO file that you downloaded earlier. Once selected press "Start" to run the virtual machine.
</p>
<br />
</p>
Now we will go through our Windows Setup prompts to create our Administrative User.
</p>
<img src="https://i.imgur.com/ybm87mV.png"/>
</p>
Select Windows Server 2019 Standard Evaluation (Desktop Experience)
<img src="https://i.imgur.com/pZbv43v.png"/>
Select "Custom: Install Windows only (advanced)" to format our VM hard drive and start it from scratch.
</p>
<img src="https://i.imgur.com/M7g33ty.png"/>
After the Windows Setup is complete our VM will restart. Once our system is back up we will create a password for our Admin account. To keep it simple during this lab use "Password1" for all our passwords. Our server is now set up.
<br />

</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
