- First we need to copy the hashes that we captured into a separate file, here called `hashes.txt`.
- We can now use `john` or `hashcat` to crack the captured hashes.
- Realistically, we don't want to crack hashes on our virtual machine since cracking hashes requires plenty of computational resources.
- Ideally we want to crack hashes using a graphics card, or `crackstation`.

### Cracking NTLM hashes using hashcat
- `hashcat --help | grep NTLM`
- Since we are using NTLMv2, we will note down the code that is specific to that.

```
┌──(root㉿kali)-[/home/kali]
└─# hashcat --help | grep NTLM
   5500 | NetNTLMv1 / NetNTLMv1+ESS                                  | Network Protocol
  27000 | NetNTLMv1 / NetNTLMv1+ESS (NT)                             | Network Protocol
   5600 | NetNTLMv2                                                  | Network Protocol
  27100 | NetNTLMv2 (NT)                                             | Network Protocol
   1000 | NTLM                                                       | Operating System

```

- The code for NTLMv2 hashes is 5600, so we will use that in our command to specify the hash type.
- `hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt -O --show`
- When cracking the hash with rockyou as the wordlist, we should come across the password "Password1".
- If we are not able to get hashcat to run, we can try to force it using the `--force` flag.
- The `-O` flag can be used to ask hashcat to optimize the speed of cracking the hashes specific to the hardware we are using to do so.
- If the hashes have already been cracked earlier, hashcat will display a message that all hashes have been found in the potfile, we can explicitly show them in the command output using the `--show` flag.
- The potfile is where already cracked hashes are saved for reference.
- NTLMv2 hashes for the same user may change on each use, NTLMv1 hashes do not.
- Rockyou-2021 is a more comprehensive wordlist than rockyou for cracking hashes, since it has exponentially more possible values to sift through.
- We can also use rules to mutate the values present in the wordlist and make our cracking more effective, we can use a rule like `OneRuleToRuleThemAll`, by specifying it with the flag `-r`.
- Region and culture specific wordlists are a good idea in real world scenarios.