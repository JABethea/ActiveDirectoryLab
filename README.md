<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
In this lab we will be creating a lab environment utilizing Windows 10 and Windows Server 2019 VMs on a Virtual Box Hypervisor. We will use the Windows Server 2019 VM as our domain controller, which we will use use to authorize and authenticate users that we establish in AD. 

- The first steps in this lab will be downloading VirtualBox, as well as the Windows 10 and Windows 2019 ISOs, so that we can install and configure the two OS onto the VMs. 
- After getting things installed and set up, we will be setting up the first VM as the Domain Controller, which will house Active Directory. We will give the DC two NIC network adaptors, one will be connect to the external network internet, the other will be an internal network for virtual box client users to connect to. IP Addressing will be assigned for the internal network on the DC, the external internet will come from your home router.
- Once IP Addressing is set up, we will install AD and create our domain, then configure RAS & NAT so that the client can reach the intenret through the DC, and then we will set up DHCP on the DC so that we can automatically allocate an IP address for the client Windows 10 VM.
- The last step on the DC will be to run a powershell script to create over 1,000 users in AD. 
- After creating the users, we will create a VM and install windows 10 on it for connection to the private Virtual Box Network. This will be named Client 1, join it to the domain, and then join it to a domain account. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)
- <b>Windows Server 2019

<h2>Program walk-through:</h2>

<p align="center">

### Setting up VirtualBox & configuring our Virtual Machines
Download Oracle VirtualBox for your appropriate OS: <br/>
<img width="900" height="900" alt="Virtual Box Downloads Page" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/48abcfd2-b842-427f-8afa-78088ffe6a92">
<br />
<br />
Make sure to also install the Virtual Box Extension Pack: <br/>
<img width="900" height="900" alt="Virtual Box Extension Pack Install" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/f83ac1fd-d6d7-4632-a6c7-1d86c511b32e">
<br />
<br />
Now download the ISO images for Windows 2010 and Windows Server 2019. Be sure to select the ISO and not one of the other options. 
Note: Microsoft may require information from you in order to access Server2019 for download.  <br/>
<img width="450" height="275" alt="Windows Server 2019 Install Page Choose ISO" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/9e944941-3429-436e-8976-1f93ea8b6aa9">
<img width="450" height="275" alt="Windows Server 2019 Install Page ISO" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/9283b841-93ee-444d-882f-921ce79e8bdf">
<br />
<br />
For the Windows 10 download process, use the Installation Media and select the ISO file radio buttons and your computer architecture.<br/>

<img width="450" height="450" alt="Windows 10 Installation Media on Windows 10 Download Page" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/ebd66c8b-29e3-44f0-a3b7-81f67e321a5f">

<img width="450" height="450" alt="Windows 10 Installation Media Choose Proper Specifications" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6cad9029-9e1e-41d7-8e68-c39ad778e5fc">

<img width="450" height="450" alt="Windows 10 Installation Media ISO file select option" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6b55369a-f957-4825-8bb6-50d43bf48fbe">

