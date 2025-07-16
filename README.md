## 🏢 Active Directory Lab – Installation & Configuration

---

Objective: Learn how to install and configure Active Directory on a Windows Server EC2 instance. Tasks include installing DNS, creating Organizational Units (OUs), users, groups, group policies, and adding a client machine to the domain.

---

## 📌 Overview
This lab walks through setting up a basic Active Directory environment on AWS. You'll:

Install and configure DNS and AD DS roles

Create OUs, users, and groups

Apply Group Policy Objects (GPOs)

Add a client machine to the domain

Verify policy application and assign admin rights

---

## 🛠️ Task 1: Update AWS Security Group (Firewall)
Go to EC2 > Instances and select your Windows Server instance.

Under the Networking tab, locate your VPC ID and copy the IPv4 CIDR.

Go to the Security tab > Security groups > Edit inbound rules.

Add rule:

Type: All traffic

Source: Your copied IPv4 CIDR

Click Save rules.

---

## 📡 Task 2: Install DNS & Active Directory Roles
Log into Windows Server as Administrator.

Open Server Manager > Manage > Add Roles and Features.

Select:

DNS Server

Active Directory Domain Services (AD DS)

Click Next through prompts.

Click Install.

---

## 🌐 Task 3: Configure DNS for AD
Open Tools > DNS from Server Manager.

Create new zone:

Zone Type: Primary

Zone Name: yourcppusername.local

Allow nonsecure and secure updates

Go to DNS > Properties:

Interfaces tab: Uncheck IPv6

Forwarders tab: Use IP from ipconfig /all

Update network adapter DNS settings:

Preferred DNS: Set to local server IP

---

## 🌳 Task 4: Promote Server to Domain Controller
In Server Manager, click the flag icon > “Promote this server to a domain controller”.

Select:

Add a new forest

Root domain: yourcppusername.local

Use password: Password123

Ignore DNS delegation warning.

Wait for server reboot.

---

## 👥 Task 5: Create AD Objects (OUs, Users, Groups)
Open AD Users and Computers:

Set Administrator password to never expire

Create OUs:

Employees, Groups, Workstations

Create Users:

User One → user1

User Two → user2

Create Groups:

All Company: user1, user2

IT: user1

Marketing: user2

---

## 🧩 Task 6: Create Group Policy (GPO)
Open Group Policy Management.

Right-click Workstations OU → Create GPO → Name: Computer Settings

Edit GPO:

Navigate to:
Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options

Set:

Message title: “Welcome to my domain”

Message text: “You are logging into the yourcppusername.local domain!”

---

## 🖥️ Task 7: Add Client Computer to Domain
Launch a new EC2 instance:

Name: Windows Client

OS: Windows Server 2025

Instance type: t3.small

Rename PC: yourcppusername-client

Update network settings:

Preferred DNS: Local server IP

Join Domain: yourcppusername.local

Confirm successful domain join.

On the server:

Move client to Workstations OU

---

## 🔐 Task 8: Assign Admin Rights to Domain Users
Log into client using domain admin credentials.

Open Computer Management > Local Users and Groups > Administrators.

Add:

user1, user2

Log in as user1 using domain credentials.

Open PowerShell as Admin.

Run:

``` bash
Copy
Edit
gpresult /R

```

---
