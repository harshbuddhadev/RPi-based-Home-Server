# SSH Setup

## Generating SSH Keys
1. Open any Terminal Window (Powershell/Windows-Terminal/Command-Prompt/Terminal)
2. Navigate to the ssh Folder for the User
    - For Windows PowerShell/Linux
        ```bash
        cd ~/.ssh
        ```
    - For Windows Command Prompt
        ```bash
        cd %homepath%/.ssh
        ```
3. Generate a new RSA key pair using the following command
    ```bash
    ssh-keygen
    ```

4. Enter a Location to Save the ssh keys with the file name
    > **Note:** If no input is given then ssh-key will be saved in the `.ssh` folder with `id_rsa` name <br> 
    If only a file name is provided, then the key will be saved to the current folder with the given name <br>
    If the location is specified along with the key name, then it will be save to the specified location with the given name

5. Enter a Passphrase

    > **Note:** A SSH key can be used without a passphrase, but it is highly recommended to have a passphrase for the ssh key incase of a security breach

6. Expected Output:

    ```text
    PS C:\Users\johndoe\.ssh> ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (C:\Users\johndoe/.ssh/id_rsa): C:\Users\johndoe/.ssh/keyname
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in C:\Users\johndoe/.ssh/keyname
    Your public key has been saved in C:\Users\johndoe/.ssh/keyname.pub
    PS C:\Users\johndoe\.ssh>
    ```

7. Now go the the location where the ssh key is stored, you will see 2 files, a public key and a private key.

8. Now we can use the Keys for Further Setup

## Remote Server SSH Key Setup

1. Connect to the system either physically or via SSH
2. Navigate to SSH folder on remote device
    ```bash
    cd ~/.ssh
    ```
3. create Authorized list
    ```bash
    touch authorized_keys
    nano authorized_keys
    ```
5. Paste the previously generated public key in this file and save the file

## Disabling SSH using Password [Optional]
1. To disable SSH password authentication, SSH in to your server as root to edit this file:
    ```bash
    /etc/ssh/sshd_config
    ```
2. Find the line that says `PasswordAuthentication` and change it to `no`	- 
3. After making that change, restart the SSH service by running the following command as root:

    ```bash
    sudo service ssh restart
    ```