<p align="center">
  <img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/2c068a6c-d82b-4a43-ad32-d9fe559e1837" />
</p>

# Okta RBAC Design & Implementation

This project demonstrates the design and implementation of a Role-Based 
Access Control (RBAC) model using Active Directory as the identity source 
and Okta as the access policy layer. Building on the Active Directory 
integration established in the previous lab, this lab walks through creating 
role-based security groups in Active Directory, assigning users to their 
correct role groups, verifying group sync into Okta as directory-linked 
groups, investigating and remediating access sprawl using the Okta System 
Log, and simulating a complete leaver offboarding process. All configurations 
reflect real-world IAM practices around least privilege access, clean RBAC 
pipeline design, and identity lifecycle management where Active Directory 
remains the single source of truth for both identity and access decisions.

---

## Prerequisites

This is the third lab of the [Okta IAM Lab Series](https://github.com/RyanKennon/Okta-Lab-Series/tree/main). 
Complete all previous labs before starting this one.

The following should be in place before starting:

- Active Okta trial org with users, groups, and policies configured from Labs 1 and 2
- Windows Server 2022 VM running in Microsoft Azure with Active Directory configured
- The following should already exist in Active Directory:
  - `_Users`, `_Groups`, `_Admins`, `_ServiceAccounts`, and `_DisabledUsers` OUs created
  - John Smith, Jane Doe, Bob Johnson, and Sarah Connor created in the `_Users` OU
  - Finance, IT, Human Resources, and Kennon Technologies Employees groups created in the `_Groups` OU
  - Users assigned to their correct role groups
- The following should already exist in Okta:
  - Active Directory integration configured and synced
  - All users imported and active
  - SCIM 2.0 Test App added as an application
 
---

## Environments and Technologies Used

- Okta Identity Cloud (Trial Org)
- Okta Admin Console
- Okta Universal Directory
- Okta AD Agent
- Okta System Log
- Microsoft Azure
- Windows Server 2022
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Remote Desktop Protocol (RDP)

---

## Table of Contents

- [1) Create Groups in Active Directory](#1-create-groups-in-active-directory)
- [2) User-to-Role Assignment](#2-user-to-role-assignment)
- [3) Okta Group Sync Verification](#3-okta-group-sync-verification)
- [4) Access Sprawl Investigation](#4-access-sprawl-investigation)
- [5) Leaver Offboarding & Access Revocation](#5-leaver-offboarding--access-revocation)

---

### 1) Create Groups in Active Directory

Role-based security groups in Active Directory serve as the foundation of 
the RBAC model. Creating dedicated groups for each job function ensures that 
access decisions are driven by role membership rather than individual user 
assignments, establishing a clean and scalable access control pipeline.

1. In **Active Directory Users and Computers** create the following groups in the **_Groups** folder:
   - Helpdesk
   - Cloud Engineer
  
<p align="center">
  <img width="748" height="522" alt="image" src="https://github.com/user-attachments/assets/f81d29c9-1069-472b-9af7-bb016703343c" />
</p>

---

### 2) User-to-Role Assignment

Assigning users to exactly one role group in Active Directory enforces the 
core RBAC principle that access is granted through group membership, not 
direct assignment. Each user's role group determines what applications and 
resources they can access in Okta.

1. In **Active Directory Users and Computers** create the following user in the **_Users** folder:
   - **First Name:** Mike
   - **Last Name:** Taylor
   - **User Logon Name:** mike.taylor
  
2. Create a second user with the following information:
   - **First Name:** Lisa
   - **Last Name:** Park
   - **User Logon Name:** lisa.park
  
<p align="center">
  <img width="749" height="524" alt="image" src="https://github.com/user-attachments/assets/dd6b2780-434e-4158-8de6-bf7865be0c50" />
</p>

3. Add **Mike Taylor** to the **Helpdesk** group and **Lisa Park** to the **Cloud Engineer** group
4. Add both **Mike Taylor** and **Lisa Park** to the **Kennon Technologies Employees** group

---

### 3) Okta Group Sync Verification

Verifying that AD role groups sync correctly into Okta as directory-linked 
groups confirms that the RBAC pipeline is functioning as intended. This step 
ensures that group membership changes made in Active Directory are 
automatically reflected in Okta without any manual intervention.

1. In **Okta** open the **Directory** tab then **Directory Integrations** then choose your **Directory**
2. Click the **Import** tab then **Import Now**
3. Select **Full Import** then click **Import**

<p align="center">
  <img width="301" height="404" alt="image" src="https://github.com/user-attachments/assets/aa8ee740-7478-4f72-a3d3-24dcd9547639" />
</p>

4. Check **All User Assignments** then **Confirm Assignments**

<p align="center">
  <img width="981" height="574" alt="image" src="https://github.com/user-attachments/assets/c29b93cf-c214-4703-9688-a18a635ee07b" />
</p>

5. Open the **Directory** tab then select **Groups**
6. Open the **Kennon Technologies Employees** group
7. Confirm that **Mike Taylor** and **Lisa Park** appear in the group

<p align="center">
  <img width="999" height="616" alt="image" src="https://github.com/user-attachments/assets/9be8ac9b-24ce-4589-b046-d03d8c7596f5" />
</p>

---

### 4) Access Sprawl Investigation

Access sprawl occurs when users receive direct application assignments outside 
of the group-based RBAC pipeline, creating uncontrolled access that bypasses 
governance controls. This section demonstrates the full remediation cycle — 
intentionally creating a direct app assignment, diagnosing it using the Okta 
System Log, and removing it to restore the clean RBAC pipeline.

1. Open the **Applications** tab then open **Applications**
2. Open the **SCIM App** then open the **Assignments** tab
3. Click **Assign** then **Assign to People**

<p align="center">
  <img width="688" height="761" alt="image" src="https://github.com/user-attachments/assets/56d25a2f-8e53-4e52-b238-4e0ad048c6fb" />
</p>

4. Find **John Smith** then press **Assign** then **Save and Go Back** then **Done**

<p align="center">
  <img width="752" height="324" alt="image" src="https://github.com/user-attachments/assets/df8284de-7992-4c3d-9fd9-e1fab4492f39" />
</p>

5. Open the **Reports** tab then go to **System Log**
6. Filter the search by **eventType equals application.user_membership.add** then **Apply Filter**

<p align="center">
  <img width="773" height="432" alt="image" src="https://github.com/user-attachments/assets/2ee7182b-e160-4a23-a7ce-d85b5acc1c04" />
</p>

7. Find the event where **John Smith** was directly added to the **SCIM App**

<p align="center">
  <img width="962" height="359" alt="image" src="https://github.com/user-attachments/assets/fa3fb8ba-e534-4c5f-9527-a8ff9fadb9f9" />
</p>

8. Go back to the **Applications** tab then **Applications**
9. Open the **SCIM App** then go to the **Assignments** tab
10. Find **John Smith** then click the **X** next to his name then confirm the removal

<p align="center">
  <img width="736" height="357" alt="image" src="https://github.com/user-attachments/assets/9830ef3c-c0f2-456e-b4ca-a5104d83536e" />
</p>

---

### 5) Leaver Offboarding & Access Revocation

The leaver process demonstrates how disabling a user account and removing 
group memberships in Active Directory automatically revokes all access in 
Okta on the next sync. This confirms that Active Directory remains the single 
source of truth for both identity and access, and that no manual Okta changes 
are required to complete an offboarding.

1. In **Active Directory Users and Computers** find **Sarah Connor**
2. Right click her account and select **Disable Account**

<p align="center">
  <img width="746" height="523" alt="image" src="https://github.com/user-attachments/assets/8960cab6-1dd6-47b3-9114-2f4e4f4463b2" />
</p>

3. Right click **Sarah Connor** then **Move** the account to **_DisabledUsers**

<p align="center">
  <img width="749" height="524" alt="image" src="https://github.com/user-attachments/assets/5d6ed17c-942d-4303-b52c-02e07d1e43ff" />
</p>

4. Remove **Sarah Connor** from the **Finance** and the **Kennon Technologies Employees** group

<p align="center">
  <img width="405" height="533" alt="image" src="https://github.com/user-attachments/assets/3daca6ff-9d41-496d-b2b1-b8557d399bfa" />
</p>

5. In **Okta** open the **Directory** tab then **Directory Integrations**
6. Choose your **Directory** then open the **Import** tab
7. Click **Import Now** then run a **Full Import**

<p align="center">
  <img width="293" height="405" alt="image" src="https://github.com/user-attachments/assets/59b107c0-a6e3-44b7-9744-2656296cd977" />
</p>

8. Open the **Directory** tab then **People**
9. Confirm **Sarah Connor** is in a **Deactivated** status

<p align="center">
  <img width="1044" height="612" alt="image" src="https://github.com/user-attachments/assets/f0f71a99-554d-4072-95a1-0b8cb89a8fa7" />
</p>

---

> **Note:** This lab is intentionally left open. The Active Directory 
> environment and Okta org configured here serve as the foundation for 
> all subsequent Okta labs. Continue to the 
> [Okta IAM Lab Series](https://github.com/RyanKennon/Okta-Lab-Series/tree/main) 
> for the next lab in the series.

---

<p align="left">
  <a href="https://github.com/RyanKennon/okta-AD-integration">⬅ Lab 2 — Okta Active Directory Integration</a>
</p>

<p align="right">
  <a href="link-to-lab-4-repo">Lab 4 — Enterprise SSO — SAML 2.0 ➡</a>
</p>
