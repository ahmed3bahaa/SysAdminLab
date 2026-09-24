# Monitoring and Troubleshooting

## Objective

Collect troubleshooting and verification notes from the Windows Server labs.

## Documented Troubleshooting

### Busy volume during `chkdsk`

The storage lab shows `chkdsk` reporting that a volume was in use. I then ran the check against another target volume.

Evidence: [Disk check](../04-Storage-File-Services/README.md#2-disk-check)

### Hard link source file missing

The first hard link attempt failed because the source file did not exist. I created the file and reran `mklink /H` successfully.

Evidence: [Hard link creation](../04-Storage-File-Services/README.md#14-hard-link)

### Dedup job typo

The first dedup job attempt used an invalid job type. The corrected command used `Optimization`.

Evidence: [Dedup job from PowerShell](../04-Storage-File-Services/README.md#21-deduplication-job-from-powershell)

### Quota event verification

FSRM quota threshold behavior was verified in Event Viewer with an SRMSVC event.

Evidence: [Quota event in Event Viewer](../04-Storage-File-Services/README.md#26-quota-alert-in-event-viewer)

### iSCSI verification

The iSCSI target was checked from the initiator side and the disk appeared in Disk Management.

Evidence: [iSCSI disk appears in Disk Management](../04-Storage-File-Services/README.md#31-iscsi-disk-in-disk-management)

## Lessons Learned

- GUI configuration should be verified with a second tool when possible.
- Event Viewer is useful for confirming FSRM and auditing behavior.
- Command errors are worth documenting because they show how the fix was found.
- Storage configuration is easier to trust after checking Disk Management, Server Manager, and File Explorer.

## Documentation TODO

- Add exported event logs for important tests.
- Add exact Event IDs and messages where possible.
- Add a repeatable troubleshooting checklist for common Windows Server storage problems.