<img width="450" height="450" alt="Windows 10 Installation Media ISO file select over USB" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/78a74e76-f446-4c91-abf3-2dded0f0f397">
<br />
<br />
The first Virtual Machine will run Windows Server 2019, choosing the correct ISO file and configuring sufficient RAM and CPU power <br/>
This will function as the Domain Controller, so name it DC </br>
<img width="900" height="450" alt="VM Hardware Configuration for Windows2019" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/5e8bfc67-ece7-4806-ab71-54ac905150e5">
<img width="900" height="450" alt="DC VM on Virtual Box" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/71373618-418e-4d93-9521-f7ed4cf7c262">
<br />
<br />
Before Launching the new VM, configure Virtual Box settings. In the Advanced Tab, set the Shared Clipboard and Drag'n'Drop to Bidirectional <br/>
<img width="900" height="450" alt="DC VM Settings Bidirectional" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/1e39442e-2906-4739-9192-a900c8149d95">
<br />
<br />
Next, navigate to the Network settings. The default Adapter 1 will be attached to NAT. Enable Network Adapter 2 and Attach it to Internal Network. <br/>
<img width="900" height="450" alt="DC VM Network configuration" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/724d003c-72ce-444b-ab71-dd79c45b1251">
<br />
<br />
Boot the DC and run the Server2019 set up on start up. Be sure to select desktop experience for the GUI. Set up loginb credentials <br/>
<img width="900" height="450" alt="Windows2019 VM Set Up Screen 1" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/99267a05-24d8-4c43-928a-4acf2c104a7e">
<img width="900" height="450" alt="Windows2019 VM Set Up Screen 2 Desktop Experience" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/93d09cf7-dbcf-4b99-a1ae-6b3bffd338b6">
<img width="900" height="450" alt="Windows2019 VM Set Up Screen 3 Login Creds" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/132cc73c-47c5-43db-a9a4-a75d1a5df99f">
<br />
<br />
The VM will restart multiple times during the OS configuration. Once things are completed, you will be met with a Windows login screen. Use Input ctrl+alt+delete to login with the credentials that you created during the OS configuiration. <br/>
<img width="900" height="450" alt="Windows2019 VM Login Screen Input" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/03e087ca-6779-4de3-9ef1-6e158c26e0b4">
<br />
<br />
Once logged in, the first step will be to select 'Insert Guest Additions CD image' from the Devices dropdown. This will improve QoL for VM use.
The guest addition can be found in file explorer on the D: Drive, then select the amd64 additions. Complete the guided process and install. Then restart the VM. <br/>
<img width="900" height="450" alt="Virtual Box Guest Additions" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/8805fa19-06e2-460b-9abe-40198b6c392c">
<br />
<br />

### Network Configuration
Now to configure the 2 NICs for this DC. First, Navigate to Network Connections (Control Panel > Network and Internet > Network Connections). Right click either of the networks, select Status, Details, 

- One of the networks should have an IP address resolved by your home router, this is the one that we will label as internet. <br/>
- The other network should have an IP address that was automatically assigned to it after it was unable to resolve an automatic IP assignment from a D1HCP.
<img width="900" height="900" alt="Windows2019 VM Network Setup 3" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/21a8bd9a-9741-4aab-b549-eabbe952a969">
<img width="900" height="900" alt="Windows2019 VM Network Setup 4" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/a10be33e-cf00-4be6-9107-deb08f09b7fc">
<br />
<br />
Note: Rename the external network something like "_INTERNET_" and the internal network something like "X_internal_X", we want to avoid confusing the two when we configure IP addressing. Rename by right clicking and selecting Rename.
<br />
<br />
Now we will set up IP Addressing, right click on the internal network, select properties, select IPv4, then we will assign an IP address, Subnet mask, and the loopback address for the preferred DNS Server. <br \>

- Note: you could also put in the same IP address for the preferred DNS server, instead of the loopback address. <br \>
<img width="900" height="450" alt="Windows2019 VM Network Setup 5 Assign IP to Internal" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/90c19df2-c31d-40ba-8beb-c29fd4b53bec">
<br />
<br />
Next we will rename this PC by right clicking on the start menu and selecting system, then select Rename this PC from the settings menu. We will rename the PC to DC for Domain Controller, then restart the VM. <br />
<img width="896" alt="Windows 2019 Rename PC to Domain Controller" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6c73b7b6-8d6d-4dd6-928c-18f227f092e3">
<br />
<br />
Now that we have our PC renamed and the NICs configured, the next step will be to install Active Directory Domain Services (AD DS) and create a Domain.

