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

## 2026-09-23 - storage, FSRM and iSCSI lab

This day was mostly storage work. I went through basic disks, dynamic disks, Storage Spaces, VHDs, deduplication, FSRM quotas/reports and a small iSCSI target test between the lab servers.

Some names are rough because I saved the screenshots while doing the lab.

### 1. Basic disk allocation

Started with a normal simple volume and assigned a drive letter from Disk Management. This is the basic path before getting into dynamic disk types.

![Basic disk allocation](<screenshots/2026-09-23/BasicDiskAllocation.png>)

### 2. Disk check

Ran `chkdsk e: /F` and watched the stages. I also saw the usual message when a volume is busy, so I tried it against a volume that could be checked.

![Disk check and configuring](<screenshots/2026-09-23/DiskCheckandconfigureingand extending.png>)

### 3. DiskPart format and logical partition

Used DiskPart to format, mark active, create an extended partition, create a logical partition, assign a letter, and then clean the disk after testing.

![DiskPart formatting and logical partition](<screenshots/2026-09-23/CreatingFomrattingAssigningtothebasicdisckLogicalandprimarystorage.png>)

### 4. Striped volume

Created a striped volume across dynamic disks. Main note here is that it can improve spread/performance, but it is not the one to pick if I need easy extending or redundancy.

![Striped disk](<screenshots/2026-09-23/StrippedDiskThatCannotbeExtended.png>)

### 5. Spanned volume

Created a spanned volume and saw how it can use space from more than one disk. This is more flexible than striped for adding space, but still not redundancy.

![Spanned disk](<screenshots/2026-09-23/SpannedDiskthatcanbeadjustedeasily.png>)

### 6. Mirrored volume

Created a mirrored volume using two disks. This one is for keeping a duplicate copy, so if one side fails there is still another copy.

![Mirrored disk](<screenshots/2026-09-23/MirrorDiskThatDuplicateforPersistance.png>)

### 7. Breaking the mirror

Broke the mirror and Windows assigned different letters to the two sides. Good to see what happens after separating a mirrored volume.

![Breaking mirror disk](<screenshots/2026-09-23/BreakingthemirriorDiskAssiginsdifferentLetters.png>)

### 8. RAID-5 volume

Created a RAID-5 volume using three disks, with one part acting for parity. I wanted to see how it looks in Disk Management compared with mirror and spanned.

![RAID 5 volume](<screenshots/2026-09-23/RAID5Thatmustuses3diskoneforparity.png>)

### 9. Storage pool creation

Moved to Server Manager and selected physical disks for a new storage pool. This is the Storage Spaces way instead of just using Disk Management.

![Storage pool creation](<screenshots/2026-09-23/Storagepoollinkscreation.png>)

### 10. Virtual disk from the pool

Created a virtual disk from the pool and picked NTFS during the new volume wizard.

![Virtual disk creation](<screenshots/2026-09-23/ThenAfteritVirtualDiskCreation.png>)

### 11. New physical disks added

Added two new physical disks and rescanned the server. The storage pool view showed the new disks and the existing virtual disks.

![Adding physical disks and rescanning](<screenshots/2026-09-23/Addiningtwonewphysicaldiskandrescaningtheserver.png>)

### 12. Disk visible in This PC

Checked from File Explorer and the disk pool showed as a normal drive. I like checking both Server Manager and Explorer so I know it is actually usable.

![Disk drive visible](<screenshots/2026-09-23/HerewecanseethediskDrive.png>)

### 13. Extending storage

Extended the storage setup and added another virtual disk. The pool had one simple virtual disk and another mirrored virtual disk.

![Extending disk and adding volumes](<screenshots/2026-09-23/ExtendingtheDiskandaddingnewvolumes.png>)

### 14. Hard link

Tested hard links with `mklink /H`. First attempt failed because the source file did not exist, then I created the file and the hard link worked.

![Hard link creation](<screenshots/2026-09-23/HardLinkCreation.png>)

### 15. Junction link

Created a junction with `mklink /J` from the Pictures path to another folder on `E:`. This is useful to understand how folder redirection-style links behave.

![Junction link creation](<screenshots/2026-09-23/JunctionLinkOrSoftlinkCreation.png>)

### 16. VHD creation and attach

Created a dynamic VHDX from Disk Management. I used `G:\vdsk1.vhdx` and picked dynamically expanding.

