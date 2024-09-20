# zfsonusb

<p align="center">
    <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=SATQ6Y9S3UCSG" target="_blank"><img src="https://img.shields.io/badge/Donate-PayPal-yellow.svg" alt="Donate to project"></a>

ZFS on USB is a shell script that helps you import and export a ZFS pool that resides on external disks
![zfsonusb](https://github.com/user-attachments/assets/b7d8cf7a-0027-490b-af58-88927dceb0d5)

Note that when you export (unmount) your ZFS pool, `zfsonusb` will also power-off the disks so that you can safely unplug your USB cable from your PC.

**The most important thing**, is to have already created the pool using the name of the attached `/dev/disk/by-id/` USB external Disks. So instead of using the common name of the partition e.g. `sda1`, you should refer to your pool with the name which contains the serial number of the drive. This serial number will not change no matter what machine the drive is connected to and it will always be the same as the serial number on the paper label affixed to the drive.

## Installing
To install ZFS on USB, just run the following commands in a terminal 

```
# Move you prompt to your home folder
$ cd

# Clone this repository
$ git clone https://github.com/synergops/.zfsonusb.git

# Copy the conf file to your home folder
$ cp .zfsonusb/.zfsonusb.conf .
$ cp .zfsonusb/.rsync-home-excludes.txt .

# Copy the executable in to your system binaries
$ sudo cp .zfsonusb/zfsonusb /usr/bin/zfsonusb
```
## How to use ZFS on USB

After the installation you can use it in terminal

```
zfsonusb {import|export|list|help}


Options:
  import   : Import the ZFS pool and mount datasets
  export   : Unmount the ZFS pool and power off external disks
  list     : List available disks in /dev/disk/by-id
  backup   : Backup your home folder to the ZFS pool
  help     : Display this help message
```
Once you install it, you need to initialize the `conf` with the appropriate settings

**Steps to use the script for the first time:**
  1. List available disks in `/dev/disk/by-id`:
     ```
     zfsonusb list
     ```
  2. After identifying the disks, edit the `.zfsonusb.conf` file
     to include under the DISKS array the disk paths that correspond to the pool you have previously created.

     Example `.zfsonusb.conf`:
     ```
     ZFS_POOL_NAME="zones"
     DISKS=(
       "/dev/disk/by-id/usb-ST9XXXXXXXXXXX-0:0-part1"
       "/dev/disk/by-id/usb-TOSHIBA_XXXXXXXXXXXXX-0:1-part1"
     )
     ```
     Set the `BACKUP_SOURCE` and `BACKUP_DESTINATION` in the `.zfsonusb.conf` file to the desired values.

     Edit the `.rsync-home-excludes.txt` file to exclude the files and directories that you do not want to backup. All lines starting with a # are ignored by rsync, i.e. those directories will be backed up. 
     The syntax doesn't support comments at the end of a line yet. 
     At the start there is a section with directories that are probably not worth backing up. Uncomment those lines to exclude them as well.

  3. After configuring the `.zfsonusb.conf` file, you can:
     - Import your ZFS pool: `zfsonusb import`
     - Export your ZFS pool and power off disks: `zfsonusb export`
     - Backup your home folder to the ZFS pool: `zfsonusb backup`

