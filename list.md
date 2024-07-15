1. SSH Setup
	- SSH into system
	- Generate Keys on Client Device
	- navigate to ".ssh" folder on remote device
	- create Authorized list
	- paste keys in the file
	- disable ssh with password option(optional)
		- To disable SSH password authentication, SSH in to your server as root to edit this file:
			- /etc/ssh/sshd_config
		- Then, change the line
			- PasswordAuthentication yes
		- to
			- PasswordAuthentication no
		- After making that change, restart the SSH service by running the following command as root:
			- sudo service ssh restart
2. Disk Automount
	- use lsblk to find disk name
	- fetch UUID of the device
		- sudo blkid
	- make a folder to mount the disk
	- change ownership of folder
	- open fstab file
		- sudo nano /etc/fstab
	- add the following lines
		- UUID=[UUID] [folder location] [TYPE] defaults,auto,users,rw,nofail,noatime 0 0
	- Mount the disk
		- sudo mount -a
	- Reload Systemctl Daemon
		- sudo systemctl daemon-reload
	- Or reboot

3. Samba Server
	- ref link https://pimylifeup.com/raspberry-pi-samba/
	- install samba
		- sudo apt install samba samba-common-bin
	- sudo nano /etc/samba/smb.conf

	- add the following lines
	```
	[Quinjet]
	path = /path/to/folder
	writeable = Yes
	create mask = 0770
	directory mask = 0770
	public = no
	force user = [username]
	force group = [group name]
	```
	- set password for samba share
	```
	sudo smbpasswd -a <USERNAME>
	```
	- Restarting Samba on your Raspberry Pi
	```
	sudo systemctl restart smbd
	```

4. VirtualHere USB Server
	- run script found on this site
	- https://github.com/virtualhere/script

5. Tailscale
	- run installation command on this link
	- https://tailscale.com/download/linux

	- login using sudo tailscale login

	- enable ip forwarding
	- ref link - https://tailscale.com/kb/1019/subnets#connect-to-tailscale-as-a-subnet-router
	```
	echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
	echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
	sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
	```
	- subnet router
	```
	sudo tailscale up --advertise-routes=192.168.0.0/24,192.168.1.0/24
	```

	- advertise exit node
	```
	sudo tailscale set --advertise-exit-node
	```
	- allow from admin panel
6. Docker/Portainer
	- install git
		```
		sudo apt install git
		```
	- git clone
		- ref link - https://github.com/pi-hosted/pi-hosted
		```
		git clone https://github.com/pi-hosted/pi-hosted.git
		```
	- create downloads folder
	```
	mkdir ~/downloads
	cd ~/downloads
	```
	- git clone
		- ref link - https://github.com/pi-hosted/pi-hosted
		```
		git clone https://github.com/pi-hosted/pi-hosted.git
		cd pi-hosted
		```
	- install docker
	```
	./install_docker.sh
	```
	- reboot device
	- install portainer
	```
	./install_portainer.sh
	```
	- open port 9000
	- create password and login
	- add template
		- ref Link : https://github.com/pi-hosted/pi-hosted?tab=readme-ov-file#portainer-architecture
		- copy template url from git
		- open portainer settings
		- paste url and save settings
	- add environment ip
		- open portainer dashboard
		- click environments tab on the sidebar
		- select reqiured environment (default "local")
		- under "Public IP" enter the publicly accessible IP, or local IP incase of internal Server
		- click update environment
7. Homer *2
	- installastion/container deployement
		- open app templates under reqiured environment
		- select homer
		- run homer installtion script via ssh / or create folders manually as required #put command here with alternative
		- enter docker container name
		- click on show advanced options
		- under host port, enter the port on which you want to access the homer page
		- under volume mapping
			- dont change, if the installation script is used
			- enter location of "assets" folder for the container which has adequate permissions
		- click on deploy the container
	- customization
		- either via sftp/ssh or via VSCode, open the assets folder assigned while deploying the container
8. Adguard
	- installastion/container deployement
		- open app templates under reqiured environment
		- select adguard
		- enter docker container name
		- click on show advanced options
		- Update the Host Ports, specifically
			- 443
			- 80
		- Open port 3001
		- Enter the Port for Web interface, entered in the Docker setup page
	- Open the port specified in setup page
	- under filters, go to DNS Rewrites and enter the data as required
	- Under DNS Filter, Enable all filters or add a custom filter as required
	- Under Settings, Go to client settings and add the devices with the required settings for each device
	- Under Settings, open DNS Settings and enter the Upstream DNS Servers
	- Suggested DNS Servers
	```
	tls://dns.adguard-dns.com
	https://dns.adguard-dns.com/dns-query
	https://dns.google/dns-query
	tls://dns.google
	https://dns.cloudflare.com/dns-query
	tls://1dot1dot1dot1.cloudflare-dns.com
	```
	- Update Router Setting to Point to this DNS Server
	- Restart router to make sure all devices use the updated DNS Server
	- Test the Server using the following command
	```
	nslookup google.com
	```
	The output should contain the Server Address of the Newly setup DNS Server
9. Nginx Proxy Manager
	- installastion/container deployement
		- Run the Pre-installation Script as Mentioned in this [Link](https://github.com/pi-hosted/pi-hosted/blob/master/docs/nginx_proxy_manager.md#pre-installation-steps)
		- open app templates under reqiured environment
		- select Nginx Proxy Manager v2 with Sqllite
		- Update the PUID and PGID as per your user ID
		- Enter the Timezone as "Asia/Kolkata"
		- Deploy the Container
		- Open port 81
		- Default Credentials

	| Username          | Password |
	|-------------------|----------|
	| admin@example.com | changeme |

	- Update the Username and Password
	- Make sure your domain is pointing to the IP address of this server
	- Under Hosts, go to Proxy hosts. Here you can enter the domains or host you would like to reverse proxy
	- Add the required details and click save
	- You can also get an SSL certificate Under the SSL Tab
	- Under Hosts, go to Redirection Hosts. Here you can enter the domains you want to redirect

10. File Browser
	- installastion/container deployement
	- open app templates under reqiured environment
	- select File Browser
	- Click on Show advanced options
	- Under volume mapping change teh host path of "/srv" to the directory which you want to share
	- Open port 8082, and enter the default Credentials

	| Username | Password |
	|----------|----------|
	| admin    | admin    |

	- Go to User Management under Settings. Here you can add/manage users and Update Credentials for all users


<!-- Pending -->
11. RPi Docker Monitor
12. QBittorent
13. Jellyfin
14. Cloudflare