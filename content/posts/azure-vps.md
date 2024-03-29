---
title: "Azure VPS"
date: 2023-03-18T20:46:20+08:00
---

## ssh-keygen

SSH keygen for passwordless login is based on the principles of public-key cryptography and key authentication mechanism. In SSH passwordless login, a pair of keys, including a public key and a private key, needs to be generated. The private key is stored on the local host, while the public key can be shared between the local host and the remote server.

SSH 免密登录的原理基于公钥密码学和密钥认证机制。在 SSH 免密登录中，需要生成一对密钥，分别是公钥和私钥。私钥存储在本地主机上，而公钥则可以在本地主机和远程服务器之间共享。

In SSH passwordless login, the user first needs to generate a pair of keys on the local host and then copy the public key to the remote server. When the user tries to log in to the remote server through SSH, the remote server sends an encrypted challenge to the user, requesting the user to provide the correct key for authentication. The user's SSH client uses the private key on the local host to encrypt the challenge and sends the encrypted result to the remote server. If the remote server can successfully decrypt the encrypted result using the public key, it means that the user has the correct private key and can perform passwordless login.

在 SSH 免密登录中，用户首先需要在本地主机上生成一对密钥，然后将公钥复制到远程服务器上。当用户尝试通过 SSH 登录到远程服务器时，远程服务器会向用户发送一个加密的挑战，要求用户提供正确的密钥以进行身份验证。用户的 SSH 客户端会将本地主机上的私钥用于对挑战进行加密，然后将加密后的结果发送到远程服务器上。如果远程服务器可以成功用公钥解密加密结果，说明用户具有正确的私钥，可以进行免密登录。

```sh
// 在本地主机上生成密钥对。可以使用以下命令生成 RSA 密钥对：
// 在生成密钥对时，需要将公钥文件（默认为 id_rsa.pub）保存在本地主机上。
// Generate a pair of keys (public and private) on the local host using the ssh-keygen command. By default, RSA algorithm is used.
ssh-keygen -t rsa

// 将公钥添加到远程服务器的授权列表中。可以使用以下命令将公钥复制到远程服务器：
// 输入密码后，公钥会被添加到远程服务器的 authorized_keys 文件中。
// Copy the generated public key file (default: id_rsa.pub) to the remote server and add it to the authorized list.
ssh-copy-id user@remote_host

// Test SSH passwordless login. When using the ssh command to connect to the remote server, authentication is completed without entering a password.
ssh user@remote_host
```

## RDP

* [Install and configure xrdp to use Remote Desktop with Ubuntu]

Most Linux VMs in Azure do not have a desktop environment installed by default. Linux VMs are commonly managed using SSH connections rather than a desktop environment. There are various desktop environments in Linux that you can choose. Depending on your choice of desktop environment, it may consume one to 2 GB of disk space, and take 5 to 10 minutes to install and configure all the required packages.

This command installs the XFCE4 desktop environment on a Debian-based Linux system, with the following options:

* sudo: runs the command with superuser privileges
* DEBIAN_FRONTEND=noninteractive: sets the DEBIAN_FRONTEND environment variable to noninteractive, which tells the package manager to run in non-interactive mode and assume the default answer for any prompts.
  * DEBIAN_FRONTEND=noninteractive 是一个环境变量，用于告诉 Debian-based 操作系统的软件包管理器（例如 apt-get）以非交互模式运行。
  * 在交互模式下，软件包管理器会向用户询问有关软件包安装的信息，例如软件包配置选项、默认值和授权等。如果在非交互模式下运行，软件包管理器会自动假定某些值，并绕过用户输入的提示。
  * 因此，当我们在使用 apt-get 等软件包管理器时，设置 DEBIAN_FRONTEND=noninteractive 环境变量可以让软件包管理器在不需要用户交互的情况下自动完成软件包的安装、升级或删除操作。这在自动化脚本中非常有用。
* apt-get: the package manager for Debian-based systems
* -y: automatically answers "yes" to any prompts
* install xfce4: installs the XFCE4 desktop environment package and its dependencies. XFCE4 is a lightweight and customizable desktop environment for Linux.

```sh
sudo apt-get update
sudo DEBIAN_FRONTEND=noninteractive apt-get -y install xfce4
sudo apt install xfce4-session

# Install and configure a remote desktop server
sudo apt-get -y install xrdp
sudo systemctl enable xrdp
sudo adduser xrdp ssl-cert
echo xfce4-session >~/.xsession

sudo service xrdp restart
```

[Install and configure xrdp to use Remote Desktop with Ubuntu]: https://learn.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop?tabs=azure-cli
