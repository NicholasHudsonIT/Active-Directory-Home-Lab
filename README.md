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
<img src="https://i.imgur.com/aPt2twJ.png"/>
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
</p>
<br />
Next, we will set up our IP addressing. If you remember our diagram we have 2 NICs: One dedicated to the internet and the other dedicated for our internal network. The one on the internet will automatically get an IP Address from your home network so we don't have to do anything for that one. But for our Internal Network we will have to set it up manually.
<br />
To begin we'll click the "Network" icon at the bottom right of our desktop. Select Netowork > Change adapter options. Here you'll see two networks that we'll have to distinguish in order to establish the internal network. Rename your internet and internal connections so that they are easily identifiable. The next thing we're going to do is assign an IP to our internal network. Right click on our internal network > Select properties > Internet Protocol Version 4 (TCP/IPv4) > Use the following IP address.
</p>
<img src="https://i.imgur.com/QfFRUK5.png"/>
</p>
Now that we have established both our NICs, the next thing we are going to do is install Active Directory Domain System and we're going to create a domain. Open our Server Manager and select "Add Roles and Features," Keep defaults and when you arrive at Server Roles, select the "Active Directory Domain Services" box. Click through to the confirmation page and select Install. Next we'll create the domain for our Active Directory, under Deployment Configuration select "Add a new forest" > change Root Domain name to "mydomain.com". Click through with default settings and install. Once finished it will restart the computer for us.
<p>
  <img src="https://i.imgur.com/uTRAnDd.png"/>
</p>
<p>
Login again with our Admin account. Now we're going to create our own Domain Admin account, do that by going to Start > Windows Administrative Tools > Active Directory Users and Computers. Right click on "mydomain.com" > Select New > Organizational Unit (Think of this as a folder within AD to help us organize) > Name is "_ADMINS". Inside of our new _ADMINS folder we will create a new User. Input values for First and Last name and for the username a-"first initial + last name". Click through to Finish. To make this new user an admin, right click > Properties > Member Of > Add > type "domain admins" under object names > Select Check Names > Ok > Apply. Now we have our very own domain admin account.
</p>
<br />
To use this, let's log out. When we are back on the Windows Login screen, instead of logging in to the MYDOWMAIN\Admin we're going to go to other user and use our domain admin account (Username: a-nhudson Password: Password1). Next we're going to install RAS/NAT, Remote Access Server/Network Access Translation, to allow our Windows 10 client to be on a private virtual network but still be able to access the internet through the Domain Controller.
<p>
<img src="https://i.imgur.com/eeuAgOa.png"/>
</p>
<p>
To install our RAS/NAT we will go to our Server Manager > Add Roles and Features > Click next to Server Roles keeping defaults > Under Server Roles select "Remote Access" > Click to Roles Services and check "Routing" this will then auto select "DirectAccess and VPN (RAS) > Click through to Install. 
</p>
Next click on Tools > Routing and Account Access > Configure and Enable Routing and Remote Access > Select Network Address Translation (NAT) > Under NAT Internet Connection ensure that you can see both Internet and Internal network connections that we renamed earlier > Select Internet interface > Finish.
<br />
Once configured you will see a green circle next to IPv4. Clients will now have access to the internet assuming we set up the client computer correctly for them. To do this we will set up the DHCP server on our Domain Controller with this scope information. This will allow our Windows 10 Clients to get an IP address that will let them get on the internet and browse even though they are on a private internal network, similar to an office or school environment. 
<br />
Let's return to our Domain Controller > Roles > Click through > Select DHCP Server > Add Features > Next > Install. Now we can set up the Scope. Select Tools > DHCP > Right click IPv4 > New Scope > Name the Scope after our IP range (172.16.0.100-200) > Enter our IP range start: 172.16.0.100 - end: 172.16.0.200 > Change length to 24 so that our subnet mask is now 255.255.255.0 > Click through keeping default settings > Under Router (Default Gateway) enter the Domain Controller's IP address (172.16.0.1) > Add > Click through using defaults > Finish > Right click dc.mydomain.com > Authorize > Right Click refresh.
<br />
Essenentially what we've just done is establish our DC as our DNS server so that clients can join the domain (mydomain.com).
<p>
<img src="https://i.imgur.com/RO4HN5s.png"/>
</p>
<p>
Next we're going to use our PowerShell script to create a lot of users so we don't have to do it manually. <a href="https://github.com/joshmadakor1/AD_PS.../">Add Users w/ PowerShell</a>:
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
