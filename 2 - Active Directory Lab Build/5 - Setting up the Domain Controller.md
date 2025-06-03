### Lab VM setup
- Just set up a Microsoft Server 2022 VM on your machine, allocate about 4-8 GB of RAM, and 2 CPU cores.
- You can name the VM "HYDRA-DC".

### Overview - AD Certificate Services and LDAP/LDAPS
- Certificate Services are used to verify identities in a DC, for enhanced security. They are standard for a secure profile AD DS installation. They allow us to use LDAP-Secure, abbreviated as LDAPS, which is a more secure protocol that LDAP.
- LDAP is like a phone book, it stands for Lightweight Directory Access Protocol.
- We will setup the DC with Certificate Services.

### Post-installation setup steps
- Just to ease the process and make sure we don't forget it later on, we can use the password `P@$$w0rd!` for the Administrator account.
- Install the necessary VM specific packages for the Hypervisor application you are using by selecting the insert "Guest Additions CD" option while the VM is running, and navigating to the setup file on the CD that gets mounted.
- Rename the PC to "HYDRA-DC", and reboot the system.
- Server Manager should open up on boot, on it, navigate to the "Manage>Add Roles and Features" option, a wizard would open up, on it, we can hit Next, and select "Role-based or feature-based installation", after which we will select our server "HYDRA-DC".
- Now, we will be asked to select the Roles we want to install on the server, we will select the "Active Directory Domain Services" Role, and select "Add Features" on the window that pops up, and click next on the main wizard to get to the feature selection page, which we will also click next on.
- On the Azure Active Directory page, select next.
- On the confirmation page of the wizard, select the "Restart the destination server automatically if required" checkbox, and click Install.
- Once the installation completes, select the "Promote this server to a domain controller" options in the text that shows up.
- Select the "Add a new forest" deployment operation, and specify the root domain name as "MARVEL.local".
- Click next to go to the next step.
- Under forest and domain functional level selection, let the defaults of "Windows Server 2016" be as they are.
- Under DC capabilities, let it stay at the default selection, which is everything but "RODC (Read Only DC)" selected.
- For the Domain Services Restore Mode password, we can use the same password we did for the Administrator account, which was `P@$$w0rd!`.
- On the next page, we will just go with the default selection and click next.
- On the page that comes up now, the NetBIOS domain name "MARVEL" should show up, after it does, we can click next.
- On the next page, we will get the option to select the location of the AD DS database, log files and SYSVOL, we are going to go with the default paths and click next.
- On the review page, we can also just click next.
- At the prerequisite checks page, we should shortly get an all prerequisite checks passed successfully message, following which we can click Install.
- Following this, we would get a reboot prompt, and upon reboot we would be brought to a login screen.
- At the new login screen, we would see our username as MARVEL\Administrator, instead of just Administrator, signifying that we are on the MARVEL domain.
- We can login using the same password that we had setup for the server Administrator account.
- The Service Manager should start up again on boot, we now need to use it to setup Certificate Services, we need to do that in order to run some relay attacks later on.
- In the Service Manager, we will again need to select the "Manage>Add Roles and Features" menu option, and again just click next on the initial page, and select "Role-based or feature-based installation" on the page that follows.
- On the server selection page, we will just select the HYDRA-DC.MARVEL.LOCAL server, which is the DC of our domain.
- Now, on the role selection page, we want to select the "Active Directory Certificate Services" option, and click "Add feature" on the pop-up window, and click next on the wizard menu.
- On the features selection page, we can just hit next.
- On the AD CS brief page, we can also just click next.
- On the "Select role services" page, we will just make sure that "Certification Authority" is selected, and click next.
- On the next page, we will again just select the "Restart the destination server automatically if required" option, and click "Install".
- After the installation is complete, we will get some text on the next steps to follow, here we should just select the "Configure Active Directory Certificate Services on the destination server" option, and the "AD CS Configuration" window should open up.
- We will let the credentials be as is, which should be "MARVEL\Administrator" at this point, and click next.
- On the next page that pops up, we need to make sure that "Certification Authority" is selected and click next.
- On the following pages, select "Enterprise CA" and "Root CA" respectively.
- On the "Private Key" page, select "Create a new private key", for the private key cryptography configuration, we can just use the default selection and click next.
- On the "CA Name" page, just use the defaults and click next.
- On the "Validity Period" page, we can change the period to 99 years if we want to have the lab for a longer term.
- For the CA DB locations, we can just use the default paths and click next.
- On the configuration review page, we can just press "Configure".
- Once the configuration is complete, close everything, reboot your system and login as MARVEL\Administrator.