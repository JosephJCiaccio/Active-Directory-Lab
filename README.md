# Active Directory Lab

## Enterprise Windows Network in VirtualBox

---

## Overview

This project documents the design, deployment, configuration, and validation of a fully functional Active Directory home lab built using Oracle VirtualBox. The lab simulates a small-to-medium enterprise Windows network environment and mirrors how corporate IT infrastructures are commonly deployed and managed in real-world organizations.

The environment was designed to provide hands-on experience with centralized authentication, directory services, networking, automation, and troubleshooting. While the overall architecture is inspired by common public homelab designs, all installation steps, configurations, testing, and issue resolution were completed independently to reinforce practical understanding.

The completed lab includes:

- A Windows Server Domain Controller running Active Directory Domain Services (AD DS)
- Integrated DNS, DHCP, and NAT routing services
- A Windows 10 client machine joined to the domain
- Centralized user authentication and device management
- Automated user provisioning using PowerShell
- A private internal network that emulates a corporate LAN

This lab was built to strengthen enterprise IT skills relevant to help desk, desktop support, system administration, and junior infrastructure roles.

---

## Lab Objectives

The primary goals of this lab were to gain both theoretical and practical understanding of how enterprise Windows environments operate. Specific objectives included:

- Understanding how Active Directory authenticates users and computers
- Learning how DNS and DHCP function within a Windows domain
- Designing a private internal network similar to a corporate LAN
- Configuring NAT routing to allow internal systems internet access
- Practicing PowerShell automation for user management
- Observing how domain-joined client machines behave in a business environment
- Developing troubleshooting skills for common Active Directory and networking issues

---

## Lab Architecture & Design

### Network Design

The lab uses a dual-network architecture, which closely resembles how many enterprise environments are structured:

1. External Network (NAT)  
   - Provides internet access to the lab environment  
   - Simulates an ISP or upstream network  

2. Internal Network (Private LAN)  
   - Isolated network used for Active Directory, DNS, and DHCP traffic  
   - Represents a secure corporate internal network  

The Domain Controller (DC) sits between both networks and acts as:

- The authentication authority
- The DNS and DHCP server
- The default gateway for internal clients
- The NAT router providing internet access

Traffic Flow:

Windows 10 Client → Internal Network → Domain Controller (AD, DNS, DHCP, NAT) → External Network → Internet

This design ensures that all domain-related traffic stays internal while still allowing controlled internet connectivity.

---

## Virtual Machines

### Domain Controller (DC)

- Operating System: Windows Server 2019  
- RAM: 4 GB  
- Storage: 50 GB  
- Network Adapters:  
  - Adapter 1: NAT (External / Internet)  
  - Adapter 2: Internal Network (Private LAN)  

After installation, the server was:

- Renamed to DC
- Assigned a static IP address on the internal network
- Configured to use itself as the DNS server

Static IP Configuration (Internal Adapter):

- IP Address: 172.16.0.1  
- Subnet Mask: 255.255.255.0  
- DNS Server: 172.16.0.1  

A static IP is critical for domain controllers to ensure reliable DNS and authentication services.

---

### Client Machine (CLIENT01)

- Operating System: Windows 10  
- RAM: 4 GB  
- Storage: 50 GB  
- Network Adapter: Internal Network only  

The client has no direct access to the external NAT network and relies entirely on the Domain Controller for:

- IP addressing
- DNS resolution
- Internet access
- Authentication

This mirrors how workstations typically operate in enterprise environments.

---

## Tools & Technologies Used

- Oracle VirtualBox
- Windows Server 2019
- Windows 10
- Active Directory Domain Services (AD DS)
- DNS Server
- DHCP Server
- Routing and Remote Access Services (RRAS)
- PowerShell

---

## Phase 1: Virtual Environment Setup

### Step 1: Install Required Software

The following components were installed:

- Oracle VirtualBox
- Windows Server 2019 ISO
- Windows 10 ISO

---

### Step 2: Create Domain Controller VM

A new virtual machine was created with appropriate resources to simulate a production-ready domain controller. Special attention was given to configuring two network adapters to support both internal and external traffic.

