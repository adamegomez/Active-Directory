## 🏢 Active Directory Lab – Installation & Configuration

---

Objective: Learn how to install and configure Active Directory on a Windows Server EC2 instance. Tasks include installing DNS, creating Organizational Units (OUs), users, groups, group policies, and adding a client machine to the domain.

---

## 📌 Overview
This lab walks through setting up a basic Active Directory environment on AWS. You'll:

  - Install and configure DNS and AD DS roles

  - Create OUs, users, and groups

  - Apply Group Policy Objects (GPOs)

  - Add a client machine to the domain

  - Verify policy application and assign admin rights

---

## 🛠️ Task 1: Update AWS Security Group (Firewall)
1. Go to EC2 > Instances and select your Windows Server instance.

2. Under the Networking tab, locate your VPC ID and copy the IPv4 CIDR.

3. Go to the Security tab > Security groups > Edit inbound rules.

4. Add rule:

    - Type: All traffic

    - Source: Your copied IPv4 CIDR

5. Click Save rules.

---

## 📡 Task 2: Install DNS & Active Directory Roles
1. Log into Windows Server as Administrator.

2. Open Server Manager > Manage > Add Roles and Features.

3. Select:

    - DNS Server

    - Active Directory Domain Services (AD DS)

4. Click Next through prompts.

5. Click Install.

---

## 🌐 Task 3: Configure DNS for AD
1. Open Tools > DNS from Server Manager.

2. Create new zone:

    - Zone Type: Primary

    - Zone Name: yourcppusername.local

    - Allow nonsecure and secure updates

3. Go to DNS > Properties:

    - Interfaces tab: Uncheck IPv6

    - Forwarders tab: Use IP from ipconfig /all

4. Update network adapter DNS settings:

    - Preferred DNS: Set to local server IP

---

## 🌳 Task 4: Promote Server to Domain Controller
1. In Server Manager, click the flag icon > “Promote this server to a domain controller”.

2. Select:

    - Add a new forest

    - Root domain: yourcppusername.local

3. Use password: Password123

3. Ignore DNS delegation warning.

5. Wait for server reboot.

---

## 👥 Task 5: Create AD Objects (OUs, Users, Groups)
1. Open AD Users and Computers:

    - Set Administrator password to never expire

2. Create OUs:

    - Employees, Groups, Workstations

3. Create Users:

    - User One → user1

    - User Two → user2

4. Create Groups:

    - All Company: user1, user2

    - IT: user1

    - Marketing: user2

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
