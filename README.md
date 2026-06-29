<h1>Sudo misconfiguration Lab</h1>



### Description
This lab walks you through enumerating and exploiting a target. The target has several misconfiguations.
<br />



### Penetration testing tools and Utilities Used:


- <b>Linux CLI</b> 
- <b>Metasploit</b>
- <b>Hydra</b> 
- <b>Nmap</b>
- <b>Searchsploit</b> 


### Environment Used:

### Oracle Virutal Box:

- **Attacker machine** = Kali linux
  
- **Target machine** = Ubuntu 18.04 LTS/ Linsecurity vulnerable linux machine</b>
<br><br>


<h1>Lab:</h1>


### Launch kali Virtual machine:
<br>

<img width="695" height="282" alt="Image" src="https://github.com/user-attachments/assets/38f7315f-77f2-4ef0-b2f1-e29f51780dc4" />


### Launch kali Virtual machine:
<br>

<img width="700" height="499" alt="Image" src="https://github.com/user-attachments/assets/d7ca2c51-35b8-4c28-9402-f56e52a7ac13" />
<br><br>



<h1>Penetration Testing Phase: Reconnaissance:</h1>



### Run network command on kali linux to confirm kali IP address:

ifconfig


<img width="698" height="482" alt="Image" src="https://github.com/user-attachments/assets/bb60f173-b1cb-4048-8686-b3932555658e" />
<br><br>

**Results:** Kali IP is 10.0.2.10 on Ethernet 0.


**Information:** We know we are on 10.0.2.0 network. We can use nmap -sn (no port scan) host discovery option. This option tells nmap to run host discovery only.  Sends ping packets (ICMP echo request) to the remote network. Also, sends ARP request to local network. This way we can discover the host we want to attack.
<br><br>


<h1>Penetration Testing Phase: Scanning:</h1>

### Run nmap scan:

**-v =** verbose output.

**-oN =** save 1stscan.txt as normal format.</h2>

**-sn =** ping scan</h2>


<img width="697" height="805" alt="Image" src="https://github.com/user-attachments/assets/02733f04-d868-4653-b413-30067cbcd52a" />
<br><br>

**Results:** Output shows 4 host are up. 

**Information:** We know our kali IP is .10 device and .1 device is the router. Our target must be .3 or .4 device. 

<h1>Using nmap tool to discover more information about the host:</h1>

nmap -v -sC -sV 10.0.2.3 10.0.2.4 -oN 2ndscan.txt

**-v =** verbose output.

**-sC =** script scan, uses default set of Nmap Scripting Engine (NSE) scripts.

**-sV =** script version scan, tries to discovery the exact applications and version numbers of the services running on a target network.

**-oN =** save 2ndscan.txt normal output.</h2>

<img width="701" height="419" alt="Image" src="https://github.com/user-attachments/assets/89127251-58ae-4b59-9a00-2ee8eb2572bd" />
<br><br>

<img width="698" height="661" alt="Image" src="https://github.com/user-attachments/assets/1693f5d7-41e2-45e3-9e82-0a21cc0616ad" />
<br<br>

**Results:** Output shows 3 ports open. We will focus on the ssh open port.
<br><br>


**Information:** SearchSploit command searches the offline Exploit-DB archive. 

<h1>Penetration Testing Phase: Exploitation (Vulnerability Assessment)</h1>

### Use searchsploit command to search for vulnerabilities:

searchsploit OpenSSH 7.6p1 | tee searchsploit.txt


<img width="1847" height="315" alt="Image" src="https://github.com/user-attachments/assets/6e4da447-9a6d-42e9-824a-eac435c2b0e6" />
<br><br>

**Results:** Searchsploit found a ssh vulnerability for the version this linux machine is running.

### Google searched this vulnerability:

<img width="696" height="527" alt="Image" src="https://github.com/user-attachments/assets/267f4d66-5ba6-4147-8949-74df142f9594" />
<br><br>

**Results:** Output shows 3 ports open. We can will focus on ssh open port.
<br><br>


