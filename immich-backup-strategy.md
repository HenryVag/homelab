# Immich Backup Documentation

This file contains the backup strategy for Immich, which includes an overview and description of the architecture and instructions to perform the manual off-site HDD backups.
The basis of this strategy has been the 3-2-1 backup rule.

# Overview

Immich stores its data in the **UPLOADS LOCATION**, which contains both uploaded data (images, videos) and automatic back ups of the database information in UPLOADS LOCATION/backups. 
TODO: What data is stored locally on the phone, have the phone store and backup images from its memory card.

## Risks & Mitigations
|Risk|Mitigation|
|---------|---------|
|Single SSD failure on the Debian server/Proxmox|2x512GB NVME SSDs are in ZFS Mirror|
|Single disk failure in the TrueNAS storage pool|2x2TB HDDs are in ZFS Mirror.|
|Accidental deletion or modification of files|Automated ZFS snapshots provide historical recovery points.|
|Accidental deletion or corruption of the Immich database|Immich automatically creates daily database backups with a 14-day retention period.|
|Need to recover data from an earlier point in time|Daily, weekly, monthly and yearly ZFS snapshots are automatically taken.|
|Complete loss of the TrueNAS system or storage pool|Manual periodic rsync backups to an external off-site HDD.|
|Physical loss or damage to the server|External HDD is stored off-site|
|Ransomware or other malicious modification of data|The external HDD is normally disconnected, preventing it from being modified through the main system.|
|Loss of the Immich database|The database can be restored from an Immich database backup. They are stored both on the ZFS snapshots and external off-site HDD|
|Complete loss of the Debian VM|Immich can be reinstalled and the media and database restored from the available backups.|
|Sudden power loss| Off-site backup + hopes and dreams.|

## Architecture Diagram

images/immich-backup-diagram.png
https://github.com/HenryVag/homelab/blob/d2af8070d4dcfd85b5c01a168fbcde367035e64f/images/immich-backup-diagram.png

## Automated Schedules & Retention Times

### TrueNAS Snapshots
|Type|Time|Day|Retention|
|---------|-----|-----------------|-----------------|
| Daily    | 00:00  | Daily    | 14 days |
| Weekly     | 00:00  | Sunday    | 2 months |
| Monthly      | 00:00  | 1st of the month| 12 months |
| Yearly | 00:00  | January 1st| 4 years |


### Immich DB Dumps
|Type|Time|Day| Retention|
|---------|-----|-----------------|-----------------|-------------------------------------|
| Daily    | 00:00  | Daily    | 14 days|



## How To Manually Backup Off-Site HDD 

1. Plug the HDD into the SATA-USB adapter and connect it with the rear USB 3.0 ports of the Proxmox machine.
2. Add the SATA-USB adapter as a USB device to TrueNAS in Proxmox.
3. Identify the HDD by running ``lsblk`` in the TrueNAS shell.
4. Verify the filesystem with ``lsblk -f /dev/sdd``. exFat is recommended because it is compatible with both Linux and Windows.
5. Create a mount point with ``sudo  mkdir  -p /mnt/usb-backup`` if it does not already exist (it should).
6. Mount the partition ``sudo mount -t exfat /dev/sdd1 /mnt/usb-backup`` .
7. Verify that it is mounted with ``mount | grep usb-backup``.
8. Create the Immich backup directory ``sudo mkdir -p /mnt/usb-backup/immich``.
9. Copy the Immich dataset ```sudo rsync -avh  --progress /mnt/tank/immich-nfs/ /mnt/usb-backup/immich/``. Rsync is useful here because it allows to copy files directly on an old backup because it copies the new and changed files to the HDD and skips up to date files.
10. Verify that the backup worked by comparing the storage sizes ``sudo du -sh /mnt/tank/immich-nfs
sudo du -sh /mnt/usb-backup/immich`` .
11. Flush pending writes ``sync``.
12. Unmount the HDD ``sudo umount /mnt/usb-backup``.
13. Disconnect the HDD.


