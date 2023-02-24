<h1 align='center'>Manage Multiple SSH Keys</h1>

<div align='center'>
    <img src="./assets/terminal-logo.png" alt="teminal logo" width="200"/>
</div>

<p align="center">SSH (Secure Shell) keys are an access credential that is used in the SSH protocol and they 
are foundational to modern Infrastructure-as-a-Service platforms such as AWS, Google Cloud, and Azure.</p>

<h1 align='center'>About This Guide</h1>

<p>This guide is designed to help you manage multiple SSH keys for different servers and services. SSH keys are a crucial part of modern infrastructure management, providing a secure way to access remote servers and resources without relying on passwords.</p>

<p>In this guide, you'll learn how to create an SSH key using ed25519 encryption, save it in the .ssh directory, add a passphrase to secure it, and copy it to your server. You'll also learn how to use a configuration file to manage multiple keys and configurations, and how to log in to your server using your SSH key.</p>

<p>Finally, we'll provide some additional tips for securing your server and making sure your SSH access is as safe and secure as possible.</p>

<p>Whether you're a developer, sysadmin, or anyone who needs to manage multiple SSH keys, this guide will provide you with the knowledge and tools you need to get the job done.</p>

**Steps:**

1. Create ssh key using encryption ed25519.
2. Save key inside the .ssh directory.
3. Add a passphrase aka password.
4. Copy key into your server.

## Create ssh key using encryption ed25519

**For this example I will create a ssh key for a Raspberry Pi.**

```bash
ssh-keygen -t ed25519 -C "Raspberry Pi"
```

**Flags:**

**-t**: Specifies the type of encryption.

**-C**: Provides a comment for our ssh key this comment, will be add it at the end of our key.

## Save key inside the .ssh directory

You will be asked where you want to save the key, the path will be **/home/my_user_name/.ssh/raspberrypi_key**, I choose the name **raspberrypi_key** because is easy to identify.

```bash
/home/my_user_name_here/.ssh/raspberrypi_key
```

## Add a passphrase aka password

Secure your key with a passphrase.

You will be ask for this passphrase every time you use the key.

## Copy ssh key into server

To copy our new ssh key into the server we use the command ssh-copy-id followed by -i flag.

```bash
ssh-copy-id -i ~/.ssh/server_key.pub pi@ip_of_the_server
```

**Flags**:

**-i**: Use only the key(s) contained in identity file

## Test SSH Key

Before we continue with the rest of this guide let's test our key.

```bash
ssh -i ~/.ssh/server_key.pub pi@ip_of_the_server
```

Now you should be able to log in with the key instead of using a password.

## Config File

**Steps:**
1. Create a config file.
2. Add config info.
4. Login using the key.

## Create a config file

Now we create a config file for ssh, here is where the configuration for all our servers and keys is save.

```bash
cd .ssh/
touch config
nano config
```

## Add the configuration of your sever

In this file we will add the information necessary for login to our server.

```bash
Host raspberrypi
    HostName ip_of_the_server
    User pi
    Port 22
    IdentityFile ~/.ssh/raspberrypi_key
```

**Save file.**

**Explanation:**

**Host:** this will be the name you will use to login.  
**HostName:** ip of the server. "In this case the ip of my Raspberry Pi"  
**User:** username on the server.  
**Port:** Port for connection.  
**IdentityFile:** path to the raspberrypi_key.  


You can create multiple keys and manage them using the config file.

## Login using the key

Now is possible to login into the Raspberry pi using the raspberrypi_key.

```bash
ssh raspberrypi
```
## Extras:

**For security reasons is recommended access the server only using ssh keys instead of passwords.**

For this we will modify the **sshd_config** file.

Inside of your server modify the file /etc/ssh/sshd_config

```bash
sudo nano /etc/ssh/sshd_config
```

This file is for the sshd daemon (the program that listens to any incoming connection request to the SSH port) on the host machine.

Change these lines.

```bash
PermitRootLogin yes  
PasswordAuthentication yes
#PubkeyAuthentication yes
#ChallengeResponseAuthentication no
X11Forwarding yes 
AllowTcpForwarding yes
```

For:

```bash
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
X11Forwarding no 
AllowTcpForwarding no 
```

To make this changes work you will need to restart the ssh server or you can simple reboot.

**Restart SSH server**

```bash
sudo service sshd restart
```

**Reboot**

```bash
sudo reboot
```

This is a basic secure configuration, other things you can look for are: change the default ssh port, disable unnecessary services, lock the root user, install packages like fail2ban, and ufw.

<p>If you find this guide useful please give it a star and share!</p>
