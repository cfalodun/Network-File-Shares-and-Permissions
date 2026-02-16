# <p align="center">Network File Shares & Permission Management

## Objective

Create shared folders with varying permission levels, test access as standard users, and implement security group-based access control within an Active Directory environment.

This lab demonstrates:

- Share-level permissions
- Role-based access control
- Security group configuration
- Access validation from client machines

---

# Part 1 – Create Shared Folders with Permissions

Logged into **DC-1** as:

mydomain.com\jane_admin

Created the following folders on C:\:

- read-access
- write-access
- no-access
- accounting

<img src="https://i.postimg.cc/qqfH3f6Y/01-dc1-folder-structure-created.png" width="500">

---

## Configure Share Permissions

### Read-Only Access

Folder: read-access  
Group: Domain Users  
Permission: Read

<img src="https://i.postimg.cc/L5dF1dgw/02-dc1-share-read-access.png" width="500">

---

### Read/Write Access

Folder: write-access  
Group: Domain Users  
Permission: Read/Write

<img src="https://i.postimg.cc/j2VY7VnG/03-dc1-share-write-access.png" width="500">

---

### No Access for Standard Users

Folder: no-access  
Group: Domain Admins  
Permission: Read/Write  

(Standard users do not have access.)

<img src="https://i.postimg.cc/BbWfKWPR/04-dc1-share-no-access.png" width="500">

---

# Part 2 – Test Access as Normal User

Logged into **Client-1** as:

mydomain\<someuser>

Accessed shared directory:

\\DC-1

<img src="https://i.postimg.cc/j2VY7VJY/05-client1-access-test-normal-user.png" width="500">

Tested read vs write capabilities.

<img src="https://i.postimg.cc/0jg1wgJq/06-client1-read-vs-write-test.png" width="500">

### Observations

- read-access → Can open, cannot modify  
- write-access → Can create and modify files  
- no-access → Access denied  

Behavior matched assigned permissions.

---

# Part 3 – Implement Security Group-Based Access (ACCOUNTANTS)

## Create Security Group

On DC-1:

Created security group:

ACCOUNTANTS

<img src="https://i.postimg.cc/zvmYgmRr/07-ad-accountants-group-created.png" width="500">

---

## Assign Folder Permissions

Folder: accounting  
Group: ACCOUNTANTS  
Permission: Read/Write

<img src="https://i.postimg.cc/kGrPbrtC/08-dc1-accounting-share-permissions.png" width="500">

---

## Test Access Before Group Membership

On Client-1 as <someuser>:

Attempted to access:

\\DC-1\accounting

Access denied (user not in ACCOUNTANTS group).

---

## Add User to Security Group

On DC-1:

Added <someuser> to ACCOUNTANTS group.

Logged out and back in on Client-1.

Accessed accounting share again.

<img src="https://i.postimg.cc/HnG1yG7H/09-client1-accounting-access-before-and-after-group.png" width="500">

Access granted.

---

# Skills Demonstrated

- Share-level permission configuration
- NTFS permission alignment
- Role-based access control (RBAC)
- Security group management
- Client-side access validation
- Domain authentication behavior

---

# Result

Successfully implemented structured file share permissions and validated group-based access control in an Active Directory environment.

Demonstrated practical understanding of enterprise file share security management.
