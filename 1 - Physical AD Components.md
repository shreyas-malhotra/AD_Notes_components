### Domain Controller (DC)
- Has a copy of the AD DS directory store (stores the phonebook)
- Provides authentication and authorization services
- Replicates updates to other DCs in the domain and forest
- Allows administrative access to manage user accounts and network resources (For ex. setting up group polices, changing user password etc.)

### AD DS Data Store
- Contains the DB files and processes that store and manage directory information for users, services, and applications
- Consists of the Ntds.dit file (which can help us pull a lot of information about objects, but most importantly stored password hashes)
- Is stored at %SystemRoot%\NTDS folder on all domain controllers
- Is accessible only through the DC processes and protocols