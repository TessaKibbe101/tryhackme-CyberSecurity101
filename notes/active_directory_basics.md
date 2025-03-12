# Active Directory Basics

## Windows Domain
- group of users and computers under the administration of a given business
- At the core of the Windows domain is Active Directory (AD), a directory service that acts as a central database
- **Domain Controller (DC)** - server that runs the active directory

## Advantages of a configured Windows Domain
- **Centralized identity management** - all users in the network can be configured via the active directory
- **Managing security policies** - You can configure security policies directly from active directory and apply them to users and computers across the network as needed

### Example of an Active Directory
- When you attend a university, you are given a login and password to log into any of the computers on campus
- How does some random computer know your login?
  - That computer is a part of the active directory which sends your username and password to the active directory for authentication
  - You can’t do certain things on school computers because the administrators are using the active directory to block you

## Core of the Windows Domain is the Active Directory Domain Service (AD DS)  
- Acts as a catalog that holds all the objects that exist on the network like users, groups, machines, printers, shares etc.

## Users
- One of the most fundamental objects
- **Security Principals** - they can be authenticated and can be assigned permissions
- Users in AD are either:
  - **People** aka employees, students, etc.
  - **Service Accounts**
    - Some services require user accounts to run
    - Example: IIS and MSSQL  
    - Assigned the minimal privileges to function aka **principle of least privilege**

## Machines
- For every computer that joins the domain, a machine object is automatically created
- Under **Security Principals** and has an account just like a user
- Machine Accounts are local administrators on the assigned computer
- Not supposed to be accessed by anyone except that computer, but just like a user, if you have the password you can access it
- **Machine account passwords** are automatically rotated out and are generally comprised of 120 random characters
- **Machine Account name** is often the computer's name followed by `$`  
  - Example:  
    - Computer name - `DC01`
    - Machine Account name - `DC01$`

## Security Groups
- A group of users and Machine Accounts that have certain permissions
- You can even add groups to groups
- Certain groups are created by default

### Most important groups in a domain:
- **Domain Admins** - privileges over the entire domain, can administer any computer including DC
- **Server Operators** - can administer DC, can’t modify group memberships
- **Backup Operators** - access to any file, used to perform backups of data
- **Account Operators** - create or modify other accounts on domain
- **Domain Users** - all users on domain
- **Domain Computers** - all machine accounts on domain
- **Domain Controllers** - all existing DCs on domain

## Active Directory Users and Computers
- To configure users, groups, or machines in active directory, we need to log in to the domain controller and run **"Active Directory Users and Computers"** from the start menu
- Objects are organized in **Organizational Units (OUs)**
  - Container objects that allow you to classify users and machines
  - **OUs usually mimic the business’ structure**

### Default containers aka OUs are:
- **Builtin** - default groups available to any Windows host
- **Computers** - machines joining the network are put here by default
- **Domain Controllers** - default for DCs
- **Users** - default users and groups
- **Managed Service Accounts** - holds accounts used by services

## Security Groups vs OUs
- **OUs** are used for applying policies for users and computers depending on their role
- **Security groups** are used to grant permissions over resources

## Authentication Methods
- Credentials are stored in the domain controller
- When a user tries to authenticate to a service using domain credentials, the service will need to ask the domain controller to verify they are correct

### Two protocols can be used for network authentication in Windows domain:
1. **Kerberos** - used in recent versions of Windows, default protocol
2. **NetNTLM** - legacy authentication  
   - NetNTLM is kind of not needed, but most networks use both protocols

## Kerberos Authentication Process
1. **User requests a Ticket Granting Ticket (TGT)**  
   - User wants to log in to a domain-joined computer  
   - User enters username and password  
   - Computer encrypts the timestamp using a key derived from their password hash  
   - Username and timestamp sent to Key Distribution Center (KDC) to request a **TGT**  
   - **KDC issues a TGT**  
   - KDC on domain controller receives the authentication request, verifies the user's identity by decrypting the request using the stored password hash  
   - KDC creates a **TGT** and sends it back to the user  
   - **TGT is encrypted using the krbtgt account’s password hash**, meaning only the KDC can read it  
   - **TGT acts like a temporary password**

2. **User requests a Ticket Granting Service Ticket (TGS)**  
   - User wants to access a specific service on the network like a shared folder, database, or website  
   - Instead of sending their password again, the user sends their **TGT** and **Service Principal Name (SPN)** to the KDC and asks for a **TGS**  
   - KDC verifies the TGT because only the KDC can decrypt it  
   - KDC creates a **TGS**, encrypts it using the service owner's hash, and sends it to the user  
   - **User also receives a service session key**, which they will need to talk to the service  

3. **User authenticates to the service using the service session key**  
   - User sends **TGS** to the desired service to authenticate  
   - Service will use its service owner hash to decrypt the **TGS** and validate the service session key  

## NetNTLM Authentication
- Works using a **challenge-response mechanism**

### Process:
1. Client sends an authentication request to the server they want to access  
2. Server generates a **random number** and sends it as a **challenge** to the client  
3. Client combines their **NTLM password hash with the challenge** to generate a response and sends it back to the server for verification  
4. Server forwards the challenge and response to the **domain controller** for verification  
5. DC uses the challenge to recalculate the response and compares it to the original response sent by the client  
   - If they both match, the client is authenticated, otherwise access is denied  
6. Authentication result is sent back to the server, which forwards it to the client  

## Trees, Forests, and Trusts
- As companies grow, so do their networks, requiring more domains

### Trees
- Active Directory supports integrating multiple domains so that you can partition your network into units that can be managed independently  
- **Tree** - two domains that share a namespace  
- **Enterprise Admins** - grants a user administrative privileges over all the enterprise’s domains  
- Each domain has its own **Domain Admins** with administrator privileges over their specific domain  

### Forests
- If a company grows and acquires another company, they will likely have different domain trees for each  
- **Union of several trees with different namespaces into the same network is known as a forest**  

### Trust Relationships
- **One-way trust relationship**  
  - If domain 1 trusts domain 2, this means a user on domain 2 can be authorized to access resources on domain 1  
- **Two-way Trust Relationships**  
  - Allow both domains to mutually authorize users from the other  
  - By default, joining several domains under a tree/forest will form these  

