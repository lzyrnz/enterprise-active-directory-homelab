# Active Directory OU Design

## Objective

The purpose of this phase was to design and manually create an enterprise-style Organizational Unit (OU) structure for the `corp.home.arpa` Active Directory domain.

The structure is designed to separate users, computers, servers, groups, service accounts, and administrative accounts.

This separation will allow different Group Policy and security configurations to be applied to different organizational areas in later phases.

---

## Domain

| Property | Value |
|---|---|
| Domain | corp.home.arpa |
| NetBIOS | CORP |
| Domain Controller | DC01 |
| DC01 IP | 192.168.10.10 |

---

## OU Structure

```text
corp.home.arpa
│
├── _Admin
│
├── _Computers
│   ├── IT-Workstations
│   ├── Laptops
│   └── Workstations
│
├── _Groups
│   ├── Distribution-Groups
│   └── Security-Groups
│
├── _Servers
│   ├── Application-Servers
│   └── Member-Servers
│
├── _ServiceAccounts
│
├── _Users
│   ├── Finance
│   ├── HR
│   ├── IT
│   ├── Management
│   └── Sales
│
├── Builtin
├── Computers
├── Domain Controllers
└── Users
OU Purpose
_Admin

Contains administrative accounts that will be used for privileged administrative tasks.

Administrative accounts will be separated from normal user accounts to support a least-privilege model.

_Computers

Contains domain-joined client computers.

Workstations

Standard employee desktop computers.

Laptops

Corporate laptop computers.

IT-Workstations

Workstations assigned to IT personnel.

_Groups

Contains Active Directory groups.

Security-Groups

Security groups used for access control and resource permissions.

Distribution-Groups

Groups intended primarily for communication/distribution purposes.

_Servers

Contains domain-joined member servers.

Member-Servers

General-purpose domain member servers.

Application-Servers

Servers intended to host enterprise applications.

_ServiceAccounts

Contains accounts used by applications, scheduled tasks, and services.

_Users

Contains standard employee accounts organized by department.

Departments:

IT
HR
Finance
Sales
Management
Default Active Directory Containers

The following default containers were left unchanged:

Builtin
Computers
Domain Controllers
Users

The Domain Controllers container was not modified because DC01 is an Active Directory Domain Controller.

The default Users and Computers containers were also preserved because they are part of the default Active Directory structure.

Creation Method

The OU structure was created manually using:

Active Directory Users and Computers (dsa.msc)

PowerShell was intentionally not used to create the OUs in order to gain hands-on experience with Active Directory administration.

Each custom OU was configured with:

Protect object from accidental deletion

enabled.

Design Principles
Separation of User and Computer Objects

User and computer objects are organized separately so that different Group Policy settings can be applied to each.

Department-Based Organization

Users are separated by department to allow department-specific policies and access controls.

Server Separation

Member servers and application servers are placed into dedicated OUs.

Security Group Separation

Security groups are separated from distribution groups to simplify access-control management.

Administrative Account Separation

Privileged accounts are separated from normal user accounts to support least privilege.

Verification

The completed OU structure was verified using:

Active Directory Users and Computers

The complete structure was visually inspected after creation.

Status
 _Admin created
 _Computers created
 _Groups created
 _Servers created
 _ServiceAccounts created
 _Users created
 Department OUs created
 Computer OUs created
 Server OUs created
 Group OUs created
 Default AD containers preserved
 OU structure manually verified
