# Active Directory Lab

**Enterprise Windows Network in VirtualBox**

## Overview

This project documents the Active Directory home lab that I built and configured on my local machine
using Oracle VirtualBox. The environment simulates a real enterprise Windows network and includes:

* A Windows Server Domain Controller running Active Directory, DNS, DHCP, and NAT
* A Windows 10 client joined to the domain
* Centralized authentication
* Automated user creation using PowerShell
* Internal corporate-style networking

All configuration, validation, and troubleshooting were completed independently.

This lab was built to gain hands-on experience with IT infrastructure used in real corporate environments.

## Lab Objectives

The purpose of this lab was to learn and demonstrate:

* How Active Directory authenticates users and machines
* How DNS and DHCP work inside a Windows domain
* How private corporate networks are designed
* How NAT allows internal machines to access the internet
* How PowerShell is used for enterprise automation
* How domain-joined machines operate in a business environment

## Network Design

My lab uses a two-network architecture similar to real environments:

* **External Network (NAT)**
  Provides internet access for the lab.

* **Internal Network (Private LAN)**
  Used for domain authentication, DHCP, and DNS traffic.

The Domain Controller sits between both networks and acts as the gateway.

Win10 Client → Internal Network (Private LAN) → Domain Controller (AD, DNS, DHCP, NAT) → External Network (Internet)

## Virtual Machines

### Domain Controller - DC

* OS: Windows Server 2019

* RAM: 4 GB

* Storage: 50 GB

* Network Adapters:

* Adapter 1: NAT (Internet)

* Adapter 2: Internal Network (Private LAN)

### Client Machine - Client1

* OS: Windows 10

* RAM: 4 GB

* Storage: 50 GB

* Network Adapter:

* Internal Network only

## Tools & Technologies Used

* Oracle VirtualBox
* Windows Server 2019
* Windows 10
* Active Directory Domain Services
* DNS
* DHCP
* Routing and Remote Access (NAT)
* PowerShell

## Phase 1: Virtual Environment Setup

### Step 1: Install Content

I installed Oracle VirtualBox, Windows server 2019 ISO and Windows 10 ISO.

### Step 2: Create Domain Controller VM (DC)

I created a new virtual machine with the following configuration:

* Name: Domain Controller

* OS Type: Windows Server 2019

* RAM: 4 GB

* Virtual Disk: 50 GB

* Network:

* Adapter 1 → NAT

* Adapter 2 → Internal Network

### Step 3: Install Windows Server on DC

I booted DC using the Windows Server ISO and completed the installation.

After installation:

* Renamed the server to DC

* Configured a static IP on the internal adapter:

* IP: 172.16.0.1

* Subnet: 255.255.255.0

* DNS: 172.16.0.1 (self)

## Phase 2: Active Directory Setup

### Step 4: Install Active Directory Domain Services

On DC, I installed the Active Directory Domain Services role using Server Manager.

I then promoted the server to a Domain Controller and created a new forest:

* Domain name: mydomain.com

After promotion, the server rebooted as the Domain Controller.

### Step 5: Configure NAT Routing

I installed Routing and Remote Access on DC and configured NAT:

* External interface: NAT adapter

* Internal interface: Internal Network adapter

This allows internal machines to access the internet through DC.

### Step 6: Configure DHCP

I installed the DHCP Server role on DC and created a scope for the internal network:

* Scope range: 172.16.0.100 – 172.16.0.200

* Gateway: 172.16.0.1

* DNS: 172.16.0.1

This allows all internal clients to automatically receive IP configuration.

## Phase 3: User Automation with PowerShell

### Step 7: Bulk User Creation

I used a PowerShell script to automatically create multiple users in Active Directory.

Process:

* Adjusted execution policy

* Ran the script from PowerShell

* Verified users appeared in Active Directory Users and Computers

This simulates real enterprise onboarding automation.

## Phase 4: Client Machine Setup

### Step 8: Create Windows 10 Client VM (Client1)

I created a Windows 10 virtual machine with the following configuration:

* Name: Client1

* RAM: 4 GB

* Disk: 50 GB

* Network Adapter: Internal Network only

I then installed Windows 10.

### Step 9: Join Client1 to the Domain

On Client1:

* Set DNS to 172.16.0.1

* Joined the domain mydomain.com

* Rebooted the system

* Logged in using a domain account

This confirmed successful communication with Active Directory.

## Final Validation

The lab was considered successful after verifying:

* Client1 received an IP from DHCP

* Client1 joined the domain

* Domain users could log in

* Internet access worked through NAT

* Users appeared in Active Directory

## Challenges & Troubleshooting

Several common Active Directory and networking issues were encountered and resolved during the
implementation of this lab. These challenges closely mirror real-world scenarios and provide valuable
troubleshooting experience.

* Domain join failures caused by incorrect DNS configuration on the client machine
* DNS resolution issues stemming from the use of VirtualBox NAT DNS instead of the Domain Controller’s internal DNS interface

* Lack of internet access due to initial Routing and Remote Access (RRAS) misconfiguration

Each issue was resolved through methodical troubleshooting, validation, and corrective configuration
changes, reinforcing the importance of DNS, routing, and network role awareness in Active Directory
environments.

## Lessons Learned

This lab reinforced several key principles that are critical to managing and troubleshooting real-world
Windows environments:

* Active Directory functionality is heavily dependent on proper DNS configuration

* Domain-joined clients must always use the Domain Controller as their primary DNS server

* Incorrect DNS sources (such as VirtualBox NAT DNS) can prevent domain discovery even when internet connectivity exists

* Network profile classification (Public vs Private/Domain) can directly impact Active Directory communication

* Many Active Directory issues can be resolved by systematically verifying DNS, IP configuration, and routing

## What This Lab Demonstrates

* Windows networking

* Active Directory administration

* DNS & DHCP infrastructure

* Network routing and NAT

* Virtualization

* PowerShell automation

* Troubleshooting and validation

## Conclusion

This Active Directory lab provided extensive hands-on experience building and managing a realistic
enterprise Windows environment. The project demonstrates practical knowledge of user management,
networking, automation, and troubleshooting. The lab closely mirrors real corporate infrastructure and
serves as a strong foundational project for professional IT development.


