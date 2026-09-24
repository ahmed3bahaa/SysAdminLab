# SysAdminLab

Windows Server and system administration lab portfolio.

## About This Repository

This repository documents my self-directed Windows Server and system administration training. I am a Computer Engineering graduate building practical experience for entry-level IT infrastructure, Windows System Administrator, and cybersecurity roles.

The work here was completed in a VMware-based lab environment. It is not a production environment. The goal is to show hands-on practice with Active Directory, Windows Server storage, file services, basic automation ideas, troubleshooting, and related administration tasks.

## Lab Environment

The current evidence in this repository confirms a VMware lab with Windows Server virtual machines, a Windows client/domain test setup, and Active Directory work around `Ahmed.Edu`. One older bulk-user command sheet still shows `islam.net`, so I kept that noted in the AD lab instead of hiding the mismatch.

Confirmed from screenshots:

- VMware Workstation virtual machines.
- Windows Server 2022 Standard Evaluation appears in the lab screenshots.
- Domain-related work in `Ahmed.Edu`.
- Server names including `S1` and `S1.Ahmed.Edu`.
- File and storage services, iSCSI, FSRM, Storage Spaces, and Event Viewer testing.

Detailed environment notes are in [docs/lab-environment.md](docs/lab-environment.md).

## Technical Skills Demonstrated

| Technology / Skill | Practical work completed | Documentation |
| --- | --- | --- |
| Active Directory | Created OUs, checked domain user session, prepared bulk `dsadd user` commands | [Active Directory](01-Active-Directory/README.md) |
| Group Policy / Profiles | Practiced roaming profile paths and reviewed GPO user profile settings | [Group Policy](02-Group-Policy/README.md) |
| File Services | Tested SMB/NTFS permissions, mapped drives, redirected user documents, and server-side document visibility | [Storage and File Services](04-Storage-File-Services/README.md) |
| Disk Management | Created and compared simple, spanned, striped, mirrored, RAID-5, and logical volumes | [Storage and File Services](04-Storage-File-Services/README.md) |
| Storage Spaces | Created a storage pool, virtual disks, and extended storage with added disks | [Storage and File Services](04-Storage-File-Services/README.md) |
| VHD / VHDX | Created, attached, mounted, and tested dynamic VHDX behavior | [Storage and File Services](04-Storage-File-Services/README.md) |
| Data Deduplication | Evaluated dedup savings, scheduled deduplication, and ran a dedup job from PowerShell | [Storage and File Services](04-Storage-File-Services/README.md) |
| FSRM | Installed/used FSRM features, tested quotas, event logging, and reports | [Storage and File Services](04-Storage-File-Services/README.md) |
| iSCSI | Installed iSCSI Target Server, created a virtual disk target, and connected from an initiator | [Storage and File Services](04-Storage-File-Services/README.md) |
| Command-line administration | Used tools such as `whoami`, `dsadd`, `chkdsk`, `diskpart`, `mklink`, and deduplication PowerShell cmdlets | [PowerShell and Automation](05-PowerShell-Automation/README.md) |
| Troubleshooting / Verification | Used Event Viewer, command output, Disk Management, and Server Manager to verify results | [Monitoring and Troubleshooting](07-Monitoring-Troubleshooting/README.md) |
| Virtualization | Configured VMware virtual networking for the lab | [Virtualization](06-Virtualization/README.md) |

## Projects and Labs

| Project | Description |
| --- | --- |
| [01-Active-Directory](01-Active-Directory/README.md) | OU/user work, domain session checks, and bulk user command preparation. |
| [02-Group-Policy](02-Group-Policy/README.md) | Roaming profile and User Profiles policy practice. |
| [04-Storage-File-Services](04-Storage-File-Services/README.md) | Shares, NTFS permissions, dynamic disks, Storage Spaces, VHDs, deduplication, FSRM, and iSCSI. |
| [05-PowerShell-Automation](05-PowerShell-Automation/README.md) | Command-line and automation evidence currently captured in screenshots. |
| [06-Virtualization](06-Virtualization/README.md) | VMware lab networking setup. |
| [07-Monitoring-Troubleshooting](07-Monitoring-Troubleshooting/README.md) | Verification and troubleshooting notes from the labs. |

## PowerShell Scripts

There are no standalone `.ps1`, `.bat`, `.cmd`, `.csv`, or spreadsheet files currently committed in this repository.

The repository does include screenshots showing command-based work and script concepts:

- `dsadd user` command generation for bulk Active Directory users.
- `chkdsk` volume checking.
- DiskPart partitioning and formatting.
- `mklink /H` hard link creation.
- `mklink /J` junction creation.
- `ddpeval`, `Start-DedupJob`, and `Get-DedupJob` for deduplication testing.

Details are tracked in [05-PowerShell-Automation](05-PowerShell-Automation/README.md). A current documentation TODO is to add the actual reusable scripts used during the lab.

## Troubleshooting and Lessons Learned

Documented troubleshooting examples include:

- `chkdsk` could not run on a busy volume, so the check was run against another target volume.
- A hard link command failed because the source file did not exist; the file was created and the command was rerun successfully.
- A deduplication PowerShell command failed due to an invalid job type typo, then worked after using `Optimization`.
- FSRM quota threshold events were verified in Event Viewer.
- iSCSI target connection was verified by checking that the remote disk appeared in Disk Management.

More detail is in [07-Monitoring-Troubleshooting](07-Monitoring-Troubleshooting/README.md).

## Current Learning Objectives

Planned or not-yet-documented topics:

- DNS and DHCP configuration.
- WSUS installation and update management.
- GPO-based mapped drives.
- More complete PowerShell scripts for AD users, groups, storage, and reporting.
- Cleaner script/source files instead of screenshots only.
- More Windows Server security-focused labs.
- Hybrid/Azure administration practice.

These are learning objectives, not completed projects in this repository yet.

## Contact and Professional Profile

GitHub: [ahmed3bahaa](https://github.com/ahmed3bahaa)
