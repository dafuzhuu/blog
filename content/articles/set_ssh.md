+++
title = '使用SSH密钥进行身份验证'
date = 2024-08-31T11:09:42+08:00
draft = false
tags = ['Git']
author = "GPT"
+++

SSH（Secure Shell）公钥是用于在网络通信中实现安全身份验证的一种加密方式。

# SSH原理

**非对称加密**：SSH公钥认证使用非对称加密技术，这种技术涉及一对密钥：公钥和私钥。
- **公钥（Public Key）**：可以公开发布，任何人都可以获取。
- **私钥（Private Key）**：必须严格保密，只能由密钥的拥有者持有。

**加密与解密**：
- **加密**：公钥用于加密数据。由于公钥是公开的，任何人都可以使用它加密信息。
- **解密**：只有对应的私钥可以解密由公钥加密的信息。这确保了只有密钥的拥有者（持有私钥的人）才能读取加密内容。

**身份验证过程**：
- **生成密钥对**：用户首先在本地设备上生成一对公钥和私钥。
- **上传公钥**：用户将公钥上传到需要访问的远程服务器（如GitHub、远程Linux服务器）。
- **私钥保密**：私钥保存在用户的本地设备上，不与任何人共享。

**连接与认证**：
- 当用户尝试通过SSH连接到远程服务器时，服务器会生成一个随机数并使用用户的公钥对其加密，然后将加密后的数据发送给用户。
- 用户的SSH客户端使用本地的私钥解密服务器发送的加密数据。
- 如果解密成功，用户客户端将解密结果发回给服务器。
- 服务器验证解密结果是否正确，以确认用户持有相应的私钥。如果正确，身份验证通过。

**确保安全**：
- **安全性**：由于私钥从不在网络上传输，攻击者无法通过截获通信数据来获取私钥。
- **防篡改**：即使攻击者获取了公钥，无法伪装成合法用户，因为他们没有对应的私钥。

**优势**：

- **高安全性**：相比传统的用户名/密码认证，SSH公钥认证更加安全，因为私钥不会在网络上传输，并且密钥对是独一无二的。
- **无密码登录**：SSH公钥认证允许无密码的自动化登录，便于脚本、自动化工具和日常运维。
- **管理便捷**：可以为每个设备或用户设置不同的公钥，并且可以随时管理和撤销。

# 如何设置SSH

## 生成SSH密钥

1. 打开终端并输入以下命令来生成新的SSH密钥：
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

2. 当系统提示时，按回车键以接受默认的文件路径（通常是 `~/.ssh/id_ed25519`）。

3. 接着，系统会要求你设置一个密码短语。你可以选择设置，也可以直接按回车跳过。

## 添加SSH密钥到SSH代理

### macOS

1. 启动SSH代理：
   ```bash
   eval "$(ssh-agent -s)"
   ```

2. 将生成的SSH密钥添加到SSH代理中：
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

### Windows

1. 启动SSH代理：
   
   在菜单栏右击 Powershell，以管理员身份打开。运行

   ```powershell
   Set-Service -Name ssh-agent -StartupType Manual
   Start-Service ssh-agent
   ```

   为确认SSH agent已经在运行

   ```powershell
   Get-Service ssh-agent
   ```

   如果在运行会输出

   ```powershell
   Status   Name               DisplayName
   ------   ----               -----------
   Running  ssh-agent          OpenSSH Authentication Agent

   ```

2. 将生成的SSH密钥添加到SSH代理中

   ```powershell
   ssh-add C:\path\to\your\private\key
   ```

## 将SSH密钥添加到GitHub

1. 查看你的SSH公钥：
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   然后复制输出的内容。
   SSH公钥的格式通常是一个单行的文本字符串，包含了三部分内容：加密类型、加密公钥数据和注释（通常是你的邮箱地址）。以下是一个典型的SSH公钥格式示例：
   ```bash
   ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG8sVpP8h65X8+8Km6BtJQG9d8gY6J7sZ+TpJQeV9XZT your_email@example.com
   ```

2. 登录GitHub，前往 **Settings** -> **SSH and GPG keys** -> **New SSH key**。

3. 在 "Title" 中输入一个名称（如 "My Computer"），并将复制的SSH公钥粘贴到 "Key" 字段中，最后点击 **Add SSH key**。

## 使用SSH方式克隆仓库

1. 现在你可以使用SSH URL来克隆GitHub仓库了：
   ```bash
   git clone git@github.com:username/repository.git
   ```

# SSH的应用

我有两台电脑，但一般只会随身携带一台，所以我需要在两台电脑上的本地同步博客文件夹的信息，等价于在两台电脑上都能pull和push博客所在的GitHub仓库，从而实现同步。我需要在两台电脑上生成并上传SSH公钥。
