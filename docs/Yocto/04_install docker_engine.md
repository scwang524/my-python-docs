---
sidebar_position: 5
---

# Install Docker Engine
# （Ubuntu）N200 PC

Add Docker's official GPG key:

```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
```

**install**: Typically used to copy files and set file permissions. Here it is used to create directories. The install command creates the directory and sets specific permissions at the same time.

**-m 0755**: -m is the abbreviation of "**mode**". Options for setting directory permissions. 0755 is an octal value representing the permission setting: 7 means the owner has read, write, and execute permissions (rwx). The second 7 means that users in the same group have read, write, and execute permissions (rwx). 5 means that other users have only read and execute permissions (rx) but no write permissions. Therefore, 0755 sets the owner and group users to fully operate the directory, while other users can only read and execute but not modify it.

**-d**: This option tells the install command to create directories instead of copying files. That is, the command ensures that a directory is created if it does not already exist.

**/etc/apt/keyrings**: This is the directory path to be created, specifically under the /etc/apt directory. /etc/apt is a directory of configuration files related to APT (Advanced Package Tool), which is a tool used to manage software packages in Linux systems.

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

**`curl`**: used to download or transfer data from the Internet. It can support multiple protocols (such as HTTP, HTTPS, FTP, etc.) and can be used in conjunction with other commands.
-fsSL: These are curl options, which stand for:

    -f: If the downloaded resource cannot be successfully obtained (for example, a web page 404 or 500 error), curl will stop and return an error.
    -s: Tells curl to run without displaying any progress or error messages (i.e., run in silent mode).
    -S: Used together with -s, it allows error messages to be displayed, so that even if silent mode is used, error messages can be displayed when errors occur.
    -L: If the resource requested by curl is redirected, this option tells curl to follow the redirect and continue downloading.
https://download.docker.com/linux/ubuntu/gpg: This is the URL of the file that curl will download. This URL points to the GPG key provided on the official Docker website, which is used to verify the origin and integrity of the Docker package.

**`-o`**: This option tells curl to save the downloaded file to the specified path. Here -o is followed by the path where the file is saved.

**`/etc/apt/keyrings/docker.asc`**: This is the path where the downloaded GPG key is saved. docker.asc is the name of the downloaded GPG key file and is saved in the /etc/apt/keyrings directory. This directory is commonly used to store keys for the APT packaging management system.

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

**`chmod`**: command used to change file or directory permissions. It allows users to set read, write, and execute permissions for files or directories.
    
**`a+r`**: This is the permission setting option in the chmod command, which stands for:
             a stands for "all", which includes the file's owner, group users, and other users.
    
**`+r`** means adding read permission to the specified file or directory. This means that all users will be able to read the file, but not necessarily modify or execute it.

Add the repository to Apt sources:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Add the official APT repository source of Docker to the APT configuration of your Ubuntu system to be able to install and update Docker through apt.

### Install the Docker packages

```bash
sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Post-Installation steps for Docker Engine[](https://m11158002.github.io/moil-renesas/docs/note/general/docker#post-installation-steps-for-docker-engine)

1. Create the docker group
    
    ```bash
    sudo groupadd docker
    ```
    
2. Add your user to the docker group
    
    ```bash
    sudo usermod -aG docker $USER
    ```
    
    **`usermod`**: command to modify system users. It allows modifying multiple properties of a user account, such as group, home directory, username, etc.

    **`-aG`**:

    **`-a`**: This option is short for "append", which means adding the user to the specified group without removing the user from other groups. Without this option, the usermod command removes the user from all other groups, leaving only the newly specified group.
        
    **`-G`**: This option is followed by the name of the group to be added. Here, docker means adding the user to the docker group.
    
    **`$USER`**: is an environment variable representing the name of the currently logged in user. When this command is executed, $USER will be replaced with the name of the current user, thus adding the current user to the docker group.
    
    → Add the current user to the docker group so that the user can run Docker commands without sudo. Normally, only users belonging to the docker group can execute Docker commands without sudo, which avoids having to add sudo every time you run a Docker command. After executing this command, the user will need to log back in (log out and back in) for the group change to take effect, or the newgrp docker command can be used to make the change take effect immediately so that you can start using Docker commands without sudo.   

    ```bash
    id -nG | grep docker
    ```
    
Check if the current user belongs to the `docker` group.

**`-id`** command is used to display information about the current user.

**`-n`** is a shorthand argument to `-name` instructing `id` to display group names (rather than numeric group IDs).

**`-G`** parameter displays all groups that the current user belongs to.

So, **`id -nG`** will list all the group names that the current user belongs to.

The **`grep`** command is used to search whether the input contains a specific string.

**`grep docker`** will filter and check if the `docker` group exists.

- If the current user belongs to the `docker` group, the command will output `docker`.
- If the current user does not belong to the `docker` group, there will be no output.
    
3. activate the changes to groups:
    
    ```bash
    newgrp docker
    ```
    
4. Verify that you can run docker commands without sudo
    
    ```bash
    docker run hello-world
    ```
![](../img/04_01.png)