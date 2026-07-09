<p align="center">
  <img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/2c068a6c-d82b-4a43-ad32-d9fe559e1837" />
</p>

# Okta-RBAC



### 1) Create Groups in Active Directory

1. In **Active Directory Users and Computers** create the following groups in the **_Groups** folder:
   - Helpdesk
   - Cloud Engineer
  
<p align="center">
  <img width="748" height="522" alt="image" src="https://github.com/user-attachments/assets/f81d29c9-1069-472b-9af7-bb016703343c" />
</p>

---

### 2) User-to-Role Assignment

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

### 4) Access Spawl Investigation

1. Open the **Applications** tab then open **Applications**
2. Open the **SCIM App** then open the **Assignments** tab
3. Click **Assign** then **Assign to People**

<p align="center">
  <img width="688" height="761" alt="image" src="https://github.com/user-attachments/assets/56d25a2f-8e53-4e52-b238-4e0ad048c6fb" />
</p>

4. Find **John Smith** then press **Assign** then **Save and Go Back**

<p align="center">
  <img width="752" height="324" alt="image" src="https://github.com/user-attachments/assets/df8284de-7992-4c3d-9fd9-e1fab4492f39" />
</p>

5. Open the **Reports** tab then go to **System Log**
6. Filter the search by **eventType equals application.user_membership.add** then **Apply Filter**

<p align="center">
  <img width="773" height="432" alt="image" src="https://github.com/user-attachments/assets/2ee7182b-e160-4a23-a7ce-d85b5acc1c04" />
</p>

7. Find the event where **JOhn Smith** was directly added to the **SCIM App**

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

