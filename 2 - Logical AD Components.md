### AD DS Schema
- Can be thought of as a rulebook or a blueprint
- Defines every kind of object that can actually be stored in the directory
- Enforces rules regarding object creation and configuration
- Schema is consistent across the entire forest

### Types of objects
- Class objects
	- What objects can be created in the directory
	- Ex: Users, Computers
- Attribute objects
	- Information that can be attached to an object
	- Ex: Display name, Description, Last Logon Time

### Objects
| Object         | Description                                                                     |
| -------------- | ------------------------------------------------------------------------------- |
| User           | Enables network resource access for a user                                      |
| InetOrgPerson  | Similar to a User account, used for compatibility with other directory services |
| Contacts       | Used primarily to assign email addresses to external users; no network access   |
| Groups         | Used to simplify the administration of access control                           |
| Computers      | Enables authentication and auditing of computer access to resources             |
| Printers       | Used to simplify the process of locating and connecting to printers             |
| Shared Folders | Enables users to search for shared folders based on properties                  |

### Domain
- Domains are used to group and manage objects in an organization
- An administrative boundary for applying policies to groups of objects
- A replication boundary for replicating data between Domain Controllers
- An authentication and authorization boundary that provides a way to limit the scope of access to resources

### Tree
- A domain tree is a hierarchy of domains
- There are parent and children domains in a domain tree
- All domains in a domain tree:
	- Can have child domains
	- By default have a "two-way transitive trust" with other domains
	- Share a contiguous namespace, just like subdomains of a single domain in web technologies share the same namespace, for example the following illustration demonstrates two child domains, of a parent domain "corp.local", that share the same namespace, and end in "corp.local".
		corp.local
		├── sales.corp.local
		└── it.corp.local
		- If there are two domains that don't share the same namespace, they belong to different domain trees, for example, shreyas.tech and corp.local don't belong in the same domain tree, and may belong to different trees in a forest.

### Forest
- A forest is a collection of one or more domain trees
- All Forests:
	- Forests share a common schema
	- Share a common configuration pattern
	- Share a common global catalogue to enable searching
	- Enable trusts between all domains in the forest
	- Share the Enterprise Admins and Schema Admins groups
- Compromising a Domain Admin in a domain grants control over that domain only. It does not automatically grant control over the entire forest. However, if you escalate to an Enterprise Admin, you gain privileges across all domains in the forest - and potentially, if trust relationships exist, even cross into other forests.

### Organizational Units (OUs)
- Containers that can contain users, groups, computers and even other OUs.
- OUs are used to:
	- Represent your organization hierarchically and logically
	- Manage a collection of objects in a consistent way
	- Delegate permissions to administer group of objects
	- Apply policies
	- Set different permissions on objects in a OU
- For example, we may have a set of computers that we want to apply specific policies for, and another group of computers that we want to have a completely different set of policies for, we can group them in separate OUs and apply policies to them through their OUs.
  
### Trusts
- Trusts provide a mechanism for users to gain access to resources in another domains.
- All domains in a forest trust all other domains in the forest.
- Trusts can extend outside the forest.
### Types of Trusts:
- Directional
	- The trust direction flows from trusting domain to the trusted domain
		- Domain A (trusting)  --->  Domain B (trusted)
- Bi-directional
	- Both domains trust each other
		- Domain A  <---->  Domain B
- Transitive
	- The trust relationship is extended beyond a two-domain trust to include other trusted domains
		- Domain A <----> Domain B <----> Domain C
		- Transitive Trust allows: Domain A <----> Domain C