First we click on 2. Add roles and features on the Dashboard of Server Manager <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 1" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/5d611984-a4ab-44e9-bb1b-aefd019719fb">
<br />
<br />
Select Next through the next steps of the wizard, notice that the server it points to is the DC (the only server) <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 2 Server Select" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/a5397e41-cfd2-4b0a-9c9d-b6e2c157353e">
<br />
<br />
From the Server Roles we are going to select Active Directory Domain Services by clicking the check box. This prompts a confirmation window, we will select Add Services without making any changes.<br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 3 AD DS Install" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/3fe31545-028a-4332-a483-d92d7ea78eaf">
<br />
<br />
Select Next through the following steps of the wizard, and then select Install. <br />
<br />  
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 4 AD DS Installation Complete" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/f24e40bd-3e28-411e-8fc1-fd91b55cc867">
Once Installation is complete, we can close the wizard and run the Post Configuration Deployment from the alert upper right corner. <br />
<br />
<br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 5 AD DS Post Deployment Configruation" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/ec56ce6c-37d5-48f8-914e-5898d97d363b">
In the Deployment Configuration Wizard, we will now select the Add a new forest Radio Button. Name the domain anything that you want, I chose mydomain.com <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 6 Deployment Configuration Add New Forest" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/1c50b91c-0e87-4082-94ba-bfcef0db930c">
<br />
<br />
Next we will create a password for the Directory Service Restore Mode (DSMR). Select Next through the rest of the wizard and then run the Install. Once installation is complete, the VM will restart.    <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 6 Deployment Configuration Credentials Creation" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/148a4cf8-c557-447d-8760-c3aec8bf0aba">
<br />
<br />
Once the VM has restarted after successfully installing the Post Configuration Deployment, we can log back in and see that there is now an Administrator account for our Domain and a user account. Now, we are going to log in to the Administrator account. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Set Up 8 New Login Page" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/5489a92e-629b-456d-bbb3-addfd392f984">
<br />
<br />
Now we are going to set up our Admin user account. We will begin by selecting the Windows Administrative Tools from the Start Menu, then select Active Directory Users and Computers. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Admin Account 1 AD Users   Computers" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6b3a13a1-d46a-4999-9283-98f9a935312d">
<br />
<br />
In Active Directory Users and Computers we can expand mydomain.com. <br />
- Select mydomain.com, right click, select New > Organizational Unit. We are going to name it _ADMINS. <br />
<img width="900" heihgt="450" alt="Windows2019 VM AD DS Admin Account 2 New Organizational Unit _ADMINS_" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/d4fdcec9-8cc5-4acf-85c4-fdd4faba21e9">
<br />
<br />
Select our new _ADMINS folder, right click, select New > User. You can name the user whatever you like, I chose my first and last name. For the username I chose the common convention of "a - first initial, last name". Set the password to whatever you like, this is a virtual lab.  <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Admin Account 2 create admin user" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/91b683c2-ff3a-46ab-b7c7-341db9853b3b">
<br />
<br />
Once completed, we select Finish and complete our user account creation. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Admin Account 4 create user account" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/aa111403-e06a-4fcc-962a-03302888e38f">
<br />
<br />
Now we will right click the new user account that we created <br />
- right click & select Properties <br />
- then select the Member Of tab <br />
- click Add <br />
- enter 'Domain Admins' <br />
- select Check Names <br />
- Apply <br />
- then select OK <br />
<img width="900" height="900" alt="Windows2019 VM AD DS Admin Account 5 make user account part of the admins group" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/151da256-bf0d-43cb-979f-2ca6497f3343">
<br />
<br />
Now that we have our new domain admin user account, we can log out of the administrator account, select Other User in the Windows login screen, and then input the login credentials that we just created <br />
<img width="900" height="900" alt="Windows2019 VM AD DS Admin Account 6 Sign in on other user with new admin user" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/51b1753f-976c-4933-863e-bfcb79eb5e25">
<br />
<br />

### Configuring RAS / NAT <br />
Once we are logged in, we can now install RAS/NAT (Remote Access Server / Network Access Translation) so that our Windows 10 client VM can access the internet through the DC via our private internal network.
<br />
<br />
 - Select 2. Add roles and features, then select next through the wizard, select our server, and then we will choose the Server Role for Remote Access.
<br />
<img width="900" height="450" alt="Windows2019 VM AD DS Install RAS   NAT 1 Add roles and features" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/cdec72eb-9301-4ba3-bdf1-6a67a738dde7">
<img width="900" height="450" alt="Windows2019 VM AD DS Enable Remote Access Role" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/dfe028ba-5567-4814-ad5a-0cb900a6cf70">
<br />
<br />
 - Then we are going to select Routing, which will automaticall select RAS for us in Role Services. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Install RAS   NAT 3 Install Routing and RAS" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6b1072db-354f-44c0-ad1a-65fd4f790b3f">
<br />
<br />
- Then we will select Next and run the Install. Once the Role is done installing we will close the menu. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Install RAS   NAT 4 Installation complete" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/fdd058f9-18fc-433d-bfa8-d52e1efefed7">
<br />
<br />
 - Now we want to select Routing & Remote Access from the Tools drop down menu in the top right corner of our Server Manager <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Install RAS   NAT 5 Select Routing and Remote Access from Tools Drop Down" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/bedd8093-a17f-41ee-a3e9-8bc418f7ea72">
<br />
<br />
 - Our next step will be to right click our DC and select Configure and Enable. Now we want to select the Network Address Translation (NAT) Radio Button for Install. <br />
 - Note: this may produce an issue where the next page does not recognize the DC and the public interface will be unable to be selected. The fix is to back all of the way out, relaunch Routing & Remote Access from Tools, and repeat the same install process for NAT, and the issue should be resolved.
<br />
<img width="900" alt="Windows2019 VM AD DS Install RAS   NAT 6 Install NAT" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/67aff394-de3f-405e-bb49-b0f1a22e13bd">
<br />
<br />
 - Now we will select the interface marked _INTERNET_, thene select Finish. <br />
<img width="918" alt="Windows2019 VM AD DS Install RAS   NAT 7 select internet for NAT connection" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/a6805b00-aba6-4d8c-a268-6a2bfa0819c1">
<br />
<br />
Now the icon should be green for our DC, now with RAS & NAT installed <br />
<img width="888" alt="Windows2019 VM AD DS Install RAS   NAT 8 DC (local) displays green with additional configurations" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/238abdbd-a47a-4d03-bc8d-cc37903eec0e">
<br />
<br />

### DHCP Server Set Up on DC <br />
Now on our Domain Controller, we will return to the Server Manager and being our DHCP Server set up by selecting 2. Add roles <br />
 - Note: Our Server Selection now shows or server with an updated name. <br />
 - We will select DHCP from our Server Roles and then we will choose Add Features <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 1 Install DHCP on DC" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/4c28c504-f2fc-401c-be41-ff722112c940"> 
<br />
<br />
Now we will select Next through the wizard and run the install on our DHCP role. Once installatiuon completes we can close wizard. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 1 Installation complete" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/a6b83f3d-76e1-4ef6-848a-4de6d81f6ab5">
<br />
<br />
Now we want to select DHCP from the Tools dropdown in the top right corner. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 3 Open DHCP from Tools" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/0431ab37-e3e0-4451-b74a-2573a1f9fe6f">
<br />
<br />
Now we can set up our scope for automatic IP allocation via our DHCP server. <br />
 - We are going to begin by right clicking IPv4 and selecting View Scope. <br />
 - We are going to name the scope the IP range that we will be using. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 4 Create New Scope for IPv4 and name it the IP range" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/05a6bc83-8b2c-4fa0-8cd7-cf84ce3f32c8">
<br />
<br />
Now we will define the start and end IP address by the range of IP addresses that we will have our DHCP server allocate. <br />
 - we will set the length to 24 (CIDR /24) and the subnet mask to 255.255.255.0 <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 5 Set IP Range and subnet mask CIDR" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/0a2ab5b0-6772-41ea-8e5e-e5fd6e6f254a">
