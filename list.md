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


11. RPi Docker Monitor
	- installastion/container deployement
		- Ref link - https://github.com/pi-hosted/pi-hosted/blob/master/docs/rpi_docker_monitor.md#install-and-setup-instructions-for-the-rpi-docker-monitor
	- open app templates under reqiured environment
	- select RPi Docker Monitor
	- Pre-Installation Steps
		- open the cmdline file
		```
		sudo nano /boot/cmdline.txt
		```
		- Add the following line at the end of the file
		```
		systemd.unified_cgroup_hierarchy=0 cgroup_enable=memory cgroup_memory=1
		```
		- Save the file and reboot
		```
		sudo reboot
		```
		- Confirm that c-groups are enabled
		```
		cat /proc/cgroups
		```
		- You should see output something like this.
		```
		#subsys_name    hierarchy       num_cgroups     enabled
		cpuset  9       15      1
		cpu     7       69      1
		cpuacct 7       69      1
		blkio   8       69      1
		memory  11      158     1
		devices 3       69      1
		freezer 5       16      1
		net_cls 2       15      1
		perf_event      6       15      1
		net_prio        2       15      1
		pids    4       76      1
		rdma    10      1       1
		```
		- The numbers aren't really important what is important is that you see memory in the list if you don't confirm you have put it in the correct file. Don't go on until you get this working.
		
		- Folder Setup
		- Ref Link : https://github.com/pi-hosted/pi-hosted/blob/master/docs/rpi_docker_monitor.md#folder-setup-script

		- First thing we need to do is setup the folder structure and install some files that need to be in place for everything to work correctly.

		- Run the following script
		```
		wget -qO- https://git.io/JPXba | sudo bash
		```

		- Deploy the Container from the App template
		- Once the container is deployed, Open port 3000 of the server
		- Login using the default credentials
			| Username | Password |
			|----------|----------|
			| admin    | admin    |
		- Update your password
	- Setting up grafana
		- once logged in, go to Data source under Connections
		- Click on Add Data Source and then Select Prometheus
		- Set the url to http://monitoring-prometheus:9090/
		- Click on Save and Test
	- Setup the dashboard
		- Go to Dashboards and select import under New button
		- Now we open the [Arm JSON](https://github.com/pi-hosted/pi-hosted/blob/master/configs/rpi_dashboard/arm_rpi_dashboard.json) or [PC(AMD)JSON](https://github.com/pi-hosted/pi-hosted/blob/master/configs/rpi_dashboard/amd_rpi_dashboard.json) file and Click on the "raw" button to copy the content from the json file.
		- Click on load and select Prometheus as the Data source and click Import
		- The Dashboard is ready, now you can share the link using the Share Option

12. QBittorent
	- Update package list
	```
	sudo apt update
	```
	- Install qbittorrent-nox, this is the command line version of qBittorrent with a built-in WebUI feature
	```
	sudo apt install qbittorrent-nox -y
	```
	- Add qbittorrent user to the system, because we will not run this service as a root
	```bash
	sudo useradd -r -m qbittorrent

	# sudo usermod -a -G [groupname] [username]
	sudo usermod -a -G qbittorrent pi
	```
	- Create a service file
	```
	sudo nano /etc/systemd/system/qbittorrent.service
	```
	- Enter the following data
	```
	[Unit]
	Description=BitTorrent Client
	After=network.target

	[Service]
	Type=forking
	User=qbittorrent
	Group=qbittorrent
	UMask=002
	ExecStart=/usr/bin/qbittorrent-nox -d --webui-port=8080
	Restart=on-failure

	[Install]
	WantedBy=multi-user.target
	```
	- Enable the Service
	```
	sudo systemctl enable qbittorrent
	```
	- Start the Service
	```
	sudo systemctl start qbittorrent
	```
	- Open port 8080 to access QBittorrent and login using default credentials
		| Username | Password |
		|----------|----------|
		| admin    | adminadmin    |