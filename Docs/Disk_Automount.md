# Automount Disk on Startup

> Note: Make sure the Disk is Initialized before following this procedure

1. Fetch Disk Name
    ```bash
    lsblk
    ```
- Expected Output
    ```bash
    pi@raspberrypi:~ $ lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda      8:0    0  9.1T  0 disk
    └─sda1   8:1    0  9.1T  0 part 
    sdb      8:16   1 28.7G  0 disk
    ├─sdb1   8:17   1  512M  0 part /boot/firmware
    └─sdb2   8:18   1 28.1G  0 part /
    pi@raspberrypi:~ $
    ```

2. Fetch the UUID for the Device and the Type of Partition
    ```bash
    sudo blkid
    ```
- Expected Output
    ```bash
    pi@raspberrypi:~ $ blkid
    /dev/sda1: LABEL="Label" UUID="UUID" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Basic data partition" PARTUUID="PARTUUID"

    /dev/sdb2: LABEL="rootfs" UUID="UUID" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="PARTUUID"

    /dev/sdb1: LABEL_FATBOOT="bootfs" LABEL="bootfs" UUID="UUID" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="PARTUUID"
    pi@raspberrypi:~ $
    ```

> Note: The UUID is the value in the second column of the output

3. Create a folder to mount the Disk and change its ownership to your user
    ```bash
    sudo mkdir /mnt/Folder
    sudo chown -R pi:pi /mnt/folder
    ```

4. Open the fstab file
    ```bash
    sudo nano /etc/fstab
    ```
5. Paste the following lines at the end of the File and save the file
    ```text
    #update the UUID and Type based on the output in step 2
    UUID=[UUID] [folder location] [TYPE] defaults,auto,users,rw,nofail,noatime 0 0
    ```

6. Manually Mount the Disk
    ```bash
    sudo mount -a
    ```
7. Reload Systemctl Daemon or Reboot the System
    ```bash
    sudo systemctl daemon-reload
    ```
8. Check the Disk is mounted
    ```bash
    lsblk
    ```
- Expected Output
    ```bash
    pi@raspberrypi:~ $ lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda      8:0    0  9.1T  0 disk
    └─sda1   8:1    0  9.1T  0 part /mnt/Folder
    sdb      8:16   1 28.7G  0 disk
    ├─sdb1   8:17   1  512M  0 part /boot/firmware
    └─sdb2   8:18   1 28.1G  0 part /
    pi@raspberrypi:~ $
    ```