<h1>Penetration Testing Phase: Exploitation (Metasploit)</h1>

### Lets use metasploit ssh enumeration module to enumerate users:

msfconsole -q

set rhosts 10.0.2.4

set user_file users.txt

<img width="1493" height="825" alt="Image" src="https://github.com/user-attachments/assets/4adeddd2-53ba-4cac-bc5b-4c4cead95b82" />
<br><br>

**Results:** Setup complete, created a wordlist (users.txt) with usernames.

### Run the module:

<img width="694" height="140" alt="Image" src="https://github.com/user-attachments/assets/ddcfa677-8a79-4505-86bc-abc463d6b433" />
<br><br>

**Results:** Output shows 3 ports open. We can will focus on ssh open port.
<br><br>


<h1>Penetration Testing Phase: Exploitation (Hydra tool)</h1>

**Information:** User bob has been found, lets use brute force tool hydra to try to crack the users password with a wordlist.

### Run hydra command:

hydra -l bob -p passwords.txt ssh://10.0.2.4

**-l bob =** user bob 

**-p passwords.txt =** wordlist being used with a list of words to try crack user bob's password.

**ssh://10.0.2.4 =** network protocol being targeted with target host IP</h2>

<img width="1797" height="343" alt="Image" src="https://github.com/user-attachments/assets/9763e24a-e347-4e84-a809-ebabb7a60e21" />
<br><br>

**Results:** Output shows hydra has found password for bob.
<br><br>

### Use bob's credentials to log into linux machine via ssh:

ssh bob@10.0.2.4

<img width="640" height="333" alt="Image" src="https://github.com/user-attachments/assets/d3f4eee2-c897-48d5-82e7-2a4c6e18416a" />
<br><br>

<h1>Penetration Testing Phase: Post-Exploitation (Privilege Escalation)</h1>

### Use sudo command to list the sudo privileges for bob:

sudo -l

<img width="1780" height="257" alt="Image" src="https://github.com/user-attachments/assets/39d29e26-880e-459b-9194-e5bb8615bc87" />
<br><br>

**Results:** Output shows the bob user has the ability to run a bunch of commands as root, without knowing the root password.
<br><br>

**Information:** We can use GTFOtBin site to reference privilege esculation help.


### Go to gitbin site and click on sudo option:

<img width="603" height="477" alt="Image" src="https://github.com/user-attachments/assets/05d2a45c-2908-4081-8bf6-7708133e38f3" />
<br><br>

### Find git command and click on "Shell (Sudo)" option:

<img width="766" height="530" alt="Image" src="https://github.com/user-attachments/assets/b89d5b23-49ee-4bc1-94a8-7da57b50e3b2" />
<br><br>

### Go to inherit section bob has inherited root privileges for git command, lets use this git command to get a privileged bash shell:

<img width="1065" height="768" alt="Image" src="https://github.com/user-attachments/assets/d5b35415-b820-4b5d-80eb-5c07465182b6" />
<br><br>

**Information:** git help config command instructs git to pull up the help manual for its configuration utility.
<br><br>

**Run command:**

sudo git help config

<img width="1037" height="304" alt="Image" src="https://github.com/user-attachments/assets/d90d6f0e-0c66-4295-9046-47b0a3b627ac" />
<br><br>

<img width="1602" height="854" alt="Image" src="https://github.com/user-attachments/assets/79b85476-2d2e-41dc-b336-4ba4341fd8de" />
<br><br>

**Results:** The man page for config utility launches.

### Now on the man page, type !/bin/bash and press enter to execute bash shell:

<img width="1381" height="715" alt="Image" src="https://github.com/user-attachments/assets/71fb0205-071f-41c7-9089-d517436484f7" />
<br><br>

<img width="1078" height="409" alt="Image" src="https://github.com/user-attachments/assets/87ff0689-9d0b-44c2-abc0-57762d27a648" />
<br><br>

**Results:** Bash shell launches and we have root. Ran whoami,groups and id commands to confirm root access.







