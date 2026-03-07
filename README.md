# 🔐 Active Directory and Azure AD Connect: Hybrid Identity Management

![Windows Server](https://img.shields.io/badge/Windows%20Server-Active%20Directory-0078D4?style=flat-square&logo=windows)
![Azure AD](https://img.shields.io/badge/Azure%20AD-Entra%20ID-0078D4?style=flat-square&logo=microsoft)
![Azure AD Connect](https://img.shields.io/badge/Azure%20AD%20Connect-Sync-50E6FF?style=flat-square&logo=microsoft)
![PowerShell](https://img.shields.io/badge/PowerShell-Automation-5391FE?style=flat-square&logo=powershell)
![Microsoft 365](https://img.shields.io/badge/Microsoft%20365-Cloud-00A4EF?style=flat-square&logo=microsoft)
![License](https://img.shields.io/badge/License-MIT-00CC44?style=flat-square)

> Bridge on-premises Active Directory with Microsoft 365 cloud services. Design, deploy, and manage a complete hybrid identity infrastructure with Azure AD Connect, enabling seamless authentication, password management, and user provisioning across on-premises and cloud environments.



## 📋 Table of Contents

- [The Challenge](#-the-challenge)
- [What This Project Covers](#-what-this-project-covers)
- [Architecture Overview](#-architecture-overview)
- [Key Features](#-key-features)
- [Project Components](#-project-components)
- [Environment Setup](#-environment-setup)
- [Core Implementation](#-core-implementation)
- [Hybrid Identity Features](#-hybrid-identity-features)
- [Troubleshooting Guide](#-troubleshooting-guide)
- [Skills Developed](#-skills-developed)
- [Deployment Steps](#-deployment-steps)
- [Key Learnings](#-key-learnings)
- [Conclusion](#-conclusion)



## 🚨 The Challenge

Organizations today operate in hybrid environments: some services and users remain on-premises in Active Directory, while others migrate to Microsoft 365 and cloud services. This creates a critical challenge:

**How do you maintain a single identity system across on-premises and cloud?**

Without proper hybrid identity management:
- Users must remember different credentials for on-premises and cloud services
- Password changes in one location don't sync to the other
- User provisioning and deprovisioning becomes a manual, error-prone process
- IT teams struggle to maintain consistency across two identity systems
- Security risks increase when accounts become misaligned

Organizations need a **unified identity platform** that:
- Synchronizes user accounts and group memberships automatically
- Keeps passwords in sync across both environments
- Enables single sign-on (SSO) for cloud applications
- Centralizes user management and provisioning
- Maintains security and compliance standards

This project demonstrates how to build that bridge using Azure AD Connect.



## 📦 What This Project Covers

This comprehensive project includes three detailed documentation guides covering all aspects of hybrid identity management:

| Component | Focus | Documentation |
|-----------|-------|-----------------|
| **Foundation** | On-Premises Active Directory Setup and Configuration | 1. Active Directory and Domain Controller Setup.pdf |
| **Synchronization** | Azure AD Connect Deployment and AD Sync Configuration | 2. AD sync with Entra ID.pdf |
| **Cloud Identity** | Self-Service Password Reset and Hybrid Features | 3. Self-Service Password Reset (SSPR).pdf |

Each guide provides step-by-step instructions, configuration details, and troubleshooting approaches.



## 🏗 Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    HYBRID IDENTITY ECOSYSTEM                    │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │           ON-PREMISES ENVIRONMENT                        │  │
│  │                                                          │  │
│  │  ┌────────────────────────────────────────────────┐     │  │
│  │  │  Active Directory (laborhub.store)             │     │  │
│  │  │  IP: 192.168.6.10/24                          │     │  │
│  │  │                                                │     │  │
│  │  │  Root Domain: laborhub.store                   │     │  │
│  │  │  OUs: Marketing, Sales, HR, IT                │     │  │
│  │  │  Users: 50+ with security groups              │     │  │
│  │  │  Groups: Department, Role-based               │     │  │
│  │  └────────────────────────────────────────────────┘     │  │
│  │                       ▲                                 │  │
│  │                       │ LDAP/Kerberos                  │  │
│  │                       ▼                                 │  │
│  │  ┌────────────────────────────────────────────────┐     │  │
│  │  │  Azure AD Connect (AS-SYNC)                   │     │  │
│  │  │  IP: 192.168.6.11/24                         │     │  │
│  │  │                                               │     │  │
│  │  │  Sync Engine: DirSync                        │     │  │
│  │  │  Mode: Express Settings                      │     │  │
│  │  │  Frequency: 30-minute cycles                │     │  │
│  │  └────────────────────────────────────────────────┘     │  │
│  └──────────────────────────────────────────────────────────┘  │
│                       ▲                                         │
│                       │                                         │
│         HTTPS with Azure AD Connect Service                     │
│                       │                                         │
│                       ▼                                         │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              MICROSOFT 365 CLOUD                         │  │
│  │                                                          │  │
│  │  ┌────────────────────────────────────────────────┐     │  │
│  │  │  Azure AD (Entra ID)                          │     │  │
│  │  │                                                │     │  │
│  │  │  Synced Users: 50+ with attributes            │     │  │
│  │  │  Cloud-only Groups: Licenses, Teams, etc.     │     │  │
│  │  │  License Assignment: E3, E5, etc.             │     │  │
│  │  └────────────────────────────────────────────────┘     │  │
│  │                       │                                 │  │
│  ├───────────────────────┼───────────────────────────────┤  │
│  │                       │                               │  │
│  │  ┌────────────────┐  ┌┴──────────────┐  ┌──────────┐ │  │
│  │  │    Outlook     │  │    Teams      │  │ OneDrive │ │  │
│  │  │                │  │               │  │          │ │  │
│  │  │  Cloud Email   │  │  Chat & Collab│  │  Cloud   │ │  │
│  │  │  Services      │  │  Platform     │  │ Storage  │ │  │
│  │  └────────────────┘  └───────────────┘  └──────────┘ │  │
│  │  ┌────────────────┐  ┌───────────────┐  ┌──────────┐ │  │
│  │  │ SharePoint     │  │  Office 365   │  │ Security │ │  │
│  │  │                │  │               │  │  Center  │ │  │
│  │  │  Intranet      │  │  Web Apps     │  │ & Threat │ │  │
│  │  │  Collaboration │  │  & Documents  │  │Monitoring│ │  │
│  │  └────────────────┘  └───────────────┘  └──────────┘ │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

HYBRID IDENTITY FLOW:

1. User in On-Premises AD (e.g., emily.johnson@laborhub.store)
                            ▼
2. Azure AD Connect Discovers Change
                            ▼
3. Sync Engine Processes User Object and Attributes
                            ▼
4. User Synced to Entra ID (emily.johnson@company.onmicrosoft.com)
                            ▼
5. User Can Sign In to Microsoft 365 with AD Password
                            ▼
6. Password Changes Flow Between Systems via SSPR
                            ▼
7. Seamless Access to Outlook, Teams, OneDrive, SharePoint
```



## ✅ Key Features

### On-Premises Active Directory (Foundation)

**Domain Controller (AD01)**
- Single forest: laborhub.store
- Static IP: 192.168.6.10/24
- ADDS installed and fully operational
- DNS services for name resolution
- DHCP for automatic IP allocation

**Organizational Unit (OU) Structure**
- Parent OUs: Marketing, Sales, HR, IT
- Child OUs for teams and roles
- Organized by department for policy application
- Delegation of administrative tasks

**User and Group Management**
- 50+ user accounts across departments
- Security groups for resource access control
- Distribution groups for email communications
- Role-based group membership
- Automated bulk user creation via PowerShell

**File Server Configuration**
- RAID-5 volumes for fault tolerance
- Shared departmental folders with permissions
- Private home directories for users
- Network drive mapping via Group Policy
- Granular NTFS permissions per department

**Group Policy Configuration**
- Drive mapping policies for seamless access
- Printer deployment to user groups
- Security policies for password requirements
- Startup/shutdown scripts for automation
- OU-level policy targeting

### Azure AD Connect Synchronization

**Sync Server (AS-SYNC)**
- Dedicated Windows Server for sync operations
- Static IP: 192.168.6.11/24
- Azure AD Connect installed with Express Settings
- Automatic sync cycle every 30 minutes
- Health monitoring and alerting

**Synchronization Features**
- Full directory synchronization: users, groups, contacts
- Attribute syncing: email, phone, department, title
- Filtered sync: syncs only configured OUs
- Password hash synchronization: keeps passwords in sync
- Directory extensions for custom attributes
- Sync rules for attribute mapping

**Cloud Identity**
- Users automatically created in Entra ID
- Attributes mapped from on-premises AD
- Cloud-only attributes added for licensing
- Automatic license assignment based on group
- User photos synchronized to cloud

### Hybrid Identity Features

**Self-Service Password Reset (SSPR)**
- Users reset passwords via Azure AD portal
- Password writeback: changes sync back to on-premises AD
- Multiple authentication methods: email, phone, security questions
- Seamless experience: same password for both environments
- Reduces help desk password reset tickets

**Password Hash Synchronization**
- On-premises passwords hashed and synced to cloud
- Users sign in to Microsoft 365 with AD credentials
- No separate cloud-only passwords to manage
- Enables cloud authentication without federation

**Single Sign-On (SSO) Capability**
- Users authenticate once for multiple services
- Seamless access to on-premises and cloud resources
- Conditional access policies based on user/device
- Multi-factor authentication (MFA) support

**User Provisioning**
- Automatic cloud account creation when user added to AD
- Automatic deprovisioning when user deleted from AD
- Maintains consistency between environments
- Reduces manual errors and improves efficiency

**License Management**
- Automatic license assignment based on group membership
- E3, E5, and other Microsoft 365 licenses
- Groups synced from on-premises for cloud-based assignment
- Simplifies license management at scale



## 📋 Project Components

### Component 1: Active Directory and Domain Controller Setup

**What You Learn:**
- Installing Active Directory Domain Services (ADDS)
- Creating forest and domain structure
- Designing OU hierarchy for organization
- Creating security and distribution groups
- Setting up file servers and shares
- Configuring RAID-5 for fault tolerance
- Mapping network drives via Group Policy
- Automating bulk user creation with PowerShell
- Implementing group policies for security

**Key Outcomes:**
- Fully functional on-premises AD environment
- 50+ test users organized by department
- Automated user provisioning capabilities
- Secure file sharing with proper permissions
- Network drive mapping that works seamlessly

### Component 2: Azure AD Connect and Synchronization

**What You Learn:**
- Preparing servers for Azure AD Connect
- Downloading and installing Azure AD Connect
- Configuring Express Settings for single forest
- Connecting to Microsoft 365 with Global Admin credentials
- Starting initial synchronization
- Monitoring sync health and troubleshooting
- Understanding attribute mapping
- Managing password hash synchronization
- Handling sync failures and conflicts
- Updating Azure AD Connect versions

**Key Outcomes:**
- Successful synchronization of on-premises AD to Entra ID
- All AD users and groups appearing in Microsoft 365
- Users able to sign in with their AD credentials
- Automatic sync cycle every 30 minutes
- Proper health monitoring and alerting

### Component 3: Self-Service Password Reset (SSPR)

**What You Learn:**
- Enabling self-service password reset in Entra ID
- Configuring password writeback
- Setting up authentication methods for users
- Deploying SSPR via Microsoft 365 portal
- Testing password reset and writeback
- Managing SSPR settings and policies
- Troubleshooting password sync issues
- Understanding hybrid password security
- Configuring MFA for SSPR
- Managing password writeback failures

**Key Outcomes:**
- Users can reset passwords in cloud and changes appear on-premises
- On-premises password changes sync to cloud automatically
- Same password works in both environments
- Reduced help desk password reset tickets
- Improved user experience and security



## 🔧 Environment Setup

### On-Premises Infrastructure

**Domain Controller (AD01)**
- Windows Server 2019 or later
- Static IP: 192.168.6.10/24
- Domain: laborhub.store
- DNS service: enabled and configured
- DHCP service: enabled with appropriate scopes

**Sync Server (AS-SYNC)**
- Windows Server 2019 or later
- Static IP: 192.168.6.11/24
- Network connectivity to Domain Controller
- Outbound HTTPS access to Azure (port 443)
- Azure AD Connect installed

**File Server**
- RAID-5 volume for data protection
- Shared folders: Marketing, Sales, HR, IT
- Home directory shares for users
- NTFS permissions properly configured

### Cloud Infrastructure

**Microsoft 365 Tenant**
- Global Administrator credentials ready
- Users licensed appropriately (E3 or E5)
- Password writeback feature available
- Azure AD Connect licensing included

**Entra ID Configuration**
- Hybrid identity scenario enabled
- Password sync options configured
- SSPR enabled for users
- Conditional access policies (optional)



## 🚀 Core Implementation

### Active Directory Setup Process

1. Install Active Directory Domain Services on DC server
2. Create forest and domain (laborhub.store)
3. Design OU structure: Marketing, Sales, HR, IT
4. Create security groups for resource access
5. Configure file server and RAID-5 volumes
6. Create departmental shares with permissions
7. Apply group policies for drive mapping
8. Bulk create users with PowerShell automation
9. Test user logons and resource access

### Azure AD Connect Deployment

1. Prepare sync server with static IP
2. Disable IE Enhanced Security Configuration
3. Download latest Azure AD Connect installer
4. Run Azure AD Connect with Express Settings
5. Connect to Microsoft 365 with global admin
6. Connect to on-premises AD with domain admin
7. Start initial synchronization
8. Monitor sync health and troubleshoot
9. Verify all users appear in Entra ID
10. Configure password hash synchronization

### Self-Service Password Reset Setup

1. Enable SSPR in Entra ID admin center
2. Configure password writeback in Azure AD Connect
3. Set up authentication methods for users
4. Test password reset flow
5. Verify password writeback to on-premises
6. Deploy SSPR to user groups
7. Train users on new self-service capability
8. Monitor SSPR usage and issues
9. Configure MFA for additional security



## 🔐 Hybrid Identity Features

### Feature 1: Password Synchronization

**On-Premises:** User changes password in AD
                          ▼
**Sync Engine:** Detects change and hashes password
                          ▼
**Cloud:** Password hash sent securely to Entra ID
                          ▼
**Result:** User can sign in to Microsoft 365 immediately with new password

### Feature 2: Self-Service Password Reset with Writeback

**Cloud:** User resets password via SSPR in Azure portal
                          ▼
**Sync Engine:** Detects cloud password change
                          ▼
**On-Premises:** Password updated in on-premises AD
                          ▼
**Result:** Same password works in both environments

### Feature 3: Automatic User Provisioning

**Event:** New user added to on-premises AD
                          ▼
**Sync:** Azure AD Connect discovers new user
                          ▼
**Cloud:** User automatically created in Entra ID
                          ▼
**Result:** User immediately has cloud identity and can access Microsoft 365

### Feature 4: Automatic User Deprovisioning

**Event:** User deleted from on-premises AD
                          ▼
**Sync:** Azure AD Connect removes user
                          ▼
**Cloud:** User disabled in Entra ID (soft delete)
                          ▼
**Result:** User loses access to all cloud services automatically



## 🛠 Troubleshooting Guide

### Common Issues and Resolutions

**Issue: Sync Not Running**
- Symptom: Last sync was hours/days ago
- Cause: Sync service stopped or schedule disabled
- Resolution: Run `Start-ADSyncCycle -PolicyType Delta` in PowerShell on sync server
- Prevention: Monitor sync health dashboard regularly

**Issue: Global Admin Login Fails**
- Symptom: Cannot authenticate to Microsoft 365 during Azure AD Connect setup
- Cause: Global admin role not assigned to account
- Resolution: Verify role in Microsoft 365 Admin Center: Users → Active Users → Roles
- Prevention: Assign Global Admin role before setup

**Issue: Domain Admin Credentials Fail**
- Symptom: Azure AD Connect cannot connect to on-premises AD
- Cause: Credentials incorrect or account permissions insufficient
- Resolution: Update credentials in Server Manager → Local Server → Domain
- Prevention: Test credentials before Azure AD Connect setup

**Issue: Password Not Syncing to Cloud**
- Symptom: Users cannot sign in to Microsoft 365 with on-premises password
- Cause: Password sync feature not enabled
- Resolution: Enable in Azure AD Connect: Configure → Sync Options → Password Sync → Enable
- Prevention: Verify password sync enabled during initial setup

**Issue: Password Writeback Not Working**
- Symptom: Cloud SSPR password resets don't appear on-premises
- Cause: Password writeback not enabled or permissions insufficient
- Resolution: Enable in Azure AD Connect: Configure → Password Writeback → Enable
- Prevention: Assign necessary on-premises permissions

**Issue: Duplicate UPN Error**
- Symptom: User not syncing, attribute conflict error in sync health
- Cause: Multiple users with same User Principal Name
- Resolution: Edit UPN in Active Directory Users and Computers (ADUC): User Properties → Account → UPN → make unique
- Prevention: Implement UPN naming standard before bulk user creation

**Issue: IE Enhanced Security Blocks Installation**
- Symptom: Cannot download Azure AD Connect installer
- Cause: Internet Explorer Enhanced Security Configuration enabled
- Resolution: Disable in Server Manager → Local Server → IE Enhanced Security Configuration → Off for Administrators and Users
- Prevention: Disable IE ESC before installation

**Issue: Attribute Mismatch Between On-Premises and Cloud**
- Symptom: Attributes not syncing correctly (email, phone, department, etc.)
- Cause: Sync rules not configured for those attributes
- Resolution: Modify sync rules in Azure AD Connect: Synchronization Service → Connector → Configure Attribute Flow
- Prevention: Review attribute mapping during initial setup

**Issue: Version Out of Date**
- Symptom: Sync failures or missing features
- Cause: Azure AD Connect version several updates behind
- Resolution: Download latest from Entra Admin → Identity → Hybrid Identity → Azure AD Connect → Download
- Prevention: Enable automatic updates or review version monthly

**Issue: Sync Failures After Azure AD Outage**
- Symptom: Sync errors appearing after Microsoft service incident
- Cause: Temporary connectivity issue or service recovery delay
- Resolution: Wait 1-2 hours and manually trigger sync with `Start-ADSyncCycle -PolicyType Delta`
- Prevention: Monitor Azure AD service health status

**Issue: Users Cannot Reset Passwords (SSPR)**
- Symptom: SSPR portal shows error or password reset fails
- Cause: User not licensed or SSPR not enabled for user
- Resolution: Enable SSPR for user group in Entra Admin → Identity → Passwords → Self-service password reset
- Prevention: Assign SSPR permissions to all cloud users



## 📚 Skills Developed

Completing this project develops expertise in:

**Active Directory Administration**
- Domain controller deployment and configuration
- OU design and hierarchy planning
- User and group management at scale
- Group Policy creation and application
- File server configuration and security
- RAID storage and fault tolerance
- Network drive mapping automation

**Hybrid Identity Management**
- Azure AD Connect installation and configuration
- Synchronization cycle management
- Attribute mapping and filtering
- Password hash synchronization
- Password writeback implementation
- Sync troubleshooting and health monitoring
- Version management and updates

**Cloud Identity (Entra ID)**
- User and group synchronization
- License management and assignment
- Self-service password reset configuration
- Conditional access policy setup
- Multi-factor authentication (MFA)
- Cloud-only group management
- Entra ID security features

**PowerShell Automation**
- Active Directory user and group creation
- Bulk provisioning scripts
- Azure AD Connect management commands
- Sync cycle triggering
- Health check automation
- Password policy enforcement
- Reporting and compliance scripts

**Microsoft 365 Administration**
- Global administrator role responsibilities
- User and license management
- Exchange Online integration
- Teams and SharePoint integration
- OneDrive provisioning
- Compliance and data governance
- Security and threat management

**IT Infrastructure and Networking**
- IP addressing and network design
- Static IP configuration
- Network connectivity troubleshooting
- Firewall rules for sync (port 443 HTTPS)
- Outbound connectivity verification
- Service monitoring and alerting
- High availability and disaster recovery

**Troubleshooting and Problem-Solving**
- Sync failure diagnosis and resolution
- Authentication and authorization issues
- Password synchronization problems
- Attribute conflict identification
- Service health monitoring
- Log analysis and interpretation
- Communication with cloud platform support



## 📋 Deployment Steps

### Phase 1: Prepare On-Premises Active Directory (Week 1)

1. Install Windows Server on domain controller
2. Configure static IP (192.168.6.10/24)
3. Set computer name and add to domain
4. Install Active Directory Domain Services
5. Create forest and domain (laborhub.store)
6. Create OU structure by department
7. Create security groups for resources
8. Configure file server and RAID-5
9. Create departmental shares
10. Apply group policies for drive mapping
11. Create test users manually (10-20)
12. Test user logons and resource access
13. Develop PowerShell bulk user creation script
14. Create remaining users (30-40 more)
15. Verify all users have proper group membership

### Phase 2: Deploy Azure AD Connect (Week 2)

1. Provision sync server (Windows Server)
2. Configure static IP (192.168.6.11/24)
3. Disable IE Enhanced Security Configuration
4. Test network connectivity to domain controller
5. Test outbound HTTPS to Azure (port 443)
6. Download latest Azure AD Connect installer
7. Run Azure AD Connect setup
8. Choose Express Settings
9. Enter Microsoft 365 global admin credentials
10. Enter domain admin credentials for on-premises
11. Start initial synchronization
12. Monitor sync health dashboard
13. Verify users appear in Entra ID admin center
14. Verify groups appear in Entra ID
15. Test user sign-in to Microsoft 365

### Phase 3: Configure Hybrid Features (Week 3)

1. Enable password hash synchronization
2. Test password changes on-premises
3. Verify passwords work in Microsoft 365
4. Configure password writeback in Azure AD Connect
5. Enable SSPR in Entra ID admin center
6. Configure SSPR authentication methods
7. Configure SSPR for user groups
8. Deploy SSPR to pilot group (10 users)
9. Test SSPR password reset flow
10. Verify writeback to on-premises
11. Train users on SSPR capability
12. Gradually roll out SSPR to all users
13. Configure MFA for SSPR (optional)
14. Monitor SSPR usage and issues
15. Document lessons learned and runbooks

### Phase 4: Optimize and Stabilize (Week 4)

1. Review sync health reports
2. Identify and resolve any sync failures
3. Test user onboarding process (add new user)
4. Test user offboarding process (remove user)
5. Verify automatic license assignment
6. Test Exchange Online integration
7. Test Teams licensing
8. Test OneDrive provisioning
9. Set up monitoring and alerting
10. Create runbooks for common tasks
11. Document troubleshooting procedures
12. Schedule regular health reviews
13. Plan update strategy for Azure AD Connect
14. Implement backup and disaster recovery
15. Final validation and sign-off



## 🧠 Key Learnings

Building this hybrid identity infrastructure provides critical insights:

**Hybrid Identity Complexity**
- Understanding dependencies between on-premises and cloud
- Managing synchronization cycles and latency
- Handling attribute mapping and conflicts
- Coordinating password changes across systems

**User Management at Scale**
- Designing OU structures for large organizations
- Automating user provisioning and deprovisioning
- Managing group membership across domains
- Bulk operations with minimal manual effort

**Cloud Security Principles**
- Zero-trust architecture concepts
- Conditional access and risk-based policies
- Multi-factor authentication implementation
- Password hash synchronization security

**Operational Excellence**
- Monitoring hybrid systems for health
- Responding to sync failures quickly
- Maintaining credential security
- Planning for updates and version management

**User Experience**
- Single password for multiple environments
- Seamless cloud service access
- Self-service capabilities reducing help desk load
- Transparent synchronization

**Disaster Recovery Considerations**
- Maintaining identity service availability
- Failover planning for sync servers
- Backup strategies for AD and Entra ID
- Recovery procedures for data loss



## 🎯 Conclusion

This project demonstrates that **modern IT infrastructure requires bridging on-premises and cloud identities seamlessly.**

By completing all three components, you gain enterprise-ready knowledge of:

- **Designing** on-premises Active Directory for modern organizations
- **Deploying** Azure AD Connect for reliable synchronization
- **Managing** hybrid identity with self-service capabilities
- **Troubleshooting** sync issues in production environments
- **Automating** user provisioning at scale with PowerShell
- **Securing** identities in hybrid and cloud scenarios

The skills developed here are directly applicable to:
- IT Administration roles in enterprises
- Active Directory and identity management positions
- Cloud infrastructure and platform engineering
- Help desk to IT professional career progression
- Small to large organization deployments

**Hybrid identity management is now essential, not optional.** Organizations cannot survive with disconnected on-premises and cloud identities. This project shows you how to build the bridge.

The same capabilities that large enterprises use to manage hundreds of thousands of users across multiple cloud services can be implemented here from first principles. Whether managing a 50-person organization or 50,000 users, these concepts and techniques scale.



## 📁 Project Structure

```
Active-Directory-and-Azure-AD-Connect-Project/
├── 1. Active Directory and Domain Controller Setup.pdf
├── 2. AD sync with Entra ID.pdf
├── 3. Self-Service Password Reset (SSPR).pdf
├── README.md
└── LICENSE
```



## 🛠 Tech Stack

- **Identity Management**: Active Directory (ADDS)
- **Cloud Identity**: Azure AD (Entra ID)
- **Synchronization**: Azure AD Connect
- **Directory Services**: LDAP, Kerberos
- **Protocols**: HTTPS (port 443)
- **Automation**: PowerShell scripting
- **Cloud Services**: Microsoft 365, Exchange Online, Teams, OneDrive, SharePoint
- **Security**: Password hash sync, SSPR, MFA, Conditional Access
- **Monitoring**: Azure AD Connect Health, Entra ID logs, Event Viewer



## 📄 License

MIT License: Free to use, modify, and deploy.



---

*A comprehensive portfolio project demonstrating hybrid identity design, deployment, and management for modern enterprise environments. Created for hands-on learning and practical cloud identity expertise.*

**Next Steps After This Project:**
- Implement Conditional Access policies
- Configure Multi-Factor Authentication (MFA) enforcement
- Explore Azure AD B2B guest access
- Learn Azure AD B2C for customer identity
- Study cloud-only identity scenarios
- Explore federation with external IDPs
- Advanced troubleshooting and optimization
