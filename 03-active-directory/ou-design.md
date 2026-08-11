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
