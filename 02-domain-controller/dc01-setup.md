# DC01 Initial Configuration

## Objective

Configure the initial Windows Server 2022 virtual machine that will later become the Active Directory Domain Controller.

## Server Configuration

| Setting          | Value                                   |
| ---------------- | --------------------------------------- |
| Hostname         | DC01                                    |
| Operating System | Windows Server 2022 Standard Evaluation |
| Installation     | Desktop Experience                      |
| CPU              | 2 vCPU                                  |
| RAM              | 4 GB                                    |
| Storage          | 35 GB                                   |
| Network          | VirtualBox Internal Network             |
| Virtual Network  | AD-LAB                                  |

## Network Configuration

| Setting         | Value         |
| --------------- | ------------- |
| IPv4 Address    | 192.168.10.10 |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | None          |
| Preferred DNS   | 192.168.10.10 |

## Network Design

The domain controller is currently connected to an isolated VirtualBox Internal Network named `AD-LAB`.

The network uses:

```text
Network: 192.168.10.0/24
DC01:    192.168.10.10
```

Internet access is intentionally not configured at this stage to keep the Active Directory lab isolated.

## Verification

The following commands were used to verify the network configuration:

```powershell
ipconfig
ping 192.168.10.10
```

Expected result:

* DC01 has the static IP `192.168.10.10`.
* DC01 can successfully ping its own IP address.

## Status

* [x] Windows Server installed
* [x] Hostname configured as `DC01`
* [x] Static IPv4 configured
* [x] Internal network configured
* [x] Network configuration verified
* [ ] Active Directory Domain Services installed
* [ ] DNS configured
* [ ] Domain created
