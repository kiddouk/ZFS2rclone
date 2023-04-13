# ZFS2rclone

Allows you to send your ZFS snapshots to rclone and use the desired backend to store the files and restore them "where you want". This tool should be used in conjunction with zfs-auto-snapshot that create frequent/hourly/daily/monthly snapshot.

Backup
=====
The script manages its own instance of rclone or can be asked to use a running instance of rclone rcd. The zfs snapshot is split in multiple archives of about 2Gb that are sent to the rclone backend of your choice (see -l and -r options). You can you the -j argument to control how many parallel tasks are ran since the upload part will most likely be the bottle neck.

When ran multiple times, this tool find the last know backup and create and incremental backup instead of a full backup.



Note :
* provide the systemd file that triggers the service once the daily snapshot is created.


Restore
=====
Restoring takes an extra argument -c which tells the software on where to restore the backup. Example : ./backup_script.sh restore -c "zfs recv -duFv tank" -r pcloud:backup/hostmachine -n znap-daily-2023-02-03-22-30 rpool/home.

This will load the snapshot zsnap-daily-2023-02-03-22-30 (and all of its parents) and pipe it to "zfs recv -duFv tank" on the same machine.

