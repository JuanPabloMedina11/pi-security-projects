# pi-security-projects
Cybersecurity Projects built on a rasberry pi 

 #1 Created an SSH key pair, private one on my laptop and public one on the Rasberry Pi
 #2 Installed Linux on the Micro SD, for the Rasberry Pi 
 #3 Booted the Pi and made sure everything was up to date utilizing apt update and full -upgrade
 #4 The key hadnt been saved on the Pi, so logins were refused, I briefly allowed password logins, copied the public key over, and then turned passwords off again. SSH now works with the public key.
 #5 With ufw I turned on the firewall allowing only ssh connection as incoming and allows th ePi to make outgoing connections
 #6 Enabled automatic security upgrades with unattended-upgrades 
 #7 Installed log2ram for log files to not wear out the SD Card and are sent to RAM
 #8 Took a security baseline with Lynis and it gave a hardening score of 63 
