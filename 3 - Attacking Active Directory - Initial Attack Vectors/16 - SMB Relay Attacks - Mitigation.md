### Enable SMB Signing on all devices
- Pro: Completely stops the attack
- Con: Can cause performance issues with file copies (~10-20% degradation)

### Disable NTLM authentication on network
- Pro: Completely stops the attacks
- Con: If Kerberos stops working, windows by default falls back to NTLM based Authentication

### Account tiering
- Pro: Limits domain admins to specific tasks (e.g. only log onto servers with the need for DA)
- Con: Enforcing the policy may be difficult

### Local admin restriction
- Pro: Can prevent a lot of lateral movement
- Con: Potential increase in the amount of service desk tickets