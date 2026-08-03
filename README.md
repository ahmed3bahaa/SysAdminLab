# SysAdminLab

This repo is my own lab notes for Windows Server and sysadmin practice.

I am using this lab to practice:

- Active Directory users, OUs and domain work
- DNS and DHCP
- WSUS
- Storage, shares and NTFS/security permissions
- GPO
- Windows Server config
- PowerShell and bulk admin tasks
- Normal sysadmin stuff I should know by hand

This is not a clean tutorial. It is more like proof of work: screenshots, short notes, what I tested, and what I need to remember later.

## How I will document

For every lab day I want to add:

- Date
- What I was trying to configure
- Screenshots
- Commands or script idea if there is one
- What worked / what failed
- Small notes for the next time

## 2026-08-03 - first screenshots

First batch is mostly AD, storage, permissions, mapped drive and roaming profile practice. Environment is VMware with Windows Server and client machines. Some names are rough because this is from the working lab, not production naming.

Any passwords visible here are only throwaway lab values.

### 1. VMware network config

Set the VM network adapter to a custom VMnet so the domain controller, servers and clients can stay inside the lab network.

![VMware custom network config](<screenshots/2026-08-03/Vmwareconfig.png>)

### 2. Client to DC session check

Checked the client login/session from command line with `whoami` and `whoami /user`. This was to confirm the domain user context.

![Client DC setup with session through powershell](<screenshots/2026-08-03/Client-DC SetupwithSessiontroughpoweshell.png>)

### 3. Adding user to OU

In Active Directory Users and Computers I made basic OUs like HR, IT, Sales and Accounting, then placed a user under IT.

![Adding user to OU](<screenshots/2026-08-03/AddingusertoOU.png>)

### 4. Bulk user insert script plan

Prepared an Excel style sheet to build `dsadd user` commands for multiple users with UPN, display name and must-change-password settings.

![Bulk insert AD users using script](<screenshots/2026-08-03/BulkinsertADUsersusingScript.png>)

### 5. Bulk user profile path

Set roaming profile path for a test user to a server share like `\\s1\IT-Profiles-Data\IT03`.

![Bulk user profile creation](<screenshots/2026-08-03/BulkUserprofileCreation.png>)

### 6. Redirected Documents / profile data

Moved or checked the Documents location for a user so files go back to a server path. This is for tracking user docs from the DC/server side.

![Changing folder location to DC server location](<screenshots/2026-08-03/ChangingFolderlocationtoDCServer'slocationtotrackanydocs.png>)

### 7. Checking document changes on DC

Verified that folders or files created from the IT client side appear on the server path.

![Client document changes seen on DC server](<screenshots/2026-08-03/AnychangeordocinclientofITsectionisseeninDCServer.png>)

### 8. Mapped drive

Mapped HR data as drive `H:` from `\\S1`. This is the kind of thing I want to push later by GPO.

![Mapped drive in client](<screenshots/2026-08-03/mapdriveinclient.png>)

### 9. Share vs security permissions

Checked sharing permissions and NTFS/security settings for `HR-DATA`. Goal was to understand the difference between share permissions and file permissions.

![Share permissions and security permissions](<screenshots/2026-08-03/SharingvsSecuirtyandpermissionconfiguration.png>)

### 10. Roaming user access in GPO

Opened GPO user profile settings and checked options related to roaming profiles and administrator access.

![Roaming user administrator access in GPO](<screenshots/2026-08-03/RoamingUserAdministratorAccessinGPO.png>)

## Next things to add

- DNS and DHCP notes
- WSUS install/config screenshots
- GPO mapped drive test
- More PowerShell scripts for AD users/groups
- Storage/share permission cases
- Things that failed and how I fixed them
