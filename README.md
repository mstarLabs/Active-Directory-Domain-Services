# Active Directory Domain Services

## Project Overview

This repository documents the deployment and configuration of Active Directory Domain Services (AD DS) within the Enterprise Identity Security Lab.

Building upon the Enterprise Network Architecture and Enterprise Firewall Platform, this repository introduces centralized identity, authentication, directory services, DNS, and enterprise administration capabilities that become the foundation for all downstream identity and security services.

Rather than documenting Windows Server installation in isolation, this repository demonstrates how enterprise identity infrastructure is deployed onto an existing segmented network while following least-privilege communication, centralized authentication, and enterprise architecture principles.

The repository emphasizes identity-first architecture, enterprise directory design, authentication services, DNS integration, validation, and implementation evidence.

---

## Role Within the Enterprise Identity Security Lab

The Active Directory Domain Services repository represents the third architectural layer of the Enterprise Identity Security Lab.

It implements the centralized identity platform required by downstream repositories including Group Policy, RBAC, Active Directory Certificate Services, Hybrid Identity, Identity Automation, Identity Governance, and Privileged Access Management.

This repository is responsible for documenting:

- Active Directory Domain Services deployment
- Forest and domain architecture
- Domain controller configuration
- Enterprise DNS configuration
- Organizational Unit design
- Identity administration
- Domain-joined workstation integration
- Authentication validation
- Enterprise identity implementation evidence

Network topology, routing, firewall policy, DHCP, NAT, and inter-zone communication remain the responsibility of the Enterprise Network Architecture and Enterprise Firewall Platform repositories.

---

## Architecture

The Active Directory Domain Services repository depends on the network topology, security zones, trust boundaries, routing model, and firewall policies established by the Enterprise Network Architecture and Enterprise Firewall Platform repositories.

Enterprise identity services are introduced into the existing enterprise infrastructure without modifying the underlying network architecture.

The following relationship defines the responsibility boundary between the repositories:

| Repository | Responsibility |
|------------|----------------|
| Enterprise Network Architecture | Defines network topology, addressing, trust boundaries, and communication requirements |
| Enterprise Firewall Platform | Implements routing, DHCP, NAT, and firewall policies required to support enterprise services |
| Active Directory Domain Services | Implements centralized identity, authentication, DNS, directory services, and enterprise administration |

---

## Design Objectives

The Active Directory Domain Services implementation was designed around the following enterprise identity objectives:

- Centralize authentication and authorization.
- Provide enterprise directory services.
- Establish centralized DNS for domain resources.
- Support secure domain-joined workstations.
- Enable least-privilege administrative practices.
- Provide the identity foundation for downstream enterprise services.
- Support future certificate services, hybrid identity, identity automation, governance, and privileged access management.
- Simulate enterprise identity infrastructure within the capabilities of VirtualBox.

---

## Identity Platform

| Component | Configuration |
|-----------|---------------|
| Platform | Microsoft Active Directory Domain Services |
| Operating System | Windows Server 2019 Standard |
| Hypervisor | Oracle VirtualBox |
| Primary Role | Domain Controller |
| Domain | lab.local |
| DNS | Active Directory Integrated DNS |
| Primary Services | Authentication, Authorization, DNS, Directory Services |

Although this implementation uses VirtualBox to simulate enterprise infrastructure, the Active Directory architecture, identity model, authentication flow, DNS hierarchy, and administrative practices closely mirror those used in production enterprise environments.

The supporting network topology, routing, firewall policies, and communication requirements are documented in the Enterprise Network Architecture and Enterprise Firewall Platform repositories.

---

### DC01 (Windows Server 2019)
|  Adapter  | VirtualBox Network  | Purpose           |
|-----------|---------------------|-------------------|
| Adapter 1 | LabNet_VLAN10   | Infrastructure (DC01) |

### Sales_Client (Win 10)
|  Adapter  | VirtualBox Network  | Purpose           |
|-----------|---------------------|-------------------|
| Adapter 1 | LabNet_VLAN20   | Sales Clients (Act as VLAN) |

> **Note:** Each VLAN is mapped to a separate VirtualBox Internal Network. VLAN tagging (802.1Q) is not supported in VirtualBox, so each virtual NIC represents a "VLAN" segment.

---

## Identity Communication Requirements

Active Directory Domain Services depends on the communication paths implemented by the Enterprise Firewall Platform.

Rather than assuming unrestricted communication between security zones, every identity service required for authentication, directory access, DNS resolution, and domain management must be explicitly permitted by firewall policy.

Enterprise identity services introduced by this repository include:

- DNS
- Kerberos
- LDAP
- RPC Endpoint Mapper
- Dynamic RPC
- SMB
- Group Policy processing
- Time synchronization (NTP)

The Enterprise Firewall Platform repository documents how these communication requirements are implemented using least-privilege firewall rules.

This repository documents why those services are required from an identity perspective.