---

### Step 3: Install Windows Server

Windows Server 2019 was installed on the Domain Controller VM. After installation:

- The server name was changed
- Windows updates were applied
- Network adapters were identified and labeled
- A static IP was assigned to the internal adapter

---

## Phase 2: Active Directory & Network Services Setup

### Step 4: Install Active Directory Domain Services

Using Server Manager, the Active Directory Domain Services role was installed. The server was then promoted to a Domain Controller with the creation of a new forest:

- Domain Name: mydomain.com

After promotion, the server rebooted and began functioning as the central authentication authority for the environment.

---

### Step 5: Configure NAT Routing

Routing and Remote Access Services (RRAS) were installed and configured to enable NAT routing:

- External Interface: NAT adapter
- Internal Interface: Internal Network adapter

This configuration allows internal clients to access the internet while remaining isolated from direct external exposure.

---

### Step 6: Configure DHCP

The DHCP Server role was installed and authorized in Active Directory. A scope was created for the internal network:

- Scope Range: 172.16.0.100 – 172.16.0.200
- Default Gateway: 172.16.0.1
- DNS Server: 172.16.0.1

This ensures that all internal clients automatically receive proper IP configuration without manual intervention.

---

## Phase 3: User Automation with PowerShell

### Step 7: Bulk User Creation

A PowerShell script was used to automate the creation of multiple Active Directory user accounts. This simulates real-world enterprise onboarding processes and demonstrates the efficiency of automation in IT environments.

Key actions included:

- Adjusting execution policy
- Running the script from PowerShell
- Verifying accounts in Active Directory Users and Computers

---

## Phase 4: Client Configuration

### Step 8: Create Windows 10 Client VM

A Windows 10 virtual machine was created and connected exclusively to the internal network to ensure full dependency on domain services.

---

### Step 9: Join Client to the Domain

On the Windows 10 client:

- DNS was set to 172.16.0.1
- The system was joined to mydomain.com
- The machine was rebooted
- A domain user account was used to log in

Successful login confirmed proper DNS resolution, authentication, and domain communication.

---

## Final Validation

The lab was considered fully functional after verifying:

- Client received IP configuration from DHCP
- Client successfully joined the domain
- Domain users could authenticate and log in
- Internet access worked through NAT routing
- User accounts appeared correctly in Active Directory

---

## Challenges & Troubleshooting

Several common Active Directory and networking issues were encountered and resolved during the implementation of this lab. These challenges closely mirror real-world scenarios faced in enterprise environments and provided valuable troubleshooting experience.

- Domain join failures caused by incorrect DNS configuration on the client machine
- DNS resolution issues stemming from the use of VirtualBox NAT DNS instead of the Domain Controller’s internal DNS interface
- Lack of internet access due to initial Routing and Remote Access (RRAS) misconfiguration

Each issue was resolved through methodical troubleshooting, validation, and corrective configuration changes, reinforcing the importance of DNS, routing, and network role awareness in Active Directory environments.

---

## Lessons Learned

This lab reinforced several key principles that are critical to managing and troubleshooting real-world Windows environments:

- Active Directory functionality is heavily dependent on proper DNS configuration
- Domain-joined clients must always use the Domain Controller as their primary DNS server
- Incorrect DNS sources (such as VirtualBox NAT DNS) can prevent domain discovery even when internet connectivity exists
- Network profile classification (Public vs Private/Domain) can directly impact Active Directory communication
- Many Active Directory issues can be resolved by systematically verifying DNS, IP configuration, and routing

---

## What This Lab Demonstrates

- Enterprise Windows networking fundamentals
- Active Directory administration
- DNS and DHCP infrastructure
- Network routing and NAT concepts
- Virtualization skills
- PowerShell automation
- Real-world troubleshooting techniques

---

## Conclusion

This Active Directory lab provided extensive hands-on experience building and managing a realistic enterprise Windows environment. The project demonstrates practical knowledge of user management, networking, automation, and troubleshooting—core skills required for entry-level and mid-level IT roles. The lab closely mirrors real corporate infrastructure and serves as a strong foundational project for professional IT development.

