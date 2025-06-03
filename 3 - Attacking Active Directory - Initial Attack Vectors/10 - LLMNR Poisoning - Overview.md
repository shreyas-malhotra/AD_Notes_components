- Our first attack is LLMNR poisoning, which is the most common attack we may run on internal penetration tests.
- LLMNR stands for link local multicast name resolution, it is enabled by default on networks.
- LLMNR is used to identify hosts when DNS fails to do so.
- Previously was known as NBT-NS.
- Key flaw is that the services' utilize a username and NTLMv2 hash when appropriately responded to.
- Illustration:

![[Pasted image 20250525195207.png]]

- Let's say that we have a victim machine and this victim machine on the network wants to connect to a shared folder called "hackme", but try to connect to "hackm" instead of "hackme".
- The server may not respond in the expected way if a connection request is made to the wrong share, so the victim computer broadcasts to the network and tries to resolve "hackm".
- If we are able to intercept the victim's broadcast request, and respond to that, we can request the victim's hash under the assumption that the attacker will connect it to the share.
- If the intercepted hash is weak enough, we can take the hash offline and try to crack it.
- The share request is just an example, but if LLMNR is enabled, we may find tons of such requests that we may try to respond to, to harvest hashes.
- It is preferable to run this attack in high traffic environments, in the morning, after lunch, during Nessus scans etc.
### Performing an LLMNR Poisoning attack:
- Step 1: Run responder.
	- `sudo responder -I tun0 -dwP`
- Step 2: In our lab, because we are simulating this traffic here, we are actually going to try to go to a file share that just points to our attacker IP address.
	- In such a scenario, we are trying to point ourselves at responder and thus causing an event to occur.
- Step 3: Once responder successfully intercepts the hash, we can see the victim IP, username and NTLMv2 hash.
- Step 4: We can then try to crack the hashes using a tool like "hashcat", if the password is weak enough, as in this example where the password was "Password1".