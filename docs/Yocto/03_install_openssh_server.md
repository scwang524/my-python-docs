---
sidebar_position: 4
---

# Install OpenSSH Server
# （Windows）

**Generate ssh key pair:**

```bash
ssh-keygen -t ed25519 -C "renesas-ssh-key"
```

**-t** is the abbreviation of **type**, which is used to specify the encryption algorithm type of the SSH key pair, such as rsa, dsa, ecdsa, ed25519, etc.

Two files will be generated in the ".ssh" directory under the **user's home directory: **id_ed25519**: **Private key file**  **for secure login**, should be kept properly and should not be shared with anyone. **id_ed25519.pub**: **Public key authentication**, which is publicly available and can be added to the remote server's ~/.ssh/authorized_keys file to enable password-free login.

**-C** is used to provide a **comment** for the key pair. This comment is typically used to help identify the purpose or owner of this key pair.

![](../img/03_01.png)

**There are 2 files in the folder of “.ssh”：**

![](../img/03_01_2.png)

### Log into GitHub > Settings > SSH and GPG keys > New SSH key:

![](../img/03_02.png)

# （Ubuntu）N200 PC

**Install OpenSSH Server**

```bash
sudo apt install openssh-server
```

![](../img/03_03.png)

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original  # Back up the original
sudo chmod a-w /etc/ssh/sshd_config.original  # Revoke write permissions for the file for all users
```

**Configure the SSH server**

```bash
sudo nano /etc/ssh/sshd_config
```

**Check for and adjust existing occurrences of these configuration directives, or add new ones, as required:**

- **`Ctrl + W`**：Search keyword
- **`Ctrl + X`**：Exit to save

```bash
# PermitRootLogin prohibit-password → PermitRootLogin no
Default setting (if this line is commented out or set to PermitRootLogin yes): Allow root user to log in with a password via SSH.
Other settings:
PermitRootLogin yes: Allow the root user to log in using a password via SSH. This is not very secure and is generally not recommended.
PermitRootLogin no: completely prohibit the root **** user from logging in through SSH, which will force the system administrator to use other users (such as sudo) to obtain root permissions.
PermitRootLogin prohibit-password**: Allows the root user to log in via SSH, but prohibits login using a password and can only use SSH keys for authentication.

# PasswordAuthentication yes → no  # Disable Password Verification
```

**Check the configuration after changing it before restarting the server:**

```bash
sudo sshd -t -f /etc/ssh/sshd_config
```

**`-t`** parameter is short for test and is used to check the syntax of the configuration file.

**`-f`** parameter is the abbreviation of file and is used to specify the path to the configuration file.
A command to check the syntax of the SSH configuration file without affecting the running SSH service. An effective way to ensure that the modified configuration file does not cause SSH service failure or inability to log in.

**Restart the ssh service to pick up configuration changes:**

```bash
sudo systemctl try-reload-or-restart ssh
```

This command is used to reload or restart the SSH service (usually refers to the sshd service).

**import ssh key from github:**

```bash
ssh-import-id-gh <github-username>
```

Import the public key from GitHub (also convenient for using GitHub in the future)
After importing, the public key is stored in **`~/.ssh/authorized_keys`** → you can **`cat`** browse or **`nano`** edit it

### Check the server ip address using **`ip a`**

# （Windows）

*Connect to the server using ssh:*

```bash
ssh <username>@<server-ip>
```