<br />
<br />
We will not be excluding any IP addresses for this lab <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 6 No Exclusions" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/2ce2f975-4b20-4d10-bc2e-37a413a10690">
<br />
<br />
We can choose to select a lease duration here, I will be leaving this default. But if you want to change the lease duration for your home lab, go ahead. <br />
 - A practical example may be to better simulate something like a public cafe wifi network, then you can make it a shorter lease time. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 6 Set Lease time, left default" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/b893073d-78f1-46b9-8ee8-7e608b9109e2">
<br />
<br />
We want to configure our DHCP options now <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 7 Configure DHCP Options" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/c06660f4-941c-44a6-81e0-c624963f0543">
<br />
<br />
With NAT/RAS configured on our DC, our DC will forward client traffic to the internet, and therefore our clients will use this internal NIC of the DC as their default gateway & router. So we will set the IP of the DC as our default gateway. <br />
 - IMPORTANT: Be sure to select Add before selecting Next. The data entered in the IP address field will not be automatically added if you hit Next before Adding.
<br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 8 Add DC IP as DG" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/f7905479-9fae-45b5-88a2-0be0d423bc79">
<br />
<br />
We are using the DC as the DNS. So mydomain.com (or whatever you chose to name the DC) for the Parent Domain. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 9 Windows Domain Name" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/a3a4611e-da50-4a47-bc39-f5188a072833">
<br />
<br />
Skip WINS server and move on to the Next.  <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 10 Skip WINS" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/f508cfda-bcb8-4b1b-9236-e14d976b48f4">
<br />
<br />
Now we can activate the scope. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 11 Activate the Scope" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/310a5413-d9ab-4fbb-a746-8e73ff4db5a1">
<br />
<br />
Now authorize the DC by right clicking and then selecting authorize, then refresh it by right clicking and selecting refresh. <br />
 - Now our icons should show as green. <br />
<img width="900" height="450" alt="Windows2019 VM AD DS DHCP 12 Finish, Authorize DC, Refresh DC" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/ee931363-e590-49e9-98bb-b02abaf422f1">
<br />
<br />
Now we will select 1. Configure this Local Server and turn off IE Enhanced Security Configuration. Now we can browse without constant confirmations.  <br />
<img width="900" height="450" alt="Windows2019 VM AD DS Internet access to DC 2 Turn off Internet Explorer Enhanced Security Config" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/811b12fb-bef6-49ad-a96c-5b43b0459fec">
<br />
<br />

### Powershill Script to generate Users  <br />
<br />
<br />
Now we are going to take a powershell script from online to generate users. I recommend saving the script to your desktop. <br />
<img width="900" height="450" alt="Windows Server 2019 Powershell Script 1 Download Script zip file" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/64a51b19-3cb3-4da3-918e-323c83073fef">
<br />
<br />
Open the folder and then open the txt file called names <br />
 - This txt file contains hundreds of randomly generated names <br />
 - We are going to manually enter your name, or whichever name that you used for the admin user account, to the top of  the names list <br />
<img width="900" height="450" alt="Windows Server 2019 Powershell Script 2 Open file and open name list and add your name to top" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/395c5f69-011d-4fa0-947f-5dffb04ef5d5">
<br />
<br />
Now we will search for Windows Powershell ISE from the start menu, then right click and select run as administrator 
<img width="900" height="450" alt="Windows Server 2019 Powershell Script 3 Now Run Windows PowerShell ISE as administrator" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/3b7ea09b-8426-4d4e-aa00-6cc653018698">
<br />
<br />
From within Powershell we will open the 1_CREATE_USERS powershell script from the AD_PS-master folder on our desktop
<img width="900" height ="450" alt="Windows Server 2019 Powershell Script 4 Run the 1_CREATE_USERS PS Script" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/d8341621-255b-48d4-8cae-dd45e0f1dc81">
<br />
<br />
Now we want to set our execution policy to unrestricted in our home lab. When prompted, say Yes to All <br />
 - Note: this is insecure outside of our home lab environment and is otherwise not recommended. <br />
