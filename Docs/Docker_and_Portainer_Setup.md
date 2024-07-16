# Docker and Portainer Installation and Setup

## Docker Installation
1. Update Device
    ```bash
    sudo apt update
    sudo apt upgrade
    ```
2. Install Git
    ```bash
    sudo apt install git
    ```

3. Create a Downloads Folder
    ```bash
    mkdir ~/downloads
    cd ~/downloads
    ```

4. Clone the Repository
    >Reference [Link](https://github.com/pi-hosted/pi-hosted)

    ```bash
    git clone https://github.com/pi-hosted/pi-hosted.git
    ```
5. Run the Docker Installation Script
    ```bash
    cd pi-hosted
    ./install_docker.sh
    ```
6. Once completed, Reboot the Device
    ```bash
    sudo reboot
    ```
7. After Reboot, log in and Verify that the User is added to the Docker Group
    ```bash
    groups <Username>
    ```
    - Expected Output
    ```bash
    pi@raspberrypi:~ $ groups pi
    pi : pi adm dialout cdrom sudo audio video plugdev games users input render netdev spi i2c gpio docker
    pi@raspberrypi:~ $  
    ```

## Portainer Installation
1. Once verified, we can install portainer
    ```bash
    cd ~/downloads/pi-hosted
    ./install_portainer.sh
    ```

## Portainer Template Setup
1. Once the Installation is completed, Open port 9000 of the Server to access Portainer Admin Panel. It will ask you to create a User and Password.
2. Now to add the Template, Go to Portainer Settings.
3. Under Templates, paste the URL of the required template from the following [Link](https://github.com/pi-hosted/pi-hosted?tab=readme-ov-file#portainer-architecture)
4. Save Setting


## Portainer Environment IP Setup
1. Open Portainer Dashboard
2. Click environments tab on the sidebar
3. Select reqiured environment (default "local")
4. Under "Public IP" enter the publicly accessible IP, or local IP incase of internal Server
5. Click update environment