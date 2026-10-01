# PowerShell and Command-Line Automation

## Objective

Track command-line administration and automation practice from the lab.

At the moment, this repository does not contain standalone script files. The evidence is screenshot-based, so this section documents commands and script ideas that are visible in the labs.

## Existing Evidence

| Command / Tool | Purpose | Evidence |
| --- | --- | --- |
| `whoami`, `whoami /user` | Verify current domain user context | [Active Directory session check](../01-Active-Directory/README.md#domain-session-check) |
| `dsadd user` | Prepare bulk AD user creation commands | [Bulk user command preparation](../01-Active-Directory/README.md#bulk-user-command-preparation) |
| `chkdsk e: /F` | Check file system consistency | [Storage troubleshooting](../04-Storage-File-Services/README.md#2-disk-check) |
| DiskPart commands | Format, partition, assign letters, and clean disks | [DiskPart storage work](../04-Storage-File-Services/README.md#3-diskpart-format-and-logical-partition) |
| `mklink /H` | Create a hard link | [Hard link practice](../04-Storage-File-Services/README.md#14-hard-link) |
| `mklink /J` | Create a junction | [Junction practice](../04-Storage-File-Services/README.md#15-junction-link) |
| `ddpeval G:` | Estimate deduplication savings | [Dedup evaluation](../04-Storage-File-Services/README.md#19-deduplication-evaluation) |
| `Start-DedupJob`, `Get-DedupJob` | Run and inspect deduplication jobs | [Dedup PowerShell job](../04-Storage-File-Services/README.md#21-deduplication-job-from-powershell) |
| `Install-WindowsFeature` | Install IIS, NLB, Storage Replica, and File Server features | [NLB/IIS](../08-Network-Load-Balancing-IIS/README.md#install-iis-and-nlb-features), [Storage Replica](../04-Storage-File-Services/README.md#lab-3-storage-replica-prep-and-script) |
| `Set-DscLocalConfigurationManager` | Push DSC Local Configuration Manager settings to a remote server | [DSC IIS lab](#lab-desired-state-configuration-for-iis) |
| `Test-SRTopology`, `New-SRPartnership` | Test and create Storage Replica pairing | [Storage Replica script](../04-Storage-File-Services/README.md#storage-replica-script-draft) |

## Lab: Desired State Configuration for IIS

Date: 2026-10-01.

### Objective

Practice writing a DSC configuration that prepares IIS and remote IIS management settings on another server.

### Implementation

#### LCM push mode

I started with the Local Configuration Manager settings. The script sets the target server to `S3`, uses push mode, and sets `ApplyAndAutoCorrect`.

![DSC LCM push mode](<screenshots/2026-10-01/DSCRemotelyonS2andS3withpushmode.png>)

#### IIS feature configuration

The DSC configuration imports `PSDesiredStateConfiguration` and defines IIS features such as `Web-Server`, `Web-Static-Content`, `Web-Default-Doc`, and the IIS management console.

![IIS features in DSC](<screenshots/2026-10-01/IIStoolsandfeaturesinstalledonthetwoserversremotly.png>)

#### Remote IIS management and website files

The rest of the script enables remote IIS management, sets the `WMSVC` service to automatic/running, creates the website root, and prepares an `index.html` file.

![IIS management and web root settings](<screenshots/2026-10-01/RestoftheIISManagmenttfeaturesforDSC.png>)

### Commands and Scripts

Commands visible in the screenshots include:

```powershell
[DSCLocalConfigurationManager()]
Set-DscLocalConfigurationManager -ComputerName $TargetServer -Path $LcmOutputPath -Verbose
Get-DscLocalConfigurationManager
Import-DscResource -ModuleName PSDesiredStateConfiguration
```

The actual `DSCScript.ps1` file is not committed yet. Right now this is proof of the script work, not reusable automation stored in the repo.

### Verification and Testing

- The screenshots show the script structure and target server value.
- The LCM block uses push mode.
- The IIS DSC resources are written in PowerShell ISE.

I do not have the execution output committed yet, so I am not claiming the DSC configuration fully applied from these screenshots alone.

## Results

The current repository shows real command-line and PowerShell automation practice, but the reusable `.ps1` files still need to be committed.

## Documentation TODO

- Add actual `.ps1` scripts for user creation, storage checks, and reporting.
- Add `DSCScript.ps1`, `NLBScripts.ps1`, and `StorageReplica.ps1` when ready.
- Add comments and usage examples for each script.
- Add sample input files such as CSV templates when available.
- Keep screenshots as proof, but commit the scripts themselves so the work is reusable.
