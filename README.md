<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring GPO & Unlocking Users Accounts/Resetting Passwords</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Powershell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2> Steps </h2>

- Configuring the Account Lockout & Passowrd Policy
- 

<h2></h2>

<h2> Configuring the Account Lockout Policy </h2>

**Logging in to our DC-1 (Domain Controller) Virtual Machine**

<p> 
  
  - Log into your domain controller as Jane Admin.
  - Right click Start Menu > open Group Policy Management
    - Or press win+r to open run command, type & run "gpmc.msc"
  - Expand the Forest: mydomain.com > expand folder: Group Policy Objects
  - Take note: There is no password policy in effect.

</p>

<p> 

  <img width="318" height="670" alt="image" src="https://github.com/user-attachments/assets/449c59a4-6f10-4f09-98fe-fc0a8e3457d5" /> <img width="356" height="181" alt="image" src="https://github.com/user-attachments/assets/5b2fc3c7-70c6-4c4c-bd23-a3cec79dc9aa" />

  <img width="983" height="451" alt="image" src="https://github.com/user-attachments/assets/de4c4126-1b51-443a-8cc5-c55cccc4fdb1" />

</p>

<h2></h2>

<p> 

<h3> Create/ Edit a Password policy </h3> 

  - Right click on folder: Group Policy Objects > select New > name policy "Password Policy" > OK.
      - OR Edit "Default Domain Policy"
  - Right click on the policy > select Edit > Computer Configuration > Policies > Windows setting > Security Settings > Account Polices
  - There you will see both "Password Policy" & "Account Lockout Policy". Both Not Defined


</p>

</br>

<p> 
<img width="453" height="236" alt="image" src="https://github.com/user-attachments/assets/5d45dcb9-9514-4547-91b1-3ce51e6b0af4" />
<img width="825" height="634" alt="image" src="https://github.com/user-attachments/assets/6ab98a00-60ff-4b6a-94e0-75dbd88aeff6" />


</p>

<h2></h2>

<P> 

Here you can set password parameters to match organizational standards
  - Double click on the policy you want to update > Check the box for " Define this policy setting" to gain edit access.
  - Set parameters > apply and save

</P>

</br>

<p> 
<img width="828" height="603" alt="image" src="https://github.com/user-attachments/assets/d59d027f-0d69-4245-b308-46c41d8d8175" />

</p>
<p> 

<h2></h2>

**If you have created a new Group Policy**

Once you have configure the Password Policy, We need to link it to the OU's required. preferably link it to the entire Domain.

We do this to ensure the policy settings take effect because the Default Domain Policy Configuration will override the newly created Policy.

Ensure to Enforce the Policy

  - Right Click "mydomain.com" > Link an Existing GPO
  - Select "Password Policy" GPO Created
  - Update Enforce Rule to 'yes'

<img width="801" height="416" alt="image" src="https://github.com/user-attachments/assets/a9da0be3-87fa-4494-a8c6-d223ab61d933" />

</p>

<p>

<h2></h2>

 <h3> Force Group Policy Update </h3> 

<P> 
  
**Access our Client-1 machine to force the group policy update via the command line.**

- Login to your Client-1 machine as Jane Admin.
- In the Search bar, look up Command Prompt.
- Right click to run as administrator.
- Type gpupdate /force
- This will update the Group Policy.
- Restart Client-1 VM


</P>

<P> 
  
<img width="505" height="223" alt="image" src="https://github.com/user-attachments/assets/7aabc8bb-139a-4b94-bd2b-7599ca0cba2b" />

</P>

<h2></h2>

<h3> Test Group Policy is Active </h3>

</p>

**We're going to lockout an account on purpose.**

- Pick a radom user from your Active Directory (DC-1).
- Type in the password incorrectly according to policy limits 
- This should lockout the account.
  
</p>

<p> 
<img width="424" height="337" alt="image" src="https://github.com/user-attachments/assets/6882a75d-6571-40ce-aabb-1de3e12f8830" />

</p>

- This error message appears:

<p> 
<img width="557" height="149" alt="image" src="https://github.com/user-attachments/assets/4a3bd7a4-eb4a-45e8-a297-03996231b7a5" />

</p>

<h2></h2>

<p> 

**Here we can simulate that the employee created a helpdesk ticket requesting a password reset as they have been locked out of their account.**

To action this ticket request we use Active Directory User Management
  - Go to _EMPLOYEES OU
  - Search employee name
  - Right click on name > Select "Reset Password"
  - Create a new generic password
  - Select: User must change password at next login
  - select: Unlock the user's account > ok

<img width="879" height="667" alt="image" src="https://github.com/user-attachments/assets/8781e683-1d15-4f34-b827-c5072e9a0781" />
<img width="377" height="257" alt="image" src="https://github.com/user-attachments/assets/48516281-7b33-42ef-9073-b8f15e8fe6f4" /><img width="353" height="148" alt="Screenshot 2025-10-07 153713" src="https://github.com/user-attachments/assets/5e3cb89f-66f6-4db7-ad17-b4cbed084134" />


**Inform the end user that their account has been unlocked, provide the new generic password and resolve the ticket.**

Once unlocked you should now have access to the account

<img width="452" height="140" alt="image" src="https://github.com/user-attachments/assets/718b62dd-18a6-4491-839f-d9bc4c73edc5" />

</p>

<H2> Diabling/ Emabling an Account</H2>

<p> 

**In the event someone from the company has quit, been fired or is on vacation, The most common security measure taken is to diable that account**

  - Search employee name
  - Right click on name > Select Disable Account

<img width="562" height="214" alt="image" src="https://github.com/user-attachments/assets/478f3978-884c-41b6-8699-9c51aa59c2b2" />
<img width="295" height="149" alt="image" src="https://github.com/user-attachments/assets/a4bf332f-4ffe-47ad-89d3-42e1ff9122d8" />

Notice a down arrow on the user account once it has been diable.

<img width="423" height="111" alt="image" src="https://github.com/user-attachments/assets/72dca25e-b5d4-4e44-9862-3e5ee2003e8e" />


**For the purpose of a vacation, once the person has returned you take the same steps and re-enable the account**

<img width="529" height="121" alt="image" src="https://github.com/user-attachments/assets/bcb293d7-b790-4ce4-b45a-596920baeb41" />
<img width="288" height="143" alt="image" src="https://github.com/user-attachments/assets/7df316e2-d9f0-4a4d-a26c-3de01785eaf4" />


</p>

<h2> User Account Logs</h2>

<p> 

**In the event of any suspicious log in activities, one can view them via "Event Viewer"

  - Within DC-1, search from windows menu: Event Viewer
  - OR
  - Within Clint-1, search from windows menu: Event Viewer and run as Admin (for a more refined search) > log in as admin
  - Select: Windows logs > Security


<img width="433" height="457" alt="image" src="https://github.com/user-attachments/assets/831074e9-bc31-422d-b191-dc5043fb74b8" />

From here you will be able to search the log for any events

For example: highligting the failed log in attempts that prompted a password reset and account unlock. This can also be quickly identified by the event ID 4625

<img width="1564" height="985" alt="image" src="https://github.com/user-attachments/assets/882c3ad6-9688-46e8-b277-29c3b3ba52ec" />


</p>



<H2>This Concludes The Presentation</H2>


