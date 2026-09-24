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

## Results

The current repository shows real command-line practice, but not reusable automation yet.

## Documentation TODO

- Add actual `.ps1` scripts for user creation, storage checks, and reporting.
- Add comments and usage examples for each script.
- Add sample input files such as CSV templates when available.
- Keep screenshots as proof, but commit the scripts themselves so the work is reusable.
