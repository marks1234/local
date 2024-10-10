# Privilege Escalation

## Overview

In this project, we are tasked with gaining root access to a provided virtual machine (VM) without modifying the GRUB or VM settings. The primary goal is to achieve privilege escalation through the operating system itself. Below is a detailed guide outlining the steps taken to complete this challenge successfully.

## Requirements

- VirtualBox (for x86_64 architectures) or UTM (for ARM-based Apple Silicon CPUs like M1, M2)
- The VM file provided:
  - For x86: `01-Local1.ova`
  - For ARM: `01-Local1.utm.zip`

## Steps for Privilege Escalation

### 1. **Set up Virtual Machine**

- Download the provided VM image based on your system architecture.
- For x86 systems (Intel/AMD CPUs), import the `01-Local1.ova` file into VirtualBox:
- Open VirtualBox.
- Go to **File > Import Appliance** and select the `.ova` file.

### 2. **Boot the VM**

- Start the VM and wait for the operating system to load.
- After boot you should see the login screen.

### 3. **Finding the IP Address**

- Since the VM does not display an IP address by default, you can check the network configuration to retrieve it:
  - Open a terminal on your host machine.
  - Use the following command to discover the IP of the VM within the network (assuming it’s running on bridged network):
    `On windows`

       ```bash
        ipconfig /all
       ```

    - Identify the IP by matching the MAC address of the VM's network interface or checking the IP range assigned by VirtualBox/UTM.

### 4. **Accessing the VM through SSH**

- After identifying the IP address, you should can use any type of portscanner to find the open ports to see what options we have open for us.
  (I found that port 21, 22 and 80 were open)

- After identifying the ports, I attempted to SSH into the VM:

     ```bash
     ssh user@<VM_IP>
     ```

     Replaced `user` with any default user credentials that were possible, and did a brute force attack with the passwords.txt and usernames.txt using hydra.

- After this you will find that ssh is a dead end unless you get lucky.

### 5. ##Accessing the VM through FTP**

- Connecting to the ftp was easier than I would have thought since all you need to do is use an anonymous account!
- Through this you can help yourself to free access to two teasing files you can find by doing `ls`.
- In all seriousness you can now upload a php script that can be access thanks to the fact that port 80 is open by just running the ip address on chrome with the endpoint of your file name! If you configured your script correct you will find you can get access to the computer and look around.

### 6. **Privilege Escalation, After gaining access to an account**

- Once inside, you should be able to find the things that your account has sudo permissions for and since they were so nice they gave us permissions to use python3.5 with sudo access.

    ```bash
    sudo -l
    ```

- Then be clever and run a python script that gives you access to root bash!

### 8. **Capture the Flag**

- After gaining root access, navigate to the root directory to find the flag file, named `flag.txt`:

     ```bash
     cat /root/flag.txt
     ```

## Prohibited Actions

- **Modifying GRUB or VM settings to gain access**
was strictly prohibited, as per the guidelines.