<img width="900" height="450" alt="Windows Server 2019 Powershell Script 5 Set execution policy to unrestricted and select Yes to All" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/7ccfa1e0-f7d2-4080-bfc7-c58717b025f7">
<br />
<br />
### Powershell Script Breakdown <br />
Here is a breakdown of how the variables of our powershell script operate: <br />
 - $PASSWORD_FOR_USERS assigns the password of "Password1" for all of our user accounts that our PS Script generates <br />
 - $USER_FIRST_LAST_LIST uses Get-Content to take the names.txt file and loads them into the variable as an array. <br />
 - $password uses ConvertTo-SecureString to take our plain text "Password1" from our variable $PASSWORD_FOR_USERS and forces it into being an object that PS can use as a secure password <br />
  - New-ADOrganizationalUnit creates another organizational unit called _USERS in our Active Directory Users & Computers <br />
  - foreach is operating as a loop in our list of users, with $n being each individual user operating in the list. So our first user will automatically be our own name that we manually entered at the top of our names.txt file. <br />
  - $n.split is splitting the names in our list based on the space between the first and last names, then taking element [0] (first name) and store it in the element $first (hence first name). It will do the same with element [1] and store it in $last (hence last name) <br />
  - Then $username takes the first letter of first and attaches it to the last name, in order to concatenate a list of usernames based on first and last names (set to lowercase). <br />
  - Write-Host is updating us on the creation of the users as the script runs, you can set the background & foreground colors to your preferences for readability.
<br />
  - New-AdUser is taking that same process of manually creating a user in AD and automating it based on the information that is provided by the list of names, with the different fields being correspondant. The ou will be created and our user accounts will be enabled. <br />
<img width="900" height="900" alt="Powershell Script for creating users" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/09f67d42-b6aa-40a4-94ac-629adedc1422">
<br />
<br />
We want to change directories to the file on our desktop and then run the list command to confirm that the file location and integrity. Otherwise, we can run the script with the play button. <br />
<img width="900" height="450" alt="Windows Server 2019 Powershell Script 6 confirm that the names txt file is referenced correctly and list contents" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/ff14f7dc-52c8-4d3d-8739-af99d341a2c1">
<br />
<br />
If prompted with a security check on running the script, then confirm. Now we can just run the script and watch the users generate. <br />
<img width="900" height="900" alt="Windows Server 2019 Powershell Script 7 Run PS and create users" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/c1f4b5bf-b13a-4d6c-aad4-31656928327c">
<br />
<br />
Once our script has finished running, we can now check Active Directory and see that the _USERS folder has been created and has been populated by the names from the names.txt file as users. We can now search yourself up and find the user that you created for yourself (or whichever name that you created). <br />
<img width="900" height ="900" alt="Windows Server 2019 Powershell Script 8 Open AD to view new users and locate any user account" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/6933ad91-0fb1-499d-aa6e-1ab99363e61a">
<br />
<br />
### Now all that remains is for us to create our Windows 10 VM and configure it to our internal NIC. It should get its IP address from the DHCP Server that we set up, which we will verify. <br />
We will want to configure our Windows 10 VM in VirtualBox, give it the same RAM and CPU allocation as our Windows2019 Server. Then name it CLIENT1. <br /> 
<img width="900" height="450" alt="Virtual Box Set Up CLIENT VM using Windows 10 ISO" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/e4155b94-4728-4baa-94bc-2de5af9f8ad1">
<br />
<br />
Select Internal Network in the Network settings section of our Windows 10 VM. <br />
<img width="900" height="450" alt="Windows 10 Network Settings configure to Internal Network NOT NAT" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/51e1b9a4-6fad-48be-a615-6aaeb82beb85">
<br />
<br />
Now we can launch our Windows 10 VM, then walk through the Windows 10. <br />
 - Select I don't have a product key <br />
 - Be sure to select Windows 10 Pro, NOT Windows 10 Home. You cannot join the domain with Windows 10 Home. <br />
 - Accept the EULA. <br />
 - Choose custom install and select the hard drive, since it is empty. <br />
 - The VM will restart multiple times during install, once the user config is prompted we can move on to the next steps. <br />
 - Choose US for location, no internet, continue with limited set up, use for home and anything limited, we only want a local account. <br />
 - Make your local username something simple like user, you don't need a password. <br />
 - Turn off all toggled features and say not now to any offers prompted. <br />
