# Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols

# Lab: Creating File Shares with Permissions on DC-1

## Objective:
In this lab, I learned how to create and manage file shares in Windows Server and set up different access permissions based on security groups. I also tested how those permissions work from a client machine.

---

## Step 1: Log into DC-1 and Client-1

The first thing I did was log into both my **Domain Controller** (`DC-1`) and **Client-1** to start working with file shares and permissions. On `DC-1`, I logged in using my domain admin account (`mydomain.com\jane_admin`), and on `Client-1`, I logged in as a regular user (`mydomain\<someuser>`).

---

## Step 2: Create Folders on DC-1 and Set Permissions

After logging into `DC-1`, I navigated to the **C:** drive and created four folders for the file shares I was going to set up:

1. **"read-access"** – This folder was going to have **Read** access for the **Domain Users** group.
2. **"write-access"** – This folder was set to allow **Read/Write** access for the **Domain Users** group.
3. **"no-access"** – I created this folder with **Read/Write** permissions, but only for the **Domain Admins** group.
4. **"accounting"** – I left this one for later, since I was going to set up a specific security group for it.

I shared these folders and set the following permissions:

- **"read-access"**: **Domain Users** → **Read**
- **"write-access"**: **Domain Users** → **Read/Write**
- **"no-access"**: **Domain Admins** → **Read/Write**
  
By setting these permissions, I was able to control who could access these folders and at what level.

---

## Step 3: Test File Shares Access on Client-1

Once the file shares were set up, I logged into **Client-1** as a regular user (`mydomain\<someuser>`) and tried to access the folders using the **Run** command (`\\dc-1`) to browse the shared folders on `DC-1`.

- **"read-access"**: I was able to access this folder and view its contents, but I couldn’t modify or add anything because the permissions were set to **Read**.
- **"write-access"**: I was able to both view and create files in this folder, as expected, since the permissions were set to **Read/Write**.
- **"no-access"**: This folder didn’t appear for me because it had been restricted to **Domain Admins** only.
  
This made sense because the permissions I configured on the server directly impacted what I could do as a regular user from the client machine.

---

## Step 4: Create and Test the "ACCOUNTANTS" Security Group

Next, I created a security group called **"ACCOUNTANTS"** in Active Directory on `DC-1`. This group was going to have access to the **"accounting"** folder, which I hadn’t set permissions for yet.

1. I created the **"ACCOUNTANTS"** group in **Active Directory Users and Computers**.
2. Then, I went back to the **"accounting"** folder and granted **Read/Write** permissions to the **"ACCOUNTANTS"** group.

At this point, I went back to **Client-1** and logged in as **`<someuser>`** (who wasn’t part of the **"ACCOUNTANTS"** group). I attempted to access the **"accounting"** folder, but as expected, it didn’t work. I was denied access because I hadn’t added the user to the group yet.

---

## Step 5: Add User to "ACCOUNTANTS" Group

On `DC-1`, I added the user **`<someuser>`** to the **"ACCOUNTANTS"** security group. This was done through the **Active Directory Users and Computers** console.

1. I found **`<someuser>`** in the **Users** section and added them to the **"ACCOUNTANTS"** group.
2. Then, I logged out of **Client-1** and logged back in as **`<someuser>`**.

---

## Step 6: Test Access After Group Membership

After logging back into **Client-1**, I navigated to the shared folder path (`\\DC-1\accounting`) and tried to access the **"accounting"** folder again.

This time, it worked! I was able to access the folder and even create files inside it. Since I was now a member of the **"ACCOUNTANTS"** security group, my permissions to access and modify the **"accounting"** folder were granted.

---

## Conclusion:

By completing this lab, I learned how to set up file shares on a Domain Controller and how permissions for shared folders work. I also gained hands-on experience with Active Directory security groups and how adding a user to a group can affect access to resources on the network. Testing access on a client machine helped me confirm that my permissions were set up correctly.

This is just the beginning of understanding how permissions and file sharing are managed in a domain environment, and I’m looking forward to expanding this setup in future labs.

---

### Next Steps:

In the next lab, I’ll explore more advanced Active Directory configurations, such as Group Policy and auditing file share access.

