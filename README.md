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

Once you have configure the Password Policy, We need to link it to the OU's required or link it to the entire Domain.

We do this to ensure the policy settings take effect because the Default Domain Policy Configuration will override the newly created Policy.

Ensure to Enforce the Policy

<img width="1088" height="779" alt="image" src="https://github.com/user-attachments/assets/2796290d-0f8f-4b03-9b8d-1d2a84cfd6fe" />
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

-Now Attempt to Login with Correct Credentials
- This error message appears:

<p> 
<img width="424" height="337" alt="image" src="https://github.com/user-attachments/assets/6882a75d-6571-40ce-aabb-1de3e12f8830" />

</p>







<p>



## Step 6: Login to your osTicket virtual machine

**We're going to simulate the user submitting the ticket to the helpdesk to get their account unlocked.**

- Login to your osTicket VM with your credentials

<img width="993" height="506" alt="image" src="https://github.com/user-attachments/assets/d94bd80c-d3c7-43e1-83e9-0ef20ad96ae5" />



- Submit this ticket as the end user.
- We will go in as an agent to assign to ourselves and resolve this ticket.


<img width="842" height="988" alt="image" src="https://github.com/user-attachments/assets/0d486deb-35cc-45cc-a750-c2be7ed93f30" />

- Our helpdesk technician, John Doe, will login and overview the ticket.

<img width="466" height="598" alt="image" src="https://github.com/user-attachments/assets/5c2554e6-7b3a-460c-9606-7c70af1b6720" />

- John Doe will then send a reply to the user to let them know we're going to take care of them.


<img width="947" height="878" alt="Screenshot 2025-09-12 135530" src="https://github.com/user-attachments/assets/c69e0eea-3127-44f7-b2bd-5d32a4607f2c" />







<p>


## Step 7: Find the account in our Active Directory

**We're going back to our Domain Controller to find the end user's account in our Active Directory.**

- Go to Active Directory Users and Computers on the domain controller.
- Right click mydomain.com > find.
- Type in the end user's name

<img width="479" height="398" alt="Screenshot 2025-09-12 140006" src="https://github.com/user-attachments/assets/936472ce-8071-4a46-8db8-7530df106d47" />



<img width="513" height="510" alt="Screenshot 2025-09-12 140034" src="https://github.com/user-attachments/assets/a4caa2ac-5e55-4447-b0eb-4b1da9bb4eee" />

- Double click their name that's at the bottom.
- Go to the account tab.
- Hit the unlock account checkbox.
- Click Apply > OK


<img width="408" height="534" alt="Screenshot 2025-09-12 140110" src="https://github.com/user-attachments/assets/0e562cf9-7c8f-46d1-be45-ab6b062c81dd" />

- Go back to their name and right click > reset password.
- Type in their new temporary password.

<img width="376" height="253" alt="Screenshot 2025-09-12 140206" src="https://github.com/user-attachments/assets/416428ac-b6bd-46f2-b997-70d9d12d3cf1" />


<img width="325" height="146" alt="Screenshot 2025-09-12 140219" src="https://github.com/user-attachments/assets/b33fab52-4cd4-4d97-95c6-80a59a68e721" />







<p>





## Step 8: Resolve the Ticket

**We're going to reply to the end user that their account has been fixed and resolve the ticket.**

- Go back and reply to the end user that you have unlock their account and resetted their password.
- Press the "resolved" in the dropdown of Ticket status:
- Hit Post reply

<img width="950" height="999" alt="Screenshot 2025-09-12 141537" src="https://github.com/user-attachments/assets/608d9067-95a9-47b1-8986-c46c0cab8163" />






<p>

## Step 9: Attempt to login to Client-1 as the end user with the right password

**We're going to login to Client-1 as the end user with the right password this time.**

- Login to Client-1 as the end user
- They should now be able to login to their machine

  
<img width="973" height="574" alt="Screenshot 2025-09-12 142126" src="https://github.com/user-attachments/assets/1550a5af-66de-4720-b688-ee20c22ff849" />

- If you access Powershell, you can see that, we're logged into our Client-1 machine as beb.vov!

<img width="935" height="541" alt="image" src="https://github.com/user-attachments/assets/24237459-0921-4aec-b3bf-3f05ad15ec8d" />



## I hope this tutorial could represent Helpdesk flows and my experience with working with different tools used as a helpdesk professional!

<p>







