# Active-Directory-and-Azure-AD-Connect-Project
Project Summary

- Designed and deployed a full Active Directory (AD) environment in a simulated enterprise setting

- Automated user and group management using PowerShell and enforced policies for department-based OUs

- Configured file servers, shared drives, and Group Policies for seamless access and security

- Integrated on-premises AD with Microsoft 365 (Entra ID) using Azure AD Connect for hybrid identity
  

Project Overview: This project demonstrates the design, deployment, and management of a complete Active Directory infrastructure along with synchronization to Microsoft 365 using Azure AD Connect. It covers practical IT administration skills, including domain controller setup, OU and user management, Group Policy configuration, file server setup, network drive mapping, PowerShell automation, and hybrid identity integration.


Key Features:

- Domain Controller Deployment: Installed Active Directory Domain Services and configured a new forest/domain

- OU and User Management: Created parent and child OUs, added users, and organized accounts by department

- Group and Permission Management: Created security groups and assigned users to manage access to shared resources

- File Server Configuration: Configured RAID-5 volumes, shared departmental and private folders, and mapped home/shared drives

- Group Policy Automation: Applied GPOs to map network drives and enforce policies for specific OUs

- PowerShell Automation: Automated bulk user creation with predefined properties, OU placement, and group membership

- Azure AD Connect Integration: Synced on-premises AD users and groups to Microsoft 365 for hybrid identity

- Troubleshooting Scenarios: Identified and resolved common issues like login failures, drive mapping errors, and permission conflicts
  

Environment Setup:

- On-Premises AD Server (AD01) with domain laborhub.store, static IP 192.168.6.10/24

- Departmental OUs with users such as Emily Johnson and Michael Brown in Marketing

- Sync Server (AS-SYNC) prepared for Azure AD Connect with static IP 192.168.6.11

- On-Premises AD Configuration

- Installed Active Directory Domain Services and created forest/root domain

- Structured parent and child OUs by department

- Created security groups and assigned users

- Configured RAID-5 file server and shared departmental/private folders with permissions

- Applied Group Policies for drive mapping and access

- Automated bulk user creation via PowerShell with forced password changes at first login

- Azure AD Connect Synchronization

- Prepared sync server and disabled IE Enhanced Security Configuration

- Accessed Microsoft 365 Admin Center with Global Administrator credentials

- Installed Azure AD Connect using Express Settings for a single AD forest

- Linked to Microsoft 365 and on-premises AD with admin credentials

- Completed initial synchronization and verified users

- Users now access Microsoft 365 services (Outlook, Teams, OneDrive) with existing AD credentials
  

Project Outcomes:

- Fully functional on-premises AD with structured OUs, groups, file shares, and policies

- Bulk user creation and group assignments automated using PowerShell

- On-premises AD successfully synchronized with Microsoft 365, enabling hybrid identity

- Centralized management reduces password issues and simplifies onboarding

- Seamless access for users across both on-premises and cloud environments
  

Tools and Technologies:

- Windows Server (On-Premises Active Directory)

- Azure AD Connect

- Microsoft 365 (Entra ID)

- PowerShell

- Group Policy Management

- RAID-5 File Server Configuration
  

Conclusion:

- Demonstrates enterprise-ready Active Directory deployment integrated with Microsoft 365

- Covers user, group, and OU management, file server configuration, drive mapping, automation, and hybrid identity

- Validates practical IT administration skills and provides a strong foundation for real-world enterprise IT environments

This project demonstrates real-world IT administration skills, including Active Directory management, secure file sharing, network configuration, and automation. It can serve as a foundation for enterprise system and network administration and is ideal for anyone looking to showcase hands-on experience with Windows Server environments.

This project successfully synchronized the on-premises Active Directory (AD) with Microsoft 365 Entra ID using Azure AD Connect. The entire process involved preparing the AD environment, configuring the synchronization server, installing Azure AD Connect, and verifying cloud identities to ensure that all users can now sign in to Microsoft 365 using their AD credentials.

This integration improves identity management, reduces password-related issues, and provides seamless experience across cloud services such as Outlook, SharePoint, Teams, and OneDrive. By enabling hybrid identity, the organization can now benefit from centralized user management, stronger security, and streamlined onboarding for future cloud services.

