# Tailscale Setup for Linux

> Reference [Link](https://tailscale.com/download/linux)

1. Run the Installation Script
    ```bash
    curl -fsSL https://tailscale.com/install.sh | sh
    ```
2. Once installed, Log in to Tailscale
    ```bash
    sudo tailscale login #This Command will give you the link

    sudo tailscale login --qr #This Command will give you the QR for the Link which you can scan
    ```

3. Once logged in, we need to Enable IP Forwarding
    ```bash
    echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf

    echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf

    sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
    ```

4. Enabling Subnet Router
    ```bash
    sudo tailscale up --advertise-routes=192.168.1.0/24
    #Change 192.168.1.0 to your preferred IP Address and 24 to your preferred Subnet Mask
    ```

5. Enabling Exit Node
    ```bash
	sudo tailscale set --advertise-exit-node
    ```

6. Open the [Tailscale Admin Panel](https://login.tailscale.com/admin/machines) from the Web browser and Allow the Device for Subnet Routes and Exit Node