![VHD creation and attaching](<screenshots/2026-09-23/VHDCreation&Attaching.png>)

### 17. VHD isolation

After attaching the VHD, it showed as its own disk/volume. This helped show how a VHD can be mounted and treated like a separate disk.

![VHD isolation](<screenshots/2026-09-23/VHDIsolation.png>)

### 18. Growing a dynamic VHD

The trick was to mount the VHD, fill it with data until it is forced to grow, then eject it. Good practical way to see dynamic growth instead of only reading about it.

![VHD growth trick](<screenshots/2026-09-23/AtricktoincreaseVHDistomountthenfillitdatauntilitisforcedtogetbiggertheneject.png>)

### 19. Dedup evaluation

Ran `ddpeval G:` to estimate dedup savings. It showed high savings, so this volume was a good example for dedup testing.

![Dedup evaluation install](<screenshots/2026-09-23/dedupevalutioninstall.png>)

### 20. Dedup schedule

Configured dedup scheduling in Server Manager. I used background optimization and played with the schedule times.

![Dedup scheduling](<screenshots/2026-09-23/dedupscheduling.png>)

### 21. Dedup job from PowerShell

Started a dedup job from PowerShell. I made a typo first with the type name, then corrected it to `Optimization`.

![Dedup job using CMD](<screenshots/2026-09-23/DedupjobusingCMD.png>)

### 22. Dedup result

Checked Server Manager and saw a deduplication rate of `84%` with about `2.79 GB` saved. This was the proof that dedup actually did something.

![Deduplication rate](<screenshots/2026-09-23/DeduplicationRateis84%.png>)

### 23. FSRM task list

Reviewed the FSRM tasks: install FSRM, create quotas, file screening and reports. This was the checklist before doing the actual FSRM work.

![FSRM tasks](<screenshots/2026-09-23/FSRMTasks.png>)

### 24. FSRM installed

Confirmed FSRM was installed because the classification tab and quota management options were visible.

![FSRM installation seen](<screenshots/2026-09-23/FSRMinstallationseencauseoftheclassificationtabplusquotamanagmentonspecificfile.png>)

### 25. Quota settings

Tested quota settings. I used a hard quota instead of only monitoring, so users cannot exceed the configured limit.

![Quota properties](<screenshots/2026-09-23/Quotapropslimiteventviewerandhardquotainsteadofmontiorthroughsoft.png>)

### 26. Quota event in Event Viewer

Generated a quota warning and saw Event Viewer report that the user passed the `85%` threshold. This confirms the alerting side works.

![Quota event viewer test](<screenshots/2026-09-23/herewecanseethetestiranwithquotaexceeding85%seenineventviewr.png>)

### 27. FSRM report

Opened the file screening audit report in the browser. This report path is useful when checking file screening or storage policy violations.

![FSRM auditing report](<screenshots/2026-09-23/FSRMAuditingreport.png>)

### 28. iSCSI Target Server install

Installed the iSCSI Target Server role from Add Roles and Features.

![iSCSI installation](<screenshots/2026-09-23/IscsiInstallation.png>)

### 29. Assign iSCSI target

Created a new iSCSI virtual disk and assigned it to a new target.

![iSCSI target assigning](<screenshots/2026-09-23/ISCSITargetAssiging.png>)

### 30. Connect target with IQN

Connected the target from the other server using the initiator/IQN path. This is where the target side and initiator side start matching.

![Connecting iSCSI target](<screenshots/2026-09-23/ConnectingtheTargetIscsitotheServerthroughIQN.png>)

### 31. iSCSI disk appears in Disk Management

On the client/server side, the iSCSI disk appeared in Disk Management as a normal disk that could be initialized and formatted.

![iSCSI reflected in disk management](<screenshots/2026-09-23/TheISCSICREATEDontheTargetisinstantlyreflectedwhenrunningdiskmgmt.msc.png>)

### 32. Connect and disconnect steps

Checked the iSCSI Initiator window and saw the discovered target connected. This is also where I can disconnect it again when testing.

![iSCSI connect disconnect steps](<screenshots/2026-09-23/Stepsofconnecting-disconnectingtheISCITarget.png>)

## Next things to add

- DNS and DHCP notes
- WSUS install/config screenshots
- GPO mapped drive test
- More PowerShell scripts for AD users/groups
- Storage/share permission cases
- Things that failed and how I fixed them
