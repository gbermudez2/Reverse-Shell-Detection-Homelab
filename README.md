# Reverse-Shell-Detection-Homelab

## SETUP
- I used Virtual Box to set up **2 Virtual Machines:**
  1. Kali Linux VM (Pre-configured)
  2. Windows 10 VM

### Windows 10 VM Setup
- Windows 10 is the target machine that I exploited.
- I downloaded Sysmon and a <a href=https://github.com/olafhartong/sysmon-modular>Sysmon config file</a>, and verified their functionality.

![image](https://github.com/user-attachments/assets/74ad5728-8c17-4e50-ac56-5972ca385542)

- I created an account on Splunk to download their Splunk Enterprise trial.
- The dashboard is all set up.

![image](https://github.com/user-attachments/assets/b932a1a4-7e45-45bc-b917-c0d4a785dfde)

### Kali Linux Setup
- It's quite convenient that Kali has a lot of penetration tools at the user's disposal.
- I used a tool called Metasploit to create a malicious payload posing as a harmless PDF.

![image](https://github.com/user-attachments/assets/bd2ff072-7223-45f1-b5e3-21c5b803739f)

- **COMMAND DETAILS**
  - msfvenom is a combination of msfpayload and msfencode, tools for creating payloads and encoding them.
  - -p specifies the payload, in this case, reverse_tcp.
  - LHOST is the attacker's IP. Since this is a closed topology, it's the Kali machine's private IP address.
  - LPORT is the attacker's port of choice for attack. 4444 is a non-standard port.
  - -f is the format of the output file.
  - -o is the name of the output file.
- The file is then placed in my Kali desktop, which will be sent to the target Windows VM.

## Payload Delivery
- Windows Security and Defender Firewall would surely prevent a file like this from setting foot in the machine, so I disabled these services for this.

![image](https://github.com/user-attachments/assets/46004aaf-6dca-426c-8f5d-51ed07399eac) ![image](https://github.com/user-attachments/assets/d049b279-6d76-473c-9f8d-865c55864939) 

- To transfer the file from Kali to Windows easily, I created an HTTP server on my Kali's desktop.

![image](https://github.com/user-attachments/assets/596d9e4a-d5b6-4b7e-98bb-0890473b3d0d)

- On my Windows 10 VM, I used my Kali's IP address and port to download the file.
  - In my case, it's hxxp[://]192[.]168[.]68[.]107:8080/resume[.]pdf[.]exe
  
![image](https://github.com/user-attachments/assets/9a944012-3f7a-4b97-8949-0720066a29d0) ![image](https://github.com/user-attachments/assets/ff3e2492-dc34-42c3-a9a6-f97b88b539f6)

- **_Success!_**

## Splunk Setup

![image](https://github.com/user-attachments/assets/b659d224-1c28-4209-9790-3c1c9f19f16e)

## Creating an Alert to detect Reverse Shell

- It's important to note that this attack is detected by Sysmon as **T1571**, Non-Standard Port.
- In business applications, ports are typically restricted to those that are absolutely necessary, so it's important to create an alert when this happens.
- _**Reverse Shell is a critical exploit, so it should be marked as such!**_

![image](https://github.com/user-attachments/assets/8ce875a8-8613-4783-b908-7a2f1f8e7821)

- I utilized the query shown above to find events related to this port activity, then used the "Save As Alert" option to create the alert shown below.

![image](https://github.com/user-attachments/assets/7a2b29cd-00c9-43da-8241-6ec429c8290d)
