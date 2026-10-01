# Lab Environment

This file documents only details that are visible in the current repository evidence.

## Platform

- VMware Workstation is used to run the lab virtual machines.
- Screenshots show Windows Server 2022 Standard Evaluation on server VMs.
- The lab uses a custom VMware virtual network for isolated practice.

## Domain and Hosts

- Active Directory domain visible in screenshots: `Ahmed.Edu`.
- Server names visible in screenshots include `S1`, `S1.Ahmed.Edu`, `S2`, and `S3`.
- Multiple VMs are visible in VMware tabs, including `S1`, `S2`, and `S3`.
- A Windows client/domain login test is shown with `whoami` and `whoami /user`.
- NLB evidence shows cluster name `NLB.Ahmed.edu` with cluster IP `10.0.0.100`.
- NLB node IPs visible in the script/screenshots are `10.0.0.2` for `S2` and `10.0.0.3` for `S3`.

## Technologies Practiced

- Active Directory Users and Computers.
- Group Policy Management Editor.
- Windows Server Manager.
- Disk Management.
- Storage Spaces.
- SMB shares and NTFS permissions.
- Data Deduplication.
- File Server Resource Manager.
- iSCSI Target Server and iSCSI Initiator.
- Storage Replica.
- IIS / Web Server role.
- Network Load Balancing.
- PowerShell Desired State Configuration.
- Event Viewer.
- Command-line tools including `whoami`, `dsadd`, `chkdsk`, `diskpart`, `mklink`, `Install-WindowsFeature`, and Windows Server PowerShell cmdlets.

## Documentation TODO

- Add a clean diagram of the VM layout.
- Add CPU/RAM/disk allocations for each VM.
- Add IP addressing, DNS, and DHCP details after those labs are documented.
- Add a list of server roles installed on each VM.
