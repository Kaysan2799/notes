# Lab 1 

>[!Tip] ### Commands
>1. ipconfig
>2. Ipconfig /all
>3. Ipconfig /renew [Adapter
>4. Ipconfig /release Adapter]
>5. Ipconfig /flushdn
>6. Ipconfig /displaydn
>7. Ipconfig /registerdn
>8. Ipconfig /showclassid Adapte
>9. Ipconfig /setclassid Adapter [Classid]
>10. Ipconfig /?
>11. ping [ip_address]
>12. **tracert**
>13. msg
>14. netstat
>7. Arp -a
>8. 

![[Pasted image 20240214130819.png]]

>[!FAQ] ## Wireshark
>capture traffic
>### **Apply following Filters:**

>[!tip] ## 1. arp
>
>The various fields are explained below:
**Source port:** Port number of the source machine.
**Destination port:** Port number of the destination
**Sequence number:** The sequence number of the segment
**Acknowledgement number:** The acknowledgement number of the segment
**Header length:** Specifies segment’s total header length
**Reserved:** Reserved bits for future use
**Code bits (flags):** Specifies which flags are set based on nature of segment.
**Window size:** Maximum length of segment which the sender can receive as a reply to this segment and starts from acknowledgement number
**Checksum:** Specifies the error detection data
**Urgent:** If set, implies that urgent reply needed from recipient.
**Options:** Can be from 0-32 bits in multiples of eight, used optionally in checksum calculation.
**Segment data:** total data length of the segment
>
>
> ![[Pasted image 20240214191251.png]]

>-> tcp
>-> http
>-> http2
>-> dns
>-> 

## Lab 2

>[!FAQ] Tool:
>### Veracrypt
>https://www.veracrypt.fr/code/VeraCrypt/

A tool used to create encrypted drive.

Symetric and a-semetric

# Lab 2
>[!QAQ]  Tools:
>> VeraCrypt
>> WampServer
>> apache

### To set password policy in linux:
 **apt-get -y install libpam-pwquality cracklib-runtime**
 **gedit /etc/pam.d/common-password**
 Search for :   password requisite pam_pwquality.so retry=3.
 Add: **minlength=8** ucredit=1 dcredit=3 ocredit=1
 
### To set password policy in Windows:
Open **Group Policy Management**
Expend to  **CND.com** and right click on it and click on **Create a GPO in this domain, and Link it here…**
type the name for the new GPO as **“ECCPassword Policy”**
now edit it.
**Computer Configuration --> Policies --> Windows Settings -> Security Settings --> Account Policies**
**Double-click** on the **Password must meet complexity requirements**
check **Define this policy** and enable it then click on explain and select apply and ok
![[Pasted image 20240321145147.png]]
Enforce in by right clicking ECCpassword now add a user to which it should apply

After this open run and type **dsa.msc** select Users, which shows the list of AD users
double click it and go to accounts tab and check **User must change password at next logon**
 
# Lab 3
>[!QAQ]  Tools:
>> Ensuring Secure Email Communication using PGP
# Lab 4
>[!QAQ]  Tools:
>> Group Policy Management Console (GPMC)
>> Stopping unnecessary services services.msc (Windows)
>> ps ax (Linux)
>> Detecting Missing Security Patches using **MBSA** on Windows
>> Conducting Security Checks using **buck-security** on Linux

# Lab 5
>[!QAQ]  Tools:
>> Configuring Syslog Server for Log Review and Audit
>> Kiwi Syslog Server as a Service
>> Remote Log Capture using Splunk and Splunk Universal Forwarder
>> Monitoring Activities on a Remote System using **Spytech SpyAgent**
>> 
# Lab 6
>[!QAQ]  Tools:
>> Auditing System Information using **MSINFO32**
>> nmap
>> iptables (in linux)
>> OSSIM (Open Source Security Information Management)

>[!FAQ] Linux Iptables
>### Show iptable:
>iptables --list
>- - -
>### Flush all rules:
>iptables –F
>- - -
>### Block all incoming communication:
>iptables –P INPUT DROP
>- - -
>### Accept only those incoming connections which are initiated by you
>iptables –A INPUT –m state --state ESTABLISHED,RELATED –j ACCEPT
>- - -
>### Block forwarding:
>iptables –P FORWARD DROP
>- - -
>-### Allow accepting of packets for outgoing connections
>iptables –P OUTPUT ACCEPT
>- - -
>### Block all ping requests in linux:
>sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
>- - -
>###  Restore the iptable configuration at boot time
>iptables-save > /etc/iptables.rules
>- - -
>### To save the configurations permanently
>gedit /etc/network/interfaces
>> **Add this line in table:**
>> pre-up iptables-restore < /etc/iptables.rules

