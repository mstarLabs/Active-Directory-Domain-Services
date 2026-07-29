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

- Provisioned the **DC01** virtual machine within the VirtualBox enterprise lab environment.
- Allocated compute, memory, and storage resources appropriate for the simulated enterprise identity infrastructure.
- Connected the virtual machine to the **LabNet_VLAN10** infrastructure network established by the Enterprise Network Architecture repository.
- Prepared the server to host the enterprise Active Directory Domain Services role while maintaining the architectural boundaries established by the Enterprise Identity Security Lab.

> **Implementation Evidence:** Windows Server 2019 virtual machine configuration for the DC01 domain controller.

![DC01_VM_Configuration](https://github.com/user-attachments/assets/d42e1ba6-36cf-4783-afa5-f31c794c609c)

### Configure Active Directory Domain Services

- Configured a static IP address and DNS settings to support enterprise identity services.
- Installed the Active Directory Domain Services server role.
- Created the **lab.local** Active Directory forest and promoted **DC01** to the first domain controller.
- Established the enterprise identity foundation required for centralized authentication, authorization, directory services, and DNS.
- Verified successful domain controller promotion before proceeding with enterprise identity configuration.

> **Implementation Evidence:** DC01 promotion to an Active Directory domain controller for the `lab.local` forest.

![DC01_PromoteToDC](https://github.com/user-attachments/assets/56001562-a967-47c3-9311-c44bbf0f625c)

### Validate Identity Infrastructure

- Verified Active Directory-integrated DNS functionality and host record registration.
- Configured DNS forwarders to support external name resolution.
- Created the initial Organizational Unit hierarchy to support delegated administration and policy application.
- Provisioned test user accounts to validate authentication and downstream identity services.

> **Configuration Evidence:** Active Directory-integrated DNS configuration and host records on DC01.

![DC01_DNSManager](https://github.com/user-attachments/assets/6abe636c-e106-4f5d-9bb1-7aa7366a634b)

> **Configuration Evidence:** Organizational Units and test-user accounts configured in Active Directory Users and Computers.

![DC01_Users](https://github.com/user-attachments/assets/99be2487-f96b-47f1-aad6-dab0f0c68278)

### Deploy Domain-Joined Client

- Provisioned a Windows 10 Enterprise client within the Sales workstation network segment.
- Configured the client to communicate with enterprise identity services through the firewall policies established by the Enterprise Firewall Platform.
- Joined the workstation to the **lab.local** Active Directory domain.
- Verified successful authentication using a domain user account.

> **Implementation Evidence:** Windows 10 virtual machine configuration for the Sales domain client.

![SalesClient_VM_Configuration](https://github.com/user-attachments/assets/cb9e3cff-6162-431b-8233-fbd0b0cd15f9)

- Check communication from VLAN20_SALES to VLAN10_INFRA ensure they can see eachother in the simulated VLANs with pfSense
- Connect to lab.local domain using the Sales Test user as login

> **Validation Evidence:** Successful integration of the Sales client with the `lab.local` Active Directory domain.

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

Identity communication was validated by comparing Active Directory authentication requirements against the least-privilege firewall policies implemented by the Enterprise Firewall Platform.

Validation activities included:

- Verified successful domain communication between the Sales workstation network and the domain controller.
- Confirmed that DNS resolution, LDAP, Kerberos, RPC Endpoint Mapper, SMB, and supporting identity protocols traversed the firewall as expected.
- Evaluated firewall behavior after removing temporary permissive rules to identify the minimum communication requirements necessary for successful domain operations.
- Validated firewall rule configuration using PowerShell connectivity testing and functional domain authentication.

Testing identified several firewall configuration refinements that were required before enterprise identity services functioned reliably within the segmented network.

> **Validation Evidence:** PowerShell connectivity testing verifying Active Directory service communication between the Sales client and DC01.

![SalesClient_PortTest](https://github.com/user-attachments/assets/5fc18da7-e9b1-40fe-ae12-85ee914d9128)

### Identity Service Dependency Discovery

Functional testing identified several communication dependencies that were not initially captured within the enterprise firewall policy.

Analysis determined that successful Active Directory deployment required additional identity-service protocols beyond basic DNS and LDAP communication.

Engineering activities included:

- Identified required RPC Endpoint Mapper (TCP 135) communication.
- Implemented support for Dynamic RPC (TCP 49152–65535).
- Added NetBIOS Session Service (TCP 139) and supporting NetBIOS services where appropriate.
- Refined DNS communication policies to support Active Directory-integrated DNS.
- Removed temporary permissive firewall rules after validating required protocol dependencies.
- Confirmed successful domain join and enterprise authentication using least-privilege firewall policies.

These engineering findings were incorporated into the Enterprise Firewall Platform repository to ensure firewall policy accurately reflects Active Directory communication requirements within the Enterprise Identity Security Lab.

> **Configuration Evidence:** Updated VLAN10 infrastructure firewall rules supporting enterprise identity services.

![New_VLAN10_Firewall_Rules](https://github.com/user-attachments/assets/2b872b14-cf0a-41f7-83b8-f481362d29d5)

> **Configuration Evidence:** Updated VLAN20 Sales firewall rules supporting Active Directory authentication and domain communication.

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

The Active Directory Domain Services repository provides the centralized identity platform for the Enterprise Identity Security Lab by delivering authentication, authorization, directory services, DNS, and identity management capabilities consumed by other enterprise services.

| Repository | Architectural Relationship |
|-----------|----------------------------|
| **[mstarLabs](https://github.com/mstarLabs/mstarLabs)** | Provides the portfolio architecture, governance standards, repository responsibilities, and modernization workflow for the Enterprise Identity Security Lab. |
| **[Enterprise Network Architecture](https://github.com/mstarLabs/Enterprise-Network-Architecture)** | Provides the routing, segmentation, addressing, and communication foundation required to support enterprise identity services. |
| **[Enterprise Firewall Platform](https://github.com/mstarLabs/Enterprise-Firewall-Platform)** | Implements the network communication, routing, and firewall policies required to securely support Active Directory Domain Services. |
| **[Group Policy, RBAC, and Security Controls](https://github.com/mstarLabs/Group-Policy-RBAC-Security-Controls)** | Consumes the centralized identity platform provided by Active Directory to implement enterprise policy management, role-based access control, and security configuration. |

The related repositories above demonstrate how this repository integrates within the Enterprise Identity Security Lab while maintaining clear architectural responsibilities across the portfolio.

---

## Future Enhancements

The Active Directory Domain Services repository will continue evolving to support additional identity, security, and operational capabilities within the Enterprise Identity Security Lab.

Future architectural requirements include:

- Active Directory Certificate Services
- Hybrid Identity with Microsoft Entra ID
- Identity Automation

As additional enterprise identity services are introduced, this repository will document the Active Directory architecture, enterprise DNS, authentication services, directory design, and identity dependencies required to support them.
