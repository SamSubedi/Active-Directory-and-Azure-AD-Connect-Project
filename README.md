# Active-Directory-and-Azure-AD-Connect-Project
Project Summary:

- Designed and deployed a full Active Directory (AD) environment in a simulated enterprise setting

- Automated user and group management using PowerShell and enforced policies for department-based OUs

- Configured file servers, shared drives, and Group Policies for seamless access and security

- Integrated on-premises AD with Microsoft 365 (Entra ID) using Azure AD Connect for hybrid identity management
- Implement self-service password reset (SSPR) with password writeback and password hash synchronization to keep on-premises Active Directory and Entra ID passwords in sync, allowing the same password to be used in both environments.
  

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

- Sync Server (AS-SYNC) prepared for Azure AD Connect with static IP 192.168.6.11/24

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


Troubleshooting Active Directory and Azure AD Connect Issues: During the synchronization of on-premises Active Directory (AD) with Microsoft 365 using Azure AD Connect, several issues can occur that may prevent users and groups from syncing correctly. Troubleshooting these issues requires understanding both the on-premises AD environment and the cloud configuration. Common issues include sync failures, authentication problems, password sync errors, and conflicts with user attributes. The following points summarize the most frequent problems and their resolutions:

- Sync Not Running: The Azure AD synchronization cycle may not start automatically. Resolve this by running StartADSyncCycle -PolicyType Delta in PowerShell on the sync server to manually trigger synchronization.

- Global Admin Login Fails: Signing in with a Microsoft 365 global admin account may fail, preventing sync configuration. Resolve this by verifying the global admin role in Microsoft 365 Admin Center → Users → Active Users → Roles.

- Domain Admin Credentials Fail: Azure AD Connect may not access on-premises AD if domain admin credentials are invalid. Resolve this by checking and updating credentials in Server Manager → Local Server → Domain.

- Last Sync Over 24 Hours: If the last sync has not occurred for over a day, the sync may be stalled. Resolve this by restarting the Azure AD Connect sync service in Services.msc → AD Connect Health Sync Monitor → Restart.

- Password Not Syncing: On-premises password changes may not reflect in Microsoft 365. Resolve this by enabling Password Sync in Azure AD Connect → Configure → Sync Options → Password Sync.

- Cannot Download Installer (IE Security): Internet Explorer Enhanced Security Configuration may block downloads required during setup. Resolve this by disabling IE ESC in Server Manager → Local Server → IE Enhanced Security Configuration → Off for Administrators and Users.

- Duplicate UPN / Attribute Error: Conflicting User Principal Names (UPN) or other AD attributes may prevent synchronization. Resolve this by editing the user’s UPN in Active Directory Users and Computers (ADUC) → User Properties → Account → UPN.

- Version Out of Date: Using an outdated Azure AD Connect version may cause sync failures. Resolve this by downloading the latest Azure AD Connect version from Entra Admin → Identity → Hybrid Identity Management → Connect.

Common Resolution:

- Regularly monitor synchronization health to identify and fix issues quickly.

- Manage credentials carefully for both on-premises AD and Microsoft 365.

- Adjust server security settings to avoid installation or connectivity problems.

- Keep Azure AD Connect updated to ensure smooth synchronization.

- Identify and resolve attribute conflicts like duplicate UPNs.

- Understanding hybrid identity ensures seamless access for users across on-premises and cloud services.
  

Conclusion:

- Demonstrates enterprise-ready Active Directory deployment integrated with Microsoft 365

- Covers user, group, and OU management, file server configuration, drive mapping, automation, and hybrid identity

- Validates practical IT administration skills and provides a strong foundation for real-world enterprise IT environments

This project demonstrates real-world IT administration skills, including Active Directory management, secure file sharing, network configuration, and automation. It can serve as a foundation for enterprise system and network administration and is ideal for anyone looking to showcase hands-on experience with Windows Server environments.

This project successfully synchronized the on-premises Active Directory (AD) with Microsoft 365 Entra ID using Azure AD Connect. The entire process involved preparing the AD environment, configuring the synchronization server, installing Azure AD Connect, and verifying cloud identities to ensure that all users can now sign in to Microsoft 365 using their AD credentials.

This integration improves identity management, reduces password-related issues, and provides seamless experience across cloud services such as Outlook, SharePoint, Teams, and OneDrive. By enabling hybrid identity, the organization can now benefit from centralized user management, stronger security, and streamlined onboarding for future cloud services.

The project also emphasizes troubleshooting and problem-solving in a hybrid AD environment. Common issues such as sync failures, authentication errors, password synchronization problems, and attribute conflicts were identified and resolved using practical techniques. This reinforces the importance of monitoring, maintaining, and securing enterprise systems to ensure consistent user access, operational stability, and overall reliability in a real-world IT environment.

This project allows syncing on-premises Active Directory with Entra ID using AD Connect to enable self-service password reset (SSPR). It covers password writeback, which ensures that passwords reset via SSPR in Azure AD are updated on-premises, and password hash synchronization, which keeps on-premises and cloud passwords in sync for seamless authentication across both environments. Together, these configurations enable secure and complete hybrid identity management.
