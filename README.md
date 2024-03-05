> ![NOTE]
> Please visit https://github.com/radxa/backup-sh

# Rockpi-toolkit

Welcome to rockpi-toolkit, the repository will collect tools that can be used on officially supported ubuntu and debian.

## 1. rockpi-backup.sh

This script allows you to back up your system using Rock.

_It may work for unofficial systems, but we do not provide any technical support._

### Install

```bash
linaro@rockpi:~ $ sudo apt install cloud-guest-utils # For auto-expanding the image
linaro@rockpi:~ $ curl -sL https://rock.sh/rockpi-backup -o rockpi-backup.sh
linaro@rockpi:~ $ chmod +x rockpi-backup.sh
```

### Usage

#### Run `./rockpi-backup.sh -h` to print usage. Example for rock-5a.

```bash
root@rock-5a:/home/radxa# ./rockpi-backup.sh -h
Usage:
  sudo ./rockpi-backup.sh [-o path|-e pattern|-u|-m path]
    -o Specify output position, default is $PWD.
    -e Exclude files matching pattern for rsync.
    -u Unattended, no need to confirm in the backup process.
    -m Back up the root mount point, and support backups from other disks as well.
root@rock-5a:/home/radxa#
```
#### If you run it without any arguments, the script will work with the default values and will confirm you.

```bash
root@rock-5a:/home/radxa# ./rockpi-backup.sh
Welcome to rockpi-backup.sh, part of the ROCK Pi toolkit.

  Enter rockpi-backup.sh -h to view help.
  For a description and example usage, see the README.md at:
    https://rock.sh/rockpi-toolbox 

--------------------
Checking disk...
The backup file will be saved at /home/radxa/rock-5a-backup-231031-1015.img
After this operation, 5392 MB of additional disk space will be used.

Do you want to continue? [y/N]
```
#### You can specify output path with provide `-o argument`, if it is a directory, the output file will be directory+date.img, if it is a .img ending file, the output file will be the file.

**If you are using the FAT32 file system, you may encounter issues with backup sizes exceeding 4GB.**
```bash
root@rock-5a:/home/radxa# ./rockpi-backup.sh -o /mnt/backup
Welcome to rockpi-backup.sh, part of the ROCK Pi toolkit.

  Enter rockpi-backup.sh -h to view help.
  For a description and example usage, see the README.md at:
    https://rock.sh/rockpi-toolbox 

--------------------
Checking disk...
The backup file will be saved at /mnt/backup/rock-5a-backup-231031-1030.img
After this operation, 5392 MB of additional disk space will be used.

Do you want to continue? [y/N] 
```
#### You can specify the root mount path with -m mount-point, and it will back up the specified mount point system.


It can back up your system offline, without the need to back up while the system is running, which can avoid some unforeseen errors. Of course, you can directly use dd for a full disk backup, but using our tool can reduce the backup size and automatically expand after recovery, eliminating the need for manual operations.

How to use:
Connect your system media to a Linux system, locate your device, find the root partition, mount it to any location, and execute the command `rockpi-backup.sh -m your_mount_point` to perform the backup.

This operation may require some basic computer knowledge, and you might need to use it under the guidance of a professional.

Other: If your package manager is not apt, you may need to manually install the packages that are prompted as missing.

### Tips

1. If you want to use dd to restore your image, and your uSD or eMMC has GPT partitions and is mounted, please umount before dd.
2. The script only supports GPT disk type and the ext4 file system for the root, and it assumes that the root partition is the largest partition on the disk.
3. If you are using external storage, we strongly recommend that you format it to ext4 or another file system that is natively supported by Linux. If you use an exFAT file system, the process may take longer due to the lack of support for sparse files, which can affect efficiency and storage space utilization.
4. Although the files you have excluded will be accounted for in the total size calculation of the final image, they will not be copied during the backup process. This approach will result in a larger size for the backup image, but it will not have any other additional effects.
5. If you use the exclude command, the rules for exclusion are identical to those used in rsync. Essentially, we are just mapping the exclusion process straightforwardly onto the rsync command.