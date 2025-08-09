# HoneyPi
### Objective

Create a honeypot using a Raspberry Pi 3 A+.

### Tools
- Raspberry Pi Imager
- Windows PowerShell
- Linux
- DShield
  
## Steps

### Flashing the OS

- Install the Rasperberry Pi Imager.
- Choose "Raspberry Pi OS Lite (64-bit).
- Insert microSD and select on application.
- Use custom OS settings: Enable SSH, set hostname (ie, honeypot.pi), configure Wifi, set password.

### Access and Update Pi


- SSH into your pi using:

 ssh (username)@(yourpiname)
  
<img width="1201" height="460" alt="1" src="https://github.com/user-attachments/assets/a22767c4-db09-4eb0-9c58-c8d47bd6373d" />

- Update the pi:

 sudo apt update && sudo apt-get -uy dist-upgrade

<img width="1465" height="727" alt="2" src="https://github.com/user-attachments/assets/90b27b2d-555c-4494-a751-faea6189266d" />

- Reboot the pi:

 sudo reboot

<img width="1468" height="648" alt="3" src="https://github.com/user-attachments/assets/7e1dd0e7-a306-4309-8b7a-b092285a6cff" />

- Log back into the pi

 ### Install DShield
 

- Install Git:

 sudo apt-get -y install git

<img width="1487" height="657" alt="4" src="https://github.com/user-attachments/assets/652a9c6e-85b3-4730-b491-e41ea1d8b52b" />

- Download DShield:

 git clone https://github.com/DShield-ISC/dshield.git
    
<img width="1476" height="186" alt="5" src="https://github.com/user-attachments/assets/355ad7a5-2ef9-4fb9-ad98-779a9122a9fd" />

- Change directories: cd dshield/bin
- Install DShield:

 ./install.sh
   
<img width="1487" height="647" alt="6" src="https://github.com/user-attachments/assets/0ce049cc-778b-4ee2-ba27-dac38defbd29" />
This interface will eventually pop up. Click yes until prompted for DShield account.

- Go to DShield [website](https://isc.sans.edu/honeypot.html) and sign up for account. Go to my account and input email and API Key

<img width="1469" height="643" alt="image" src="https://github.com/user-attachments/assets/e1161b25-e0b6-4302-9a1f-d381f2e6721c" />
Go throught the next steps using the default options. Write down information for admin use (IP's, port)

- You want to keep these IPs as default or you will not be able to SSH into the pi anymore.
  
<img width="1482" height="653" alt="image" src="https://github.com/user-attachments/assets/eb8093e6-07a8-4f6d-8aa8-4589c44684ab" />

- Reboot Pi:

 sudo reboot

- DShield will have given the pi a new IP and now the hostname (honeypot) will no long work for SSH. To quickly find the new IP plug a display into the Raspberry Pi and the IP will be listed in the shell. Use the IP and port 12222 to SSH from now on: ssh -p 12222 username@ip_address.

## Test and Monitor

Now that the honeypot is set up we will begin running tests to generate data for DShield. For convience these commands should be ran through Linux (Ubuntu, Kali, WSL). Data is uploaded to DShield every 30 minutes. You can view the report logs on DShield's website under my account.

### Port Scanning (Reconnaissance)

- Basic port scan:

 nmap -sS 192.168.5.94
- Aggresive scan with service detection:

 nmap -A 192.168.5.94
- Scan common vulnerable ports:

nmap -p 21, 22, 23, 24, 25, 53, 80, 110, 443, 993, 995 192.168.5.94

### Web-Based Attacks
- Common web vulnerability probes:
  
curl http://192.168.5.94/admin

curl http://192.168.5.94/wp-admin

curl http://192.168.5.94/.env

curl http://192.168.5.94/config.php

curl http://192.168.5.94/phpinfo.php

curl http://192.168.5.94/../../../etc/passwd

- SQL injection attempts
  
curl "http://192.168.5.94/login?user=admin'--"

curl "http://192.168.5.94/search?q=1' OR 1=1--"

### Brute Force Attempts
- SSH brute force simulation
  
ssh admin@192.168.5.94 -p 12222  

ssh root@192.168.5.94 -p 12222

ssh administrator@192.168.5.94 -p 12222


    
