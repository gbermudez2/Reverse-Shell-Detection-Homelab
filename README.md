# Reverse-Shell-Detection-Homelab


## Creating an Alert to detect Reverse Shell

- It's important to note that this attack is detected by Sysmon as **T1571**, Non-Standard Port.
- In business applications, ports are typically restricted to those that are absolutely necessary, so it's important to create an alert when this happens.
- _**Reverse Shell is a critical exploit, so it should be marked as such!**_

![image](https://github.com/user-attachments/assets/8ce875a8-8613-4783-b908-7a2f1f8e7821)

- I utilized the query shown above to find events related to this port activity, then used the "Save As Alert" option to create the alert shown below.

![image](https://github.com/user-attachments/assets/7a2b29cd-00c9-43da-8241-6ec429c8290d)
