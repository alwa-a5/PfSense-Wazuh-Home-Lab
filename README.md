# Cybersecurity Home Lab




# Topology



<img width="680" height="575" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/1bbd6bb8-c4a4-451d-8d31-553b9f34854f" />







# Setting up Pdfsense


Setting up local DHCP range

<img width="720" height="400" alt="FreeBSD version 10 and earlier 64-bit (5)-2026-07-16-20-00-16" src="https://github.com/user-attachments/assets/7b0006c3-4150-4a33-83c2-065ae0a3caf9" />


<img width="720" height="400" alt="FreeBSD version 10 and earlier 64-bit (5)-2026-07-16-20-06-28" src="https://github.com/user-attachments/assets/36d0a2f1-5cc9-42b1-87aa-38df3ee73452" />




















# Setting Up Ubuntu Desktop Wazuh

Pdfsense firewall is working; the Ubuntu Desktop is getting wifif from the internal LAN

<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-16-17-55" src="https://github.com/user-attachments/assets/d77a3897-1e55-49e9-9a51-d4eb11d2aa06" />
Downloading Wazuh

<img width="1280" height="800" alt="Ubuntu 64-bit-2026-07-16-23-40-07" src="https://github.com/user-attachments/assets/d5c3bc95-b959-40dd-bfa9-af091bc7b998" />

Verifying IP which is needed to log into our Wazuh dashboard



<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-19-58-30" src="https://github.com/user-attachments/assets/92d1d42f-ac76-4a2e-b96f-bf47195e3854" />

Logged on to our Wazuh dashboard usin the IP 192.168.10.128

<img width="1718" height="820" alt="Windows 10-2026-07-17-20-44-09" src="https://github.com/user-attachments/assets/79ae50d5-6704-4c29-a805-4e2251d646b4" />
After Downnloading windows 10 machine and setting up Im now downlaoding the wazuh agent onto this machone to capture logs from it
<img width="1718" height="820" alt="Windows 10-2026-07-17-20-46-54" src="https://github.com/user-attachments/assets/843c59b9-2bcf-4e9b-8962-8404d04e2f98" />

<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-20-49-08" src="https://github.com/user-attachments/assets/a8836ff5-efb3-49b5-9a76-ca80af771472" />

Starting Wzuh agent


# Setting Up Attack environment

turning on RDP to be able to attack using FreeRDP
<img width="1718" height="820" alt="Windows 10-2026-07-21-00-20-02" src="https://github.com/user-attachments/assets/6cb355eb-8aee-44e9-9703-2e3ddb28ed6a" />

Allowing port 3389 on my firewall wihtou this the attach would not happen beacsye a firwall automallit blaocn inbound conne ctions


<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-21-01-01-38" src="https://github.com/user-attachments/assets/11480f89-2edd-4722-8659-650508a8afad" />





# FreeRDP Attack
ruuning a nmap scan on my firewall WAN Ip adress the port is openi
<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-22-23-57-09" src="https://github.com/user-attachments/assets/8accf6db-f6a3-4c9e-a814-37674120ea93" />
Using hyrda i cracjed the username and password of the machine in green is the Ip adreees of the vitivm machine and port
<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-23-00-53-25" src="https://github.com/user-attachments/assets/89cf8974-b9c8-4495-9bba-65179bf6bc93" />

After that I ran FreeRDP to gain acseess to the viictim machine which mr having the ability to run various machilious osftware 

<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-23-00-55-26" src="https://github.com/user-attachments/assets/9485c369-b051-4c9f-b774-342c53ec24ea" />


# Visibility and Remindation


Seeing the logs in Wazuh about the and RDP remote login n
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-27-00-14-30" src="https://github.com/user-attachments/assets/3513451e-19a2-43f2-9df3-480ef6cd2115" />

writitng an decection the rule in an xml in and wazuh copnfrigutration in the terminal which will autpmacilly dtect and alert me if the attacknwouldmoccurr again on this amchien in my dashboard
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-27-00-22-28" src="https://github.com/user-attachments/assets/e701f171-9d87-408f-836b-999a65a85783" />

# Project Outcomes