<img width="900" height="450" alt="Windows 10 Installation Choose Windows 10 Pro after selecting I don't have a product key" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/b9b92e9f-540b-4ef2-b664-6e251088b0a0">
<br />
<br />
Now we will have Windows 10 VM operational. Now we can open the command line and use the ipconfig command to see the network status. <br />
<img width="900" height="450" alt="Windows 10 Run ipconfig commqand to confirm the internal network connection worked successfully" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/dcc400d2-fd40-491c-a902-b2245789868d">
<br />
<br /> 
We can also ping the DC by pinging mydomain.com to see that our infrastructure is working <br />
 - We can ping to the default gateway (DC) <br />
 - Our DC is properly using NAT by forwarding it ot the internet <br />
 - Our ping can come back in the infrastructure from the internet as a reply <br />
<img width="900" height="450" alt="Windows 10 ping mydomain com for internal network" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/386ae8ec-42ec-4e79-8ade-fa29120a6e53">
<br />
<br />
Now we can change the name of the Windows 10 VM and join our domain at the same time, by using the Rename this PC (avanced) under System settings. <br />
 - We want to select the Domain radio button, it should be defaulted to the work group. <br />
 - We will just need to log in to the domain using the login credentials that we previously created. <br />
 - This will prompt a restart. <br />
<img width="900" height ="900" alt="Windows 10 changed device name and join the domain" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/0de09199-ffc5-4bb1-8bb6-110542c09287">
<br />
<img width="900" height="900" alt="Windows 10 successfully joined domain" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/1a285e6d-2df3-439d-b929-ed56c768f2dd">
<br />
Now we can navigate to our Domain Controller and open DHCP to see that our CLIENT1 VM has been assigned an IP address using our DHCP server (in the Address Leases container) <br />
<img width="900" height="900" alt="Windows 10 successfully leased ip address on Windows Server 2019 DHCP" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/7b8656e7-6077-4fdd-bd66-8950c4ff6949">
<br />
<br />
We can also now find the CLIENT1 VM as a Computer within Active Directory Users and Computers (in the Computers container) <br />
<img width="900" height="900" alt="Windows 10 VM shown as a Computer in Windows Server 2019 AD" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/bb0f2611-0f2e-482b-a506-814b42d6d7a5">
<br />
<br />
Now back on our CLIENT1 Windows 10 VM, we can select Other user on the login screen. We can now login to this Other User account with login credentials that we generated with our PS script. <br />
<img width="900" height="900" alt="Windows 10 login as a different user into MYDOMAIN than the local user for CLIENT1 Windows 10 VM" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/2b9c0d21-db99-4c63-b6ed-4bfaeddee6a7">
<br />
<br />
We have now successfully logged into our domain as a user account that we generated through our PS script and can now operate as a user on this seperate VM. <br />
<img width="900" height="450" alt="Windows 10 VM CLIENT1 Successfully accessed mydomain com as jbethea user" src="https://github.com/JABethea/ActiveDirectoryLab/assets/68124261/911d2ff3-9f94-4b06-ba5b-da8f4f0ae2ad">
<br />
<br />

### Sources <br /> 
Josh Madakor's tutorial is where I sourced this Lab Project from. I also use his Powershell Script. <br />
https://www.youtube.com/watch?v=MHsI8hJmggI <br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
