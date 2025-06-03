### Lab VM setup
- Just set up two Windows 10 Enterprise VM on your machine, allocate 4 GB of RAM, and 2 CPU cores to each.
- You can name the VMs "THEPUNISHER", and "SPIDERMAN".
- Select "Windows 10 Enterprise LTSC Evaluation" while installing Windows, do not select the N variant.
- After installation, use the "Domain join" option for user setup, and create local accounts named "frankcastle" for "THEPUNISHER" and "peterparker" for "SPIDERMAN", with "Password1" as their passwords, use "bob" as the answer to any security questions you select.
- Disable all opt out telemetry options, and cortana.
- Install the necessary VM specific packages for the Hypervisor application you are using by selecting the insert "Guest Additions CD" option while the VM is running, and navigating to the setup file on the CD that gets mounted.
- Rename the PCs to "THEPUNISHER" and "SPIDERMAN" according to their VM names, from Windows settings and reboot them.