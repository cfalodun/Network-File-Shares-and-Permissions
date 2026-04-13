# Network File Shares & Permission Management

## Project Summary

This project demonstrates how file sharing and access control are managed in an Active Directory environment using both share permissions and NTFS permissions.

Multiple shared folders were created with different access levels, and user access was tested from a client machine. A security group was then implemented to control access to a restricted folder, demonstrating role-based access control.

---

## Environment & Tools

### Environment
- Microsoft Azure Virtual Machines
- Windows Server (Domain Controller – DC-1)
- Windows 10 (Client-1)

### Technologies Used
- Active Directory
- NTFS Permissions
- Windows File Services

---

## Demonstration

# Part 1: Create Shared Folders

## Step 1: Create Folder Structure

Log into **DC-1** as:

```
mydomain.com\jane_admin
```

On the C:\ drive, create the following folders:

- `read-access`
- `write-access`
- `no-access`
- `accounting`

<img src="https://i.postimg.cc/qqfH3f6Y/01-dc1-folder-structure-created.png" width="500">

---

## Step 2: Configure Share Permissions

Right-click each folder → **Properties → Sharing → Advanced Sharing → Permissions**

---

### Read-Only Access

Folder: `read-access`  
Group: `Domain Users`  
Permission: **Read**

<img src="https://i.postimg.cc/L5dF1dgw/02-dc1-share-read-access.png" width="500">

---

### Read/Write Access

Folder: `write-access`  
Group: `Domain Users`  
Permission: **Read / Change**

<img src="https://i.postimg.cc/j2VY7VnG/03-dc1-share-write-access.png" width="500">

---

### Restricted Access

Folder: `no-access`  
Group: `Domain Admins`  
Permission: **Read / Change**

(Standard users are not added, so they have no access.)

<img src="https://i.postimg.cc/BbWfKWPR/04-dc1-share-no-access.png" width="500">

---

## Step 3: Configure NTFS Permissions

Right-click each folder → **Properties → Security**

Ensure NTFS permissions align with share permissions:

- `read-access` → Read  
- `write-access` → Modify  
- `no-access` → Only accessible to admins  

> Both Share and NTFS permissions work together. The most restrictive permission always applies.

---

# Part 2: Test Access as a Standard User

## Step 1: Log in as a Normal User

Log into **Client-1** as:

```
mydomain\<someuser>
```

---

## Step 2: Access Shared Folders

Open File Explorer and navigate to:

```
\\DC-1
```

<img src="https://i.postimg.cc/j2VY7VJY/05-client1-access-test-normal-user.png" width="500">

---

## Step 3: Test Permissions

Attempt to interact with each folder:

<img src="https://i.postimg.cc/0jg1wgJq/06-client1-read-vs-write-test.png" width="500">

### Expected Behavior

- `read-access` → Files can be opened but not modified  
- `write-access` → Files can be created and edited  
- `no-access` → Access denied  

This confirms that the configured permissions are working correctly.

---

# Part 3: Role-Based Access with Security Groups

## Step 1: Create Security Group

On **DC-1**, open **Active Directory Users and Computers**.

Create a new group:

- Name: `ACCOUNTANTS`
- Type: Security

<img src="https://i.postimg.cc/zvmYgmRr/07-ad-accountants-group-created.png" width="500">

---

## Step 2: Assign Folder Permissions

Configure access for the `accounting` folder.

### Share Permissions:
- Add `ACCOUNTANTS`
- Grant **Read / Change**

### NTFS Permissions:
- Add `ACCOUNTANTS`
- Grant **Modify**

<img src="https://i.postimg.cc/kGrPbrtC/08-dc1-accounting-share-permissions.png" width="500">

---

## Step 3: Test Access Before Group Membership

On **Client-1**, as a standard user:

```
\\DC-1\accounting
```

Access is denied because the user is not part of the `ACCOUNTANTS` group.

---

## Step 4: Add User to Security Group

On **DC-1**:

- Add `<someuser>` to `ACCOUNTANTS`

Log out and back in on **Client-1** to refresh group membership.

---

## Step 5: Verify Access After Group Assignment

Access the folder again:

```
\\DC-1\accounting
```

<img src="https://i.postimg.cc/HnG1yG7H/09-client1-accounting-access-before-and-after-group.png" width="500">

Access is now granted, confirming that permissions are controlled through group membership.
