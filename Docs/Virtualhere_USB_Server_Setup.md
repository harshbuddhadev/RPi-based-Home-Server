# Install VirtualHere USB Server

> Reference [Link 1](https://github.com/virtualhere/script)

> Reference [Link 2](https://www.virtualhere.com/)

1. Run the following script
    ```bash
    curl https://raw.githubusercontent.com/virtualhere/script/main/install_server | sudo sh
    ```

2. Check if the Service is running
    ```bash
    sudo systemctl status virtualhere.service
    ```
3. Now you can access USB devices of this server on various remote Devices


4. For Uninstalling use the following command
    ```bash
    curl https://raw.githubusercontent.com/virtualhere/script/main/uninstall_server | sudo sh
    ```