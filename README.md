# Enterprise Active Directory Home Lab

A hands-on enterprise Windows infrastructure and cybersecurity home lab built using Oracle VirtualBox and Windows Server.

The purpose of this project is to simulate a small enterprise environment and gain practical experience with:

* Active Directory Domain Services (AD DS)
* DNS
* Windows Server administration
* Organizational Units (OUs)
* Users and security groups
* Group Policy
* Windows domain clients
* File and folder permissions
* Windows security hardening
* Event logging and auditing
* PowerShell administration
* Enterprise IT and cybersecurity concepts

## Lab Environment

### Host Machine

| Component         | Specification            |
| ----------------- | ------------------------ |
| Operating System  | Windows 11 Home          |
| CPU               | AMD Ryzen 5 5600H        |
| RAM               | 16 GB                    |
| Storage           | 512 GB SSD               |
| Available Storage | ~100 GB                  |
| Hypervisor        | Oracle VirtualBox 7.0.18 |

### Virtual Machines

| Hostname | Operating System    | Role                    | IP Address     |
| -------- | ------------------- | ----------------------- | -------------- |
| DC01     | Windows Server 2022 | Domain Controller / DNS | 192.168.10.10  |
| CLIENT01 | Windows 11          | Domain Client           | 192.168.10.101 |
| CLIENT02 | Windows 11          | Domain Client           | 192.168.10.102 |

> Additional servers and security tools may be added as the lab progresses.

## Domain

```text
corp.home.arpa
```

## Network

```text
Network: 192.168.10.0/24
```

The lab is designed as an isolated virtual enterprise environment using VirtualBox Internal Networking.

## Project Goals

1. Deploy a Windows Server domain controller.
2. Configure Active Directory Domain Services.
3. Configure DNS for the Active Directory domain.
4. Design an enterprise Organizational Unit structure.
5. Create users and security groups.
6. Configure and deploy Group Policy Objects.
7. Join Windows clients to the domain.
8. Configure enterprise file sharing and permissions.
9. Implement Windows security hardening.
10. Configure auditing and event logging.
11. Simulate common enterprise IT and security scenarios.
12. Document the complete implementation for future reference and portfolio use.

## Current Progress

* [x] VirtualBox installed
* [x] Hardware virtualization enabled
* [x] Windows Server 2022 ISO obtained
* [x] DC01 virtual machine created
* [x] Windows Server 2022 installed
* [ ] Configure DC01 hostname
* [ ] Configure static IP
* [ ] Install Active Directory Domain Services
* [ ] Configure DNS
* [ ] Create `corp.home.arpa` domain
* [ ] Create Organizational Units
* [ ] Create users and groups
* [ ] Configure Group Policy
* [ ] Deploy Windows clients
* [ ] Configure file server
* [ ] Implement security hardening
* [ ] Configure auditing
* [ ] Perform security scenarios
* [ ] Finalize documentation

## Disclaimer

This project is an isolated personal lab environment created for educational purposes. Security testing and attack simulations are performed only against systems intentionally created and controlled within the lab.
