# Samba File Server Setup

> Reference [Link](https://pimylifeup.com/raspberry-pi-samba/)

1. Installing Samba
    ```bash
    sudo apt install samba samba-common-bin
    ```
2. Configuring Samba
    ```bash
    sudo nano /etc/samba/smb.conf
    ```
	- add the following lines
	```
	[Quinjet]
	path = /path/to/folder #Update the Path here
	writeable = Yes
	create mask = 0770
	directory mask = 0770
	public = no
	force user = [username] #Optional. Update Username Here
	force group = [group name] #Optional. Update Group Here
	```

3. Set Password for Samba Share
    ```bash
    sudo smbpasswd -a <USERNAME>
    ```

4. Restart Samba Service
    ```bash
    sudo systemctl restart smbd
    ```

5. Now you can access the Samba Share on Remote Devices