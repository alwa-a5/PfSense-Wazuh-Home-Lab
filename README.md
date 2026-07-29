# RDP-Brute-Force-Detection-Home-Lab

For this project I simulated a realistic brute force attack against an exposed remote access service to evaluate the visibility and detection capability of my home SOC setup. I built a segmented network using pfSense as the perimeter firewall, an Ubuntu Server running Wazuh as my SIEM, a Windows 10 machine as the target, and a Kali Linux machine as the attacker. To strengthen visibility on the Windows host, I deployed a Wazuh agent, which was essential for capturing the failed and successful login events generated during the attack. On the Kali attacker machine, I used Nmap for reconnaissance and Hydra to brute force RDP credentials against the victim. I executed the attack while simultaneously reviewing and tuning detection rules in Wazuh to ensure the activity would generate a clear, actionable alert. The lab's primary goal was to demonstrate how a firewall, a SIEM, and an exposed service interact in a real attack path, and how an analyst would detect and respond to it.

# Topology
<img width="680" height="575" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/1bbd6bb8-c4a4-451d-8d31-553b9f34854f" />

# Setting Up pfSense
To begin, I set up pfSense as the firewall separating my attacker environment from my victim environment, configuring the WAN and LAN interfaces and setting a local DHCP range on the internal LAN so my Ubuntu, Windows, and Kali machines could all be issued addresses automatically.

<img width="720" height="400" alt="FreeBSD version 10 and earlier 64-bit (5)-2026-07-16-20-00-16" src="https://github.com/user-attachments/assets/7b0006c3-4150-4a33-83c2-065ae0a3caf9" />
<img width="720" height="400" alt="FreeBSD version 10 and earlier 64-bit (5)-2026-07-16-20-06-28" src="https://github.com/user-attachments/assets/36d0a2f1-5cc9-42b1-87aa-38df3ee73452" />

# Setting Up the SIEM (Ubuntu + Wazuh)
With pfSense routing traffic correctly, I confirmed the Ubuntu Desktop VM was pulling an address from the internal LAN, meaning it was sitting behind the firewall as intended.
<img width="1280" height="800" alt="Ubuntu 64-bit-2026-07-16-23-40-07" src="https://github.com/user-attachments/assets/d5c3bc95-b959-40dd-bfa9-af091bc7b998" />

I then installed Wazuh on the Ubuntu machine to serve as my SIEM, giving me a central place to collect and analyze logs from the rest of the lab.

<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-16-17-55" src="https://github.com/user-attachments/assets/d77a3897-1e55-49e9-9a51-d4eb11d2aa06" />


Using that IP, `192.168.10.128`, I logged into the Wazuh dashboard for the first time.
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-19-58-30" src="https://github.com/user-attachments/assets/92d1d42f-ac76-4a2e-b96f-bf47195e3854" />

After setting up the Windows 10 victim machine, I downloaded the Wazuh agent onto it so it could report logs back to the SIEM.
<img width="1718" height="820" alt="Windows 10-2026-07-17-20-44-09" src="https://github.com/user-attachments/assets/79ae50d5-6704-4c29-a805-4e2251d646b4" />


I started the Wazuh agent service on the Windows machine to begin forwarding events to the manager.
<img width="1718" height="820" alt="Windows 10-2026-07-17-20-46-54" src="https://github.com/user-attachments/assets/843c59b9-2bcf-4e9b-8962-8404d04e2f98" />

Confirmed the agent was online
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-17-20-49-08" src="https://github.com/user-attachments/assets/a8836ff5-efb3-49b5-9a76-ca80af771472" />


# Setting Up the Attack Environment
To make the attack path possible, I enabled RDP on the Windows victim so it could be reached using FreeRDP.
<img width="1718" height="820" alt="Windows 10-2026-07-21-00-20-02" src="https://github.com/user-attachments/assets/6cb355eb-8aee-44e9-9703-2e3ddb28ed6a" />

I then allowed port 3389 through the pfSense firewall. Without this, the attack would never have reached the victim machine, since a firewall blocks inbound connections by default.
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-21-01-01-38" src="https://github.com/user-attachments/assets/11480f89-2edd-4722-8659-650508a8afad" />

# The RDP Brute Force Attack
From Kali, I ran an Nmap scan against my firewall's WAN IP address and confirmed port 3389 was open and reachable from outside the internal LAN.
<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-22-23-57-09" src="https://github.com/user-attachments/assets/8accf6db-f6a3-4c9e-a814-37674120ea93" />

Using Hydra, I brute forced the username and password of the victim machine. The highlighted output shows the IP address of the victim machine, the port, and the valid credential pair that was found.
<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-23-00-53-25" src="https://github.com/user-attachments/assets/89cf8974-b9c8-4495-9bba-65179bf6bc93" />

With valid credentials in hand, I used FreeRDP to gain full remote access to the victim machine, the same level of access a real attacker would have to execute further malicious activity.
<img width="1718" height="820" alt="kali-linux-2025 3-vmware-amd64-2026-07-23-00-55-26" src="https://github.com/user-attachments/assets/9485c369-b051-4c9f-b774-342c53ec24ea" />

# EDR Visibility and Investigation
Going into the Wazuh dashboard, I was able to see the logs generated by the attack, including the failed login attempts leading up to the successful RDP session.
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-27-00-14-30" src="https://github.com/user-attachments/assets/3513451e-19a2-43f2-9df3-480ef6cd2115" />

# Detecting the Attack and Alerting on It
Since the default Wazuh logs on their own were noisy and not immediately actionable, I wrote a custom detection rule directly in the Wazuh configuration on the manager. This rule correlates repeated failed RDP logons within a short time window into a single, high-severity alert, so any future brute force attempt against this machine will be flagged automatically in the dashboard instead of getting lost in the raw event stream.
<img width="1718" height="820" alt="Ubuntu 64-bit-2026-07-27-00-22-28" src="https://github.com/user-attachments/assets/e701f171-9d87-408f-836b-999a65a85783" />

# Project Outcomes
- Built a segmented home lab from scratch using pfSense, Ubuntu, Wazuh, Windows 10, and Kali Linux, with a firewall sitting between the attacker and the victim
- Simulated a realistic external RDP brute force attack, using Nmap for reconnaissance and Hydra to crack valid login credentials
- Gained full remote access to the victim machine using FreeRDP, proving the attack path worked end to end from an outside facing IP
- Used Wazuh to capture and review the actual logs of the attack, including both the failed and successful RDP login events
- Wrote a custom detection rule in Wazuh so this same type of brute force attack triggers an automatic, high-severity alert in the dashboard instead of going unnoticed in the raw logs
- Gained hands-on experience with how a firewall, a SIEM, and an attacker platform interact during a real attack, and how an analyst would investigate and respond to it

Built and Documented by Aluseni Waritay

