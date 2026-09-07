<h1>Overview</h1>

EternalRomance is a critical Windows remote code execution exploit created by the NSA and was leaked by The Shadow Brokers in 2017. This exploit targets a type confusion vulnerability (CVE-2017-0143) in the SMBv1 protocol. 

Let's take a look at the machine and find the flag. 

<img width="256" height="256" alt="nsa" src="https://github.com/user-attachments/assets/479d9c0e-74d7-469d-9ce0-f819fbcb3c73" />

<h1>Proof of Concept</h1>

When you have a target, the first thing to do is plan out your attack. Enumeration > Preparation > Exploitation > Privilege Escalation > Post-Exploitation. Enumeration allows you to discover what ports are open and what type of services are running. In the picture, I am using NMAP. NMAP is a network analysis and security auditing tool. It's designed to scan and identify which hosts are available on the network. My go to scan is: **nmap -sV -sC -p- -Pn {IP}** 

**-sV** enables service version detection, **-sC** executes the default of Nmap Scripting Engine which perform safe checks for basic vulnerabilities, SSL details, and service metadata. **-p-** scans all ports. 

I found the Windows machine was on Windows Server 2016 Standard 6.3 which prompted me to Google if this version had any exploits and to my surprise, it was vulnerable to EternalRomance. 

<img width="1494" height="906" alt="1 Scan IP for open ports" src="https://github.com/user-attachments/assets/67afbf1b-6c00-44e0-a3e7-c8d7b860d196" />

I opened up MSFconsole and typed **"search eternal."** The right one to choose is 10: **exploit/windows/smb/ms17_010_psexec**

<img width="1461" height="902" alt="2 Search eternal and select 0" src="https://github.com/user-attachments/assets/6e9165d1-5def-41e5-823f-c498cc7f7a74" />

I typed show options to list out where I need to input my LHOST and RHOST IP addresses. RHOSTS is the remote Windows machine we're exploiting and LHOST is our local machines IP. 

Let's begin the exploit!

<img width="1450" height="866" alt="3 Setting up the exploit" src="https://github.com/user-attachments/assets/460abff8-ad8d-4558-86bd-3f1fdcb775e0" />

WE ARE IN! Let's turn this into an interactive shell and find the flag.txt file

<img width="1336" height="458" alt="4 We are in the system" src="https://github.com/user-attachments/assets/b6e634af-f10d-4e48-8626-5a8c18739d7b" />

We found the flag.txt file.  

<img width="1074" height="884" alt="5 We found the flag" src="https://github.com/user-attachments/assets/fa9212b2-3d0d-42eb-a95a-4a361500ba8a" />

<h2>Remediation</h2>

1. Apply the MS17-010 patch
2. Disable SMBv1
3. Block SMB traffic at the firewall
4. Network segmentation (Isolate legacy or unpatched systems from the general network. It helps reduce lateral movement.)

