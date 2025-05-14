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