Identity-aware firewall policies, routing behavior, and protocol enforcement remain the responsibility of the Enterprise Firewall Platform repository.

---

## Implementation

### Domain Controller Deployment
 - Navigated to `https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019` and downloaded ISO
 - Open VirtualBox, create new VM selecting the downloaded ISO of Windows Server 2019
 - Provide VM with 2 CPU, 4G RAM, and 30G Storage
 - Configure 1 adapter to simulate VLAN configuration (LabNet_VLAN10)

Ref 1: Windows Server 2019 VM Configuration

![DC01_VM_Configuration](https://github.com/user-attachments/assets/d42e1ba6-36cf-4783-afa5-f31c794c609c)

### Configure Active Directory Domain Services
 - After installation set a static IP to `192.168.10.5` and configure the DNS to `127.0.0.1`
 - Installed `Active Directory Domain Services` via Server Manager
 - Kept default settings
 - Promoted server to `Domain Controller` for the new forest: `lab.local`
 - Switched to `LAB\Administrator` to finish setup

Ref 2: ADDS Promote to Domain Controllor

![DC01_PromoteToDC](https://github.com/user-attachments/assets/56001562-a967-47c3-9311-c44bbf0f625c)

### Validate Identity Infrastructure
 - Verififed Host (A) records in DNS Manager
 - Set DNS forwarders to `8.8.8.8` and `1.1.1.1`
 - Created Organizational Units and test users in ADUC:
   - `HR_Users`
   - `Sales_Users`

Ref 3: DNS Manager

![DC01_DNSManager](https://github.com/user-attachments/assets/6abe636c-e106-4f5d-9bb1-7aa7366a634b)

Ref 4: ADUC Users

![DC01_Users](https://github.com/user-attachments/assets/99be2487-f96b-47f1-aad6-dab0f0c68278)

### Deploy Domain-Joined Client
 - Navigated to `https://www.microsoft.com/en-us/software-download/windows10` and downloaded Windows 10 Installation Media
 - Created a Windows 10 ISO using Microsofts creation media tool
 - Open VirtualBox, create new VM selecting the downloaded ISO of Windows 10
 - Provide VM with 2 CPU, 4G RAM, and 50G Storage
 - Configure 1 adapter to simulate VLAN configuration (LabNet_VLAN20)
 - During instalation selected Windows 10 Pro

Ref 5: Windows 10 VM Configuration

![SalesClient_VM_Configuration](https://github.com/user-attachments/assets/cb9e3cff-6162-431b-8233-fbd0b0cd15f9)

- Check communication from VLAN20_SALES to VLAN10_INFRA ensure they can see eachother in the simulated VLANs with pfSense
- Connect to lab.local domain using the Sales Test user as login

Ref 6: Sales Client Domain Joined

![SalesClient_DomainJoin](https://github.com/user-attachments/assets/a9361b8c-d495-4f32-b953-20e53c124846)

---

## Security Design

The Active Directory Domain Services implementation follows enterprise identity-security principles that support least privilege, centralized authentication, and secure administrative boundaries.

This repository documents the enterprise identity architecture and authentication services implemented within the Active Directory environment. Network topology and firewall-policy enforcement remain documented within their respective repositories.

### Centralized Authentication

Active Directory provides centralized authentication and authorization services for enterprise resources.

### Least Privilege

Authentication traffic is restricted to only the protocols required for approved identity services.

### Centralized Identity Services

DNS, Kerberos, LDAP, RPC, Group Policy, and directory services are centralized on the domain controller while communication is controlled by the Enterprise Firewall Platform.

### Enterprise Foundation

This repository establishes the identity platform required by downstream repositories including Group Policy, Active Directory Certificate Services, Hybrid Identity, Identity Automation, Identity Governance, and Privileged Access Management.

---

## Identity Communication Engineering

During Active Directory deployment, identity-service communication requirements were validated against the least-privilege firewall policies implemented by the Enterprise Firewall Platform.

This process identified several protocol and routing dependencies required for successful domain authentication, DNS resolution, and directory services. The resulting firewall refinements are documented within the Enterprise Firewall Platform repository, while this repository documents why those communication requirements exist.

### Identity Communication Validation

 - Domin join worked right off the bat, but figured this was do to the catch allow all rule at the end
 - When rule was disabled, domain join did not work nor did DNS resolution
 - Discovered pfSense source or destiniaton labled with `interface address` did **not** mean all IPs on that interface
 - Changed all firewall setting to `interface subnet` which did mean all IPs on that interface
 - DNS was still not working after that change so looked at VLAN10 rules to ensure I made the change to subnet as well
 - Even with the change still could not get DNS to work; After a change of destination to any on port 53 and source the DC01
 - DNS started resolving
 - Testing showed that allowing DNS traffic from the domain controller to any destination on TCP/UDP 53 resolved internal DNS communication. Additional investigation into pfSense's state handling and DNS forwarding behavior is planned as the lab evolves.
 - Ran testing in powershell `Test-NetConnection lab.local -Port 389` as well as port `135`, and port `445` These all came back good but domain join still did not work

Ref 7: Tested port connection to DC01

![SalesClient_PortTest](https://github.com/user-attachments/assets/5fc18da7-e9b1-40fe-ae12-85ee914d9128)

### Identity Service Dependency Discovery

 - Discovered an article that provides a list of ports that need to be added to pfSense to allow AD Domain join
 - Made changes to VLAN20
 - I was missing port `135` for RPC endpoint mapper, port `139` for NetBIOS Sessions Service, ports  `49152 - 65535` for dynamic RPC ports port `137` and `138` were optional but added them anyway
 - To get internet I added TCP/UPD port `443` and `80` and got rid of the allow all to any rule
 - On VLAN10 I set a DC to any on port TCP/UDP 53 allow to get DNS working.
 - These changed allowed the Sales client on VLAN20 to resolve DNS and join the domain lab.local

Ref 8: New VLAN10_INFRA Rules
![New_VLAN10_Firewall_Rules](https://github.com/user-attachments/assets/2b872b14-cf0a-41f7-83b8-f481362d29d5)

Ref 9: New VLAN20_SALES Rules
![New_VLAN20_Firewall_Rules](https://github.com/user-attachments/assets/abd17b9f-0f9b-4d04-955a-063148b9461e)

---

## Validation

The Active Directory Domain Services deployment was validated through functional testing to ensure centralized authentication, DNS, directory services, and domain communication behaved as expected.

### Validation Results

| Test | Expected Result | Status |
|------|-----------------|:------:|
| Domain controller promotion | Domain controller successfully promoted into the Active Directory forest | ✅ Passed |
| Active Directory DNS | Domain controller hosts and responds to integrated DNS zones | ✅ Passed |
| Internal DNS resolution | Domain clients successfully resolve Active Directory DNS records | ✅ Passed |
| Domain join | Client successfully joins the Active Directory domain | ✅ Passed |
| LDAP communication | Clients successfully communicate with directory services over LDAP | ✅ Passed |
| Kerberos authentication | Users successfully authenticate using Kerberos | ✅ Passed |
| Organizational Unit structure | Organizational Units support centralized identity administration | ✅ Passed |
| Firewall communication dependencies | Required identity-service protocols successfully traverse the firewall | ✅ Passed |

Successful validation confirms that Active Directory Domain Services integrates correctly with the Enterprise Network Architecture and Enterprise Firewall Platform while establishing the centralized identity services required by future repositories.

---

## Skills Demonstrated

### Enterprise Identity Engineering

- Active Directory Domain Services deployment
- Domain controller configuration
- Active Directory Integrated DNS
- Organizational Unit administration
- Enterprise authentication architecture
- Domain-joined workstation deployment

### Identity Infrastructure

- Active Directory forest deployment
- Enterprise DNS architecture
- Authentication dependency analysis
- Directory service validation
- Domain communication design
- Identity service integration

### Enterprise Security

- Least-privilege identity communication
- Authentication protocol analysis
- Firewall dependency validation
- Segmented identity architecture
- Secure administrative design

### Documentation

- Enterprise identity documentation
- Directory architecture documentation
- Configuration evidence
- Validation documentation
- Cross-repository architectural references

---

## Related Projects

This repository provides centralized policy management, role-based access control, and enterprise security controls for the Enterprise Identity Security Lab.

| Repository | Architectural Relationship |
|------------|----------------------------|
| **[mstarLabs](https://github.com/mstarLabs/mstarLabs)** | Provides the portfolio architecture, governance standards, repository responsibilities, and modernization workflow for the Enterprise Identity Security Lab. |
| **[Enterprise Network Architecture](https://github.com/mstarLabs/Enterprise-Network-Architecture)** | Defines the network topology, trust boundaries, and communication requirements used by this repository. |
| **[Enterprise Firewall Platform](https://github.com/mstarLabs/Enterprise-Firewall-Platform)** | Implements the routing, firewall policies, and least-privilege communication required for centralized policy management. |
| **[Active Directory Domain Services](https://github.com/mstarLabs/Active-Directory-Domain-Services)** | Provides the centralized identity platform, authentication services, Organizational Units, and directory services required for Group Policy and RBAC. |

Additional identity repositories will build upon the centralized identity platform documented here while maintaining the repository responsibilities defined by the Enterprise Identity Security Lab governance framework.

---

## Future Enhancements

The Active Directory Domain Services repository will continue evolving to support additional identity, security, and operational capabilities within the Enterprise Identity Security Lab.

Future architectural requirements include:

- Active Directory Certificate Services
- Hybrid Identity with Microsoft Entra ID
- Identity Automation

As additional enterprise identity services are introduced, this repository will document the Active Directory architecture, enterprise DNS, authentication services, directory design, and identity dependencies required to support them.
