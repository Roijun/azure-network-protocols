
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

In this walkthrough we're going to be creating a list users within our domain, and then go on to manage some of those users.

<h2>Active Directory</h2>

<h3>Creating and Managing Users</h3>

- Create a bunch of additional users and attempt to log into client-1 with one of the users

We're going to create our clients on our domain controller virtual machine, so we'll want to log in using our jane_admin account. Once in the machine we'll want to run PowerShell_ise as an administrator. Once in the Powershell application we'll want to create a new file and paste the contents of the script listed below.

https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

![script](https://github.com/user-attachments/assets/f351dc2e-b2ba-4bc6-8fae-4d8ac87ecb8a)

After successfully pasting we want to run the script and observe the accounts being created. Once the script is done running we can view our new users from the Active Directory Users and Computers window inside of the _EMPLOYEES folder that we created. I slightly changed the script to create 1,000 users instead of the default 10,000 that the script uses to make things a bit faster. Once we have all of our users created we can go ahead and try to log into our client machine with one of our new users. From the domain controller machine we're going to open the _EMPLOYEES folder and take note of one of the usernames of our users. All the passwords should be the same, set as 'Password1' from within the script. We'll try and login to make sure that we did everything correctly. Make sure to have our domain name in front of our created username. Once we confirm that we can login as one of our users we are going to log back off so we can continue with the next steps.

- Dealing with Account Lockouts

Account lockouts are a typical IT problem that come across the help desk territory. We'll be purposefully locking out our user account that we just used to sign in so we can understand the process of unlocking an account. Before we attempt that we need to set the lockout protocol, as by defualt there isn't one set in place. We need to click on start in the bottom left, and search for 'Group Policy Management'. Once in the window we can choose to edit the already existing default domain policy by right clicking it and selecting 'Edit'. 

![gpm](https://github.com/user-attachments/assets/b0676ed5-5e44-4abd-bdc8-d59e29b28a28)

To navigate to the account lockout policy section we'll want to follow the following dropdown menu pathway, Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > and finally > Account Lockout Policy.

![GPMedit](https://github.com/user-attachments/assets/d6aeaeb7-ea52-4527-a451-7606295511f3)

I'm going to set the account lockout threshold to 5 and hit ok. It'll automatically update the lockout duration and the reset account lockout counter for us after entering our lockout threshold. Now we can continue with locking out our users account. All we have to do is type in an incorrect password with our users username 5 times before we successfully lock out our account. On our 6th attempt to log into the system we should be prompted with the following message.

![lockedout](https://github.com/user-attachments/assets/8dd21410-e772-45da-a460-5897983c319a)

Now we can switch on over to our domain controller machine to view from the Active Directory window that the users account is locked out. From the _EMPLOYEES folder we want to right click on our locked out user and select properties. Within the 'Account' tab we can see that their account is locked.

![balcuw](https://github.com/user-attachments/assets/f38b843b-9c37-4e01-a007-6ec71e336503)

To unlock the users account we simply have to check the box and hit apply. If this were a real world scenario we would also have them reset thier password upon their next login. After resetting we also want to make another attempt at logging in with the user, just to make sure that the user can still log in, and that we did everything correctly.

- Enabling and Disabling Accounts

Enabling and disabling accounts is easier than the previous step where we had to configure the lockout policy. We'll use the same user which we just locked out and resetted for this portion as well. To disable a users account we just need to navigate to that users account, either by scrolling through the _EMPLOYEES folder or by right clicking it and selecing the 'Find' option. Once you find that account that you want to disable all you have to do is right click their name and select 'Disable Account'. You'll be prompted with a window notifying you that the account has been disabled.

![disabled](https://github.com/user-attachments/assets/8c8abeaf-6faf-4511-90fb-4db78994ea37)

Now that we've disabled our trusty user here, we'll want to attempt to login once more as them to ensure that our account has truly been disabled. We should be prompted with the following message.

![disabledaccount](https://github.com/user-attachments/assets/f9b81ab1-e35d-4038-865f-71b79278e7bb)

We'll keep things consistent and re-enable our account, and then proceed to attempt another login in their name. Once confirmed that your user still has access we should be done with this portion of the walkthrough dealing with enabling and disabling eccounts.

- Observing Logs

Now that we've completed the main portion of the walkthrough and tested with various aspects of account accessibility, the only thing we have left to do is to view our active directory logs showing a portion of the process which we just went through. To find our logs we're going to use our client-1 machine and search in the bottom left for our 'eventvwr'. Run it as an administrator, once inside the eventvwr window we'll have to use our jane admin credentials to allow us access to the logs. Next we want to open the dropdown menu for 'Windows Logs', and select 'Security'.

![eventvwr](https://github.com/user-attachments/assets/278d4bd6-f040-4364-bfcc-27ab5167c64c)

If we right click our security log and click on 'Find' we can search using our users name to find all logs associated with that name. It's an easy way to find what we're looking for. 

![auditfailure](https://github.com/user-attachments/assets/464e456c-048d-44e7-b8b1-b53d1c069bc0)

We'll hit next until we come across a series of audit failures back to back under the name of our user. These represent our earlier actions when we were attempting to lock out our account by incorrectly typing in our password over and over. The logs within eventvwr can provide a lot of insight to employees who are potentially trying to solve IT related issues. 

![auditfailure](https://github.com/user-attachments/assets/e2cf94f6-9714-4761-8d49-02f631ea03f0)

This concludes our walkthrough navigating the Active Directory system. If you've never dealt with active directory before this guide should have at least built a foundational layer of familiarity and understanding around what active directory does and why it's an important tool for IT professionals. 








