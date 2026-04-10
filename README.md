# Honeypot (T-Pot) Project Using AWS

## What is a Honeypot ? 
A honeypot is a decoy system designed to look like a legitimate target in order to lure attackers, monitor their activities, and analyze their methods without risking real assets.

## Overview
The overview of this project is to deploy a honeypot using the T-Pot on an Amazon Web Service EC2 instance to capture and analyze network-based attacks. T-Pot integrates multiple emulated services—including Dionaea, Cowrie, and Suricata—with data visualization through Kibana. The aim is to identify common attack patterns, characterize attacker behaviors, and recognize frequently targeted vulnerabilities.

## Obejectives

- Deploy honeypots on AWS to simulate vulnerable services.
- Capture data on attack attempts, including IP addresses, protocols, and attack vectors.
- Use visualization and analysis tools to interpret and report on attacker behaviors.
- Provide recommendations to improve security based on observed attack patterns.

  ## Tools
  - AWS EC2 Instance
  - T-pot Attack Map
  - CyberChef
  - Spiderfoot
  - Elasticvue
  - Kibana
  - Secuirty Meter
 
  # Setup Instructions

  ## Setup 1 : Launch AWS Instance

Figure 1 shows an AWS EC2 Named Honeypot and Ubuntu as the operating system to be used.

  <img width="616" height="353" alt="create vm 1" src="https://github.com/user-attachments/assets/194addad-85f7-4f46-be82-8548250a0ce5" />

  Figure 1: Launch an EC2 Instance

  In figure 2, a t2.medium instance type is selected and a Key pair name "honeypot" is created. Download the key file and remember it location.

  <img width="593" height="251" alt="create vm- select instance type" src="https://github.com/user-attachments/assets/ca9e82da-3251-4752-804b-c04609b912f7" />
  
  Figure 2:  Instance Type & Key Pair

In Figure 3, auto- assign public IP is enabled,create security group and allow SSH traffic form "My IP" is selected. The storage size is set to 50 GB. After configurations are set the Instance is Launched.  

  <img width="457" height="380" alt="create vm -networking and storage" src="https://github.com/user-attachments/assets/4fa6d533-01c2-494c-8a32-9fde17b7143c" />

Figure 3 : Network Settings

## Step 2 : Security Configurations

IN figure 4 : Now navigate to Security Groups and Edit Inbound Rules then set these parameters

<img width="920" height="324" alt="inbound2" src="https://github.com/user-attachments/assets/8faad769-7e43-4e22-a6f3-a08ed69596bc" />

figure 4 : Edit Inbound Rules


## Step 3 : SSH Access

- Navigate to the instance created and click "Connect"
- Locate the downloaded SSH Key file (eg honeypot.pem)
- If using a Linux/MacOS for you local computer, First set the following permissions : chmod 400 honeypot.pem (if using windows, there is no need for command prompt)
- Connect to instance : ssh -i "honeypot.pem" ubuntu@ <your-ec2-instance-ip>

  <img width="844" height="132" alt="connect remotely" src="https://github.com/user-attachments/assets/fedaf39f-28b9-4430-accc-98fb9f65da2c" />

  Figure 5 : Connet Remotely via SSH


## Step 4 : Install T-Pot
- Run the following commands after accessing the Ubuntu EC2 Instance
 1. sudo apt update (Updates Repositories and packages)
 2. sudo apt upgrade -y (Upgrades Repositories and packages)
 3. sudo apt install git (Install git)
 4. sudo git clone https://github.com/telekom-security/tpotce (Clone T-pot Repository
 5. sudo cd tpotce (Navigate to Folder)
 6. sudo ./install.sh (Install T-pot Script)

#### The Installer will do the following:
- Change the SSH port to tcp/64295.
- Disable the DNS Stub Listener to avoid port conflicts.
- Set SELinux to Monitor Mode.
- Set the firewall target for the public zone to ACCEPT.
- Install Docker, recommended packages, and remove conflicting packages.
- Add the current user to the docker group.
- Add helpful aliases for Docker commands and file navigation.
- Display open ports on the host and compare with T-Pot's required ports.
- Add and enable tpot.service to automatically start T-Pot on boot.

  ### Reboot System

  - After installation reboot the system using the following the command : sudo reboot
  - After rebooting, T-Pot should be fully installed and configured on your EC2 instance

 ## Step 5 : Access T-pot Website
 
- Access T-pot website via port 64297 (https://ip-address:64297), use same IP address on Instance page.

In figure 6, Sign in using the username and password from the T-pot installation process

<img width="239" height="191" alt="web interface" src="https://github.com/user-attachments/assets/8cacb111-efba-41de-8f89-f36093fac74c" />

Figure 6 : Sign In


