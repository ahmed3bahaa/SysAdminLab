# Storage and File Services

## Objective

Practice Windows Server storage and file services tasks, including SMB/NTFS permissions, mapped folders, dynamic disks, Storage Spaces, VHD/VHDX, Data Deduplication, FSRM quotas/reports, and iSCSI.

## Lab Environment

- VMware-based Windows Server lab.
- Domain evidence from related labs: `Ahmed.Edu`.
- Tools used: Disk Management, Server Manager, File Explorer, PowerShell, Command Prompt, File Server Resource Manager, Event Viewer, iSCSI Target Server, and iSCSI Initiator.

## Lab 1: File Shares, Permissions, and User Data - 2026-08-03

### Objective

Practice shared folders, NTFS/security permissions, mapped drives, and redirecting or storing user document data on the server.

### Implementation and Evidence

#### Redirected Documents / profile data

I checked a user's Documents location and pointed it to a server path. This was part of practicing how user data can be kept on the server instead of only on the client.

![Documents path moved to server location](<screenshots/2026-08-03/ChangingFolderlocationtoDCServer'slocationtotrackanydocs.png>)

#### Server-side document visibility

I verified that folders created from the client side appeared on the server path.

![Client document changes visible on server](<screenshots/2026-08-03/AnychangeordocinclientofITsectionisseeninDCServer.png>)

#### Mapped drive

I mapped an HR data share as drive `H:` from `\\S1`.

![Mapped client drive](<screenshots/2026-08-03/mapdriveinclient.png>)

#### Share and NTFS permissions

I reviewed both share permissions and NTFS security permissions for `HR-DATA`. This is important because effective access depends on both layers.

![Share and NTFS permissions](<screenshots/2026-08-03/SharingvsSecuirtyandpermissionconfiguration.png>)

### Results

The lab shows basic file server access, mapped drives, and the difference between share-level permissions and NTFS permissions.

### Lessons Learned

- Share permissions and NTFS permissions both affect final access.
- Server-side user data needs proper folder paths and permissions.
- Mapped drives make network shares easier for users to access.

### Documentation TODO

- Add exact share names and UNC paths.
- Add before/after permission tables.
- Add client-side access-denied/success tests for different users.

## Lab 2: Storage, FSRM, Deduplication, and iSCSI - 2026-09-23

### Objective

Practice Windows Server storage administration from basic disks through more advanced file and storage services.

### Implementation

#### 1. Basic disk allocation

I created a simple volume and assigned a drive letter in Disk Management.

![Basic disk allocation](<screenshots/2026-09-23/BasicDiskAllocation.png>)

#### 2. Disk check

I ran `chkdsk e: /F`. The screenshots also show the normal problem where a volume cannot be checked if it is in use.

![Disk check](<screenshots/2026-09-23/DiskCheckandconfigureingand extending.png>)

#### 3. DiskPart format and logical partition

I practiced DiskPart commands for formatting, marking a partition active, creating an extended partition, creating a logical partition, assigning a drive letter, and cleaning the disk.

![DiskPart formatting and logical partition](<screenshots/2026-09-23/CreatingFomrattingAssigningtothebasicdisckLogicalandprimarystorage.png>)

#### 4. Striped volume

I created a striped dynamic volume. A striped volume spreads data across disks, but it does not provide redundancy and is not the same as a mirrored or parity-based volume.

![Striped volume](<screenshots/2026-09-23/StrippedDiskThatCannotbeExtended.png>)

#### 5. Spanned volume

I created a spanned dynamic volume to combine space from multiple disks. This is flexible for adding capacity, but it does not protect data if one disk fails.

![Spanned volume](<screenshots/2026-09-23/SpannedDiskthatcanbeadjustedeasily.png>)

#### 6. Mirrored volume

I created a mirrored volume across two disks. The goal was to understand basic redundancy through duplicate data.

![Mirrored volume](<screenshots/2026-09-23/MirrorDiskThatDuplicateforPersistance.png>)

#### 7. Breaking a mirror

I broke the mirror and observed that Windows assigned separate drive letters to each side.

![Breaking mirrored volume](<screenshots/2026-09-23/BreakingthemirriorDiskAssiginsdifferentLetters.png>)

#### 8. RAID-5 volume

I created a Windows dynamic disk RAID-5 volume using three disks. This was to compare parity-based storage with mirror, spanned, and striped layouts.

![RAID-5 volume](<screenshots/2026-09-23/RAID5Thatmustuses3diskoneforparity.png>)

#### 9. Storage pool

I created a Storage Spaces pool from physical disks in Server Manager.

![Storage pool creation](<screenshots/2026-09-23/Storagepoollinkscreation.png>)

#### 10. Virtual disk from the pool

I created a virtual disk from the storage pool and formatted it with NTFS.

![Virtual disk creation](<screenshots/2026-09-23/ThenAfteritVirtualDiskCreation.png>)

#### 11. Adding physical disks

I added two new physical disks and rescanned the server so the storage pool could see the extra disks.

![Adding physical disks](<screenshots/2026-09-23/Addiningtwonewphysicaldiskandrescaningtheserver.png>)

#### 12. Disk visible in File Explorer

I checked File Explorer to confirm that the storage pool volume was usable as a normal drive.

![Disk visible in This PC](<screenshots/2026-09-23/HerewecanseethediskDrive.png>)

#### 13. Extending storage and adding volumes

I extended the storage setup and created additional virtual disks, including a simple and mirrored virtual disk.

![Extending storage and adding volumes](<screenshots/2026-09-23/ExtendingtheDiskandaddingnewvolumes.png>)

#### 14. Hard link

I practiced creating a hard link with `mklink /H`. The first attempt failed because the source file did not exist. After creating the file, the hard link succeeded.

![Hard link creation](<screenshots/2026-09-23/HardLinkCreation.png>)

#### 15. Junction link

I created a junction using `mklink /J`, linking a Pictures path to a folder on another volume.

![Junction link creation](<screenshots/2026-09-23/JunctionLinkOrSoftlinkCreation.png>)

#### 16. VHDX creation

I created and attached a dynamically expanding VHDX file.

![VHD creation and attach](<screenshots/2026-09-23/VHDCreation&Attaching.png>)

#### 17. Mounted VHD as isolated disk

After attaching the VHD, it appeared as its own mounted volume.

![VHD isolation](<screenshots/2026-09-23/VHDIsolation.png>)

#### 18. Growing a dynamic VHD

I tested dynamic VHD growth by mounting the VHD and adding data until the file grew.

![Dynamic VHD growth test](<screenshots/2026-09-23/AtricktoincreaseVHDistomountthenfillitdatauntilitisforcedtogetbiggertheneject.png>)

#### 19. Deduplication evaluation

I ran `ddpeval G:` to estimate possible deduplication savings.

![Deduplication evaluation](<screenshots/2026-09-23/dedupevalutioninstall.png>)

#### 20. Deduplication schedule

I configured a deduplication schedule through Server Manager.

![Deduplication scheduling](<screenshots/2026-09-23/dedupscheduling.png>)

#### 21. Deduplication job from PowerShell

I started a deduplication optimization job from PowerShell and checked job status with `Get-DedupJob`.

The screenshot also shows a typo in the first attempt. The correct job type was `Optimization`.

![Deduplication job from PowerShell](<screenshots/2026-09-23/DedupjobusingCMD.png>)

#### 22. Deduplication result

Server Manager showed a deduplication rate of `84%` and approximately `2.79 GB` saved.

![Deduplication result](<screenshots/2026-09-23/DeduplicationRateis84%.png>)

#### 23. FSRM task list

I kept notes on the FSRM areas I wanted to practice: installing FSRM, creating quotas, file screening, and reports.

![FSRM task list](<screenshots/2026-09-23/FSRMTasks.png>)

#### 24. Quota and classification properties

I checked quota-related disk settings and the Classification tab while reviewing file server management options. The actual FSRM quota work is shown in the next screenshots.

![FSRM feature visibility](<screenshots/2026-09-23/FSRMinstallationseencauseoftheclassificationtabplusquotamanagmentonspecificfile.png>)

#### 25. Quota configuration

I created or edited a quota with a hard limit so users cannot exceed the configured storage limit.

![Quota configuration](<screenshots/2026-09-23/Quotapropslimiteventviewerandhardquotainsteadofmontiorthroughsoft.png>)

#### 26. Quota alert in Event Viewer

I generated a quota threshold warning and confirmed it in Event Viewer.

![Quota warning in Event Viewer](<screenshots/2026-09-23/herewecanseethetestiranwithquotaexceeding85%seenineventviewr.png>)

#### 27. FSRM report

I opened a File Screening Audit Report generated by FSRM.

![FSRM audit report](<screenshots/2026-09-23/FSRMAuditingreport.png>)

#### 28. iSCSI Target Server install

I installed the iSCSI Target Server role.

![iSCSI role installation](<screenshots/2026-09-23/IscsiInstallation.png>)

#### 29. Assign iSCSI target

I created an iSCSI virtual disk and assigned it to a new target.

![Assign iSCSI target](<screenshots/2026-09-23/ISCSITargetAssiging.png>)

#### 30. Connect target through IQN

I connected the iSCSI target using the initiator/IQN relationship.

![Connect iSCSI target](<screenshots/2026-09-23/ConnectingtheTargetIscsitotheServerthroughIQN.png>)

#### 31. iSCSI disk in Disk Management

After connecting the target, the disk appeared in Disk Management on the other server.

![iSCSI disk in Disk Management](<screenshots/2026-09-23/TheISCSICREATEDontheTargetisinstantlyreflectedwhenrunningdiskmgmt.msc.png>)

#### 32. iSCSI Initiator connection state

The iSCSI Initiator properties showed the target connected.

![iSCSI connection state](<screenshots/2026-09-23/Stepsofconnecting-disconnectingtheISCITarget.png>)

### Commands and Scripts

Commands visible in the screenshots include:

```cmd
chkdsk e: /F
mklink /H E:\new\cat.txt E:\old\test.txt
mklink /J C:\Users\Administrator\Pictures\Images E:\Images
```

```powershell
ddpeval G:
Start-DedupJob -Volume G: -Type Optimization
Get-DedupJob
```

DiskPart commands are visible in the screenshots, including `format quick`, `active`, `create partition extended`, `create partition logical`, `assign letter`, and `clean`.

No reusable script files are currently committed for this lab.

### Verification and Testing

- Disk Management was used to confirm created volumes and layouts.
- File Explorer was used to confirm mounted volumes and Storage Spaces access.
- PowerShell was used to check deduplication job status.
- Server Manager showed deduplication savings.
- Event Viewer showed an FSRM quota threshold warning.
- iSCSI Initiator and Disk Management were used to verify iSCSI connectivity.

### Troubleshooting

- `chkdsk` could not run on a busy volume, so another volume was checked.
- The first hard link command failed because the source file did not exist.
- A deduplication job command failed due to an invalid type name, then worked after using `Optimization`.

### Results

This lab demonstrates practical storage administration across Windows Disk Management, Server Manager, PowerShell, FSRM, and iSCSI.

### Lessons Learned

- Dynamic disk volume types behave differently: spanned and striped are not redundancy features, while mirrored and RAID-5 provide fault-tolerance patterns.
- Storage Spaces is a different management model from classic dynamic disks.
- VHDX files can be mounted and treated like disks, and dynamically expanding disks grow as data is written.
- Deduplication should be evaluated and verified instead of assumed.
- FSRM quotas are useful only when paired with clear testing and event/report checks.
- iSCSI needs both target-side configuration and initiator-side verification.

### Documentation TODO

- Add exact disk numbers and sizes used in each volume test.
- Add a clean table comparing simple, spanned, striped, mirrored, RAID-5, and Storage Spaces virtual disks.
- Add iSCSI target IP/DNS details if they are intended to be public lab documentation.
- Add exported FSRM quota/report configuration if available.
