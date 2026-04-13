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

## Step 5: T-Pot Utilization

Afetr Sign In the figure 7 should be displayed.

-Attack Map : Provides a dynamic, real-time view of incoming attacks plotted on a world map
- CyberChef: Analyze data transformations, including decoding payloads or viewing suspicious file contents.
- Elasticvue: Manage and query Elasticsearch indices for deeper data insights.
- Kibana : Interactive dashboard and data exploration to visualize attack trends
- Spiderfoot: Perform reconnaissance on attacker IPs to gather context on potential threats.

<img width="937" height="471" alt="Tpot interface" src="https://github.com/user-attachments/assets/578e90f2-fe5f-498d-84ef-b5163f5c27dd" />

Figure 7

## Data Analysis

### Attack Map

In figure 8,
- Hourly Attacks: 147 attempts recorded.
- Last 24 hours Attacks: 204  attempts recorded over the last 24 hours.
- IP Reputation: Suggest known and Unknown  Attacker
- Protocols Attacked : SSH, Telnet, HTTP, IPP and Others
- Primary Source: The highest number of attacks (92) originated from Luxembourg
  
- Insight: The high attack frequency from Luxembourg may indicate concentrated botnet activity or automated scans. Applying geo-blocking or monitoring Luxembourg IP addresses closely may help in mitigating potential threats.

<img width="951" height="473" alt="attack map" src="https://github.com/user-attachments/assets/bd7ad3f3-cb71-44d7-a5bc-60d0721d3075" />

Figure 8 

### Cowrie Dashboard 

In Figure 9,
- Total Attacks: 209 attacks were recorded. 
- Unique IP addresses: 17 were recorded
- Top Attack Sources: Significant attack traffic came from Luxembourg, China, India, Taiwan, the United States, and Russia.
- Protocol Attacked: The main targets were SSH, HTTP and Telnet services, with Telnet experiencing slightly higher engagement.
- Insight: This suggests attackers aim to gain shell access, often using weak credentials to establish persistent connections.
  
- Usernames: Default usernames like root, admin, oracle, and ubuntu were frequently targeted, indicating they are typical entry points in credential-stuffing attacks.
- Password Trends: Commonly used passwords include weak combinations like 123456, admin123, and password, highlighting attackers' reliance on easily guessable passwords.
- Insight: The use of weak or default credentials in attacks highlights the need for strict access control measures, such as enforcing strong password policies and disabling default accounts.


<img width="1701" height="2667" alt="t-pot-kibana-cowrie-dashboard-1" src="https://github.com/user-attachments/assets/073fb676-20ef-4df8-be1a-4c7162c6a606" />

Figure 9 

### Sipderfoot

In fgure 10, a scan is performed, result shows :
- Risk Level : (1) High risk considered malicious with Multiple sources
- Trace IP : 176.65.139.64
- Results of the IP address from AbuseIP Database Website (<a href="https://github.com/user-attachments/assets/13a08233-a0e5-4adf-90a9-c3f0b5e6a15b" />Report</a>)

<img width="694" height="398" alt="spiderfoot-correlatiion" src="https://github.com/user-attachments/assets/dcc1f534-62d8-45b7-99ca-f8c8a5c05175" />

Figure 10


<img width="833" height="422" alt="spiderfoot-correlatiion- high" src="https://github.com/user-attachments/assets/32b9fd3a-6b45-4a05-9f37-f79d32f99448" />

Figure 11

### Security Meter

Figure 12, diplays overview of Alerts on the security meter

- Alerts per Hour : 3231020
- Alerts per 24 hours : 80261094
- Types : Network service, network management
- Ports: 22, 23, 80 etc
- URL : /edge, /root etc

  
<img width="816" height="450" alt="security meter - overview" src="https://github.com/user-attachments/assets/c998f958-f93f-4e91-95cc-900fa2a7dda6" />

Figure 12


## Recommendations 
- Disable unused ports and limit access to critical services.
- Enable multi-factor authentication (MFA) for SSH access.
- Deploy additional honeypot types for a broader attack surface.
- Automate alerting and reporting for higher-priority incidents.
- Conduct regular log reviews to detect unusual access attempts.

