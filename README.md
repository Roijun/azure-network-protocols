
![acitvedirectory](https://github.com/user-attachments/assets/281aa924-786b-4692-9111-427bb727ea92)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

Have a Microsoft Azure account and have followed my previous steps in the last walkthrough setting up and configuring our domain controller and our client virtual machines.
 
<h2>Overview</h2>

In this walkthrough we're going to be configuring network security groups as well as a variety of other settings while we explore the active directory software.

<h2>Active Directory</h2>

<h3>Installation and Deployment</h3>

We're going to go through and actually install active directory on the domain controller and we're going to create a domain admin for us to use. We're also going to join our client-1 onto the domain. Doing so allows us to use all of the users that exist in the domain to log onto the client-1 machine. 

- Install Active Directory

To install active directory we're going to want to get logged in using our dc-1 machine. Once there hit the start button in the bottom left and go to open the server manager application. Once there we'll want to select 'Add roles and features' from the top of the window. Once in this window we'll want to select next until we reach the 'Server Roles' section. Once here you need to check the 'Active Directory Domain Services' box, and make sure to apply features when prompted.

![server roles](https://github.com/user-attachments/assets/fddd2fc0-f786-4c05-8e78-e240fa9412c0)

We can hit next until we reach the 'Confirmation' section, where we'll make sure to check the box near the top of the window that reads 'Restart the destination server automatically if required'. Once we've done that we can go ahead and hit install and close out the window. 

Next we need to go in and promote dc-1 to the domain controller within the system. We need to open server manager once again, and in the top right you'll want to click on the flag icon and select 'Promote this server to a domain controller'. Once here we'll select the Add a new forest option. We'll make our domain name 'mydomain.com' and hit next. 

![activedirec](https://github.com/user-attachments/assets/ecfc9723-f096-4280-bece-424b12582678)

The next window will prompt you for a password, just make it something you can remember easily. We can hit next until we reach the DNS options page. We want the 'Create DNS Delegation' box to be unchecked. With all that in place we can go ahead and hit next unti we get to the last page, then we can hit install when the system will allow us. Once it's finished installing the computer now becomes a domain controller, and the system will auto restart itself. 

- Create a Domain Admin user within the domain

When signing in back onto the dc-1 system we'll want to make sure we use 'mydomain.com/' followed by our username. We need to specifiy that we are logging in onto the domain, and not just logging in as a local user. After we sign back in we can go ahead and open our active directory. We're going to use the start button in the bottom left and search for 'Active Directory Users and Computers'.

![activedirectory](https://github.com/user-attachments/assets/4bc82bba-8915-459e-921e-19b78cb158e9)

Next we're going to create a new organizational unit called '_EMPLOYEES'. From the ADUC window we want to right click our domain, hover over new, and then finally click organizational unit. We'll create one unit named _EMPLOYEES and other named _ADMINS. 

![employeesadmin](https://github.com/user-attachments/assets/c0a90bf3-ccd5-47c9-93ad-2c8001a33e34)

Next we're going to create a user inside of the _ADMINS OU that we just created. To do this we need to double click the _ADMINS folder, and in the right side of this window right click inside the empty space. Then hover over 'New' before selecting User. We're going to name our user Jane Doe, with a username of jane_admin. Hit the next button and you'll be prompted to give our user a password, just make sure you don't lose it before we need to use it. We normally wouldn't check these in real world scenarios, but we're going to uncheck user must change password on next login, and check password never expires just to make it a bit easier for us.

![passwordprop](https://github.com/user-attachments/assets/5dc6049d-2424-4ee3-b9e7-08ec11f05851)

The next thing we are going to do is to add our user into the domain admin security group. We'll want to right click on Jane, and select properties. From that window go to the 'Member Of' section and click on 'Add'. We'll type in 'domain admin', and click search to look for security groups with the same name association. Once 'Domain Admins' is underlined we can hit okay, then apply, then okay. Jane is now a proud member of the domain admin security group, and has extended permissions. 

![admin](https://github.com/user-attachments/assets/68af3f77-00b3-4df0-aa07-2978c2eed7bf)

We're going to continue with the rest of this walkthrough as our new admin user Jane. We'll want to log out of the account that we are currently accessing dc-1 with and log in as Jane. 

- Join Client-1 to your domain (mydomain.com)

We're going to join our client-1 machine into our domain. To navigate here we'll want to click on start and search or click on system. Once at the system page we'll want to click 'Rename this PC (advanced)'. Under the computer name tab click on change. This window is where we add our client machine into our domain system. Under domain we'll enter 'mydomain.com' and hit okay. It will have you log in as our admin Jane to give you permission to add our client into our domain. Once logged in it will greet you into the domain, and restart to update the network changes.

The last thing we'll do before moving onto the next section is create a new organizational unit for our clients named '_CLIENTS'. After that we're ging to click on the 'Computers' folder and drag our client-1 machine on over to our new folder we just created. 

- Setup Remote Desktop for non-administrative users on Client-1

To start we're going to log in to Client-1 as 'mydomain.com\jane_admin'. Once inside the machine we're going to right click the start button in the bottom left and click on system. From here we need to navigate over to 'Remote Desktop'. 

![remotedesktop](https://github.com/user-attachments/assets/77deb6cc-dce2-4a3b-a480-1c40d499e50e)

Once there we want to scroll to the bottom and click on 'Select users that can remotely access this pc', and then hit 'Add'. We're going to allow all users on our domain to access our client machine, so we're going to type in 'Domain Users' before hiting check names, then okay, and okay once more. You can now log into client-1 as a normal non-administrative user, normally you'd want to do this using group policy.

- Create a bunch of additional users and attempt to log into client-1 with one of the users

We're going to create our clients on our domain controller machine, so we'll want to log in using our jane_admin account. Once in the machine we'll want to run PowerShell_ise as an administrator. Once in the Powershell application we'll want to create a new file and paste the contents of the script listed below.

https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

![script](https://github.com/user-attachments/assets/f351dc2e-b2ba-4bc6-8fae-4d8ac87ecb8a)

After successfully pasting we want to run the script and observe the accounts being created. Once the script is done running we can view our new users from the Active Directory Users and Computers window inside of the _EMPLOYEES folder that we created. I slightly changed the script to create 1,000 users instead of the default 10,000 that the script uses to make things a bit faster. 

- 





