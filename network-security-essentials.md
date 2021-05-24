# Network Security – 7 Essentials for Stronger NetSec & Perimeter Protection!

**Network Security** is the Practices, Policies and Security measures created and implemented to Monitor and Prevent unauthorized access in a computer network.

The term Networks can relate to objects including Local Area networks, WAN Links and DMZ Networks that all need to be protected.

Over a decade ago, when I was an IT consultant to a media and entertainment company, one of their agendas was to protect the data from being stolen.

They just cannot afford to lose the data into the public domain as it will severely affect the business relationship with their stakeholders and dent their reputation in the market.

The company’s internal policy was to embrace open source software, with a few Microsoft Windows Servers and Workstations, and we plugged in the most visible security holes with a mix of open source software and Active Directory Group Policies.

## Network Security Basics for 2021:

### 1. Keep software up-to-date

Unpatched and unsupported software in your organization is one of the biggest threats to network security.

Vulnerabilities in Microsoft and 3rd-party software that are not patched up on time just make it easier for hackers and malware to sneak into your IT environment, run exploits and compromise data, wreck business productivity and the overall security of the network.

Automating software patches, and creating a thorough patch management framework will reduce the security headache.

### 2. Centralize log collection, analysis and threat detection

Centralized collection of all the event logs, [syslogs](https://www.networkmanagementsoftware.com/what-is-syslog/) and flat files from all your network devices, servers, workstations, applications and databases and analyzing them for threats and detecting suspicious activities, is a general best practice while implementing an SIEM solution.

Continuous monitoring of all these devices and real-time alerts will enhance the security incident awareness in your network environment.

If we had thought about centralized log collections and implemented an SIEM solution, the data theft mentioned above would have been prevented to a great extent, and we would have had a complete audit trail of this suspicious event.

### 3. Enforce change management and disaster recovery

A configuration change on a core router, switch or firewall on your network can have three scenarios:

\1.   An authorized change from the network engineer successfully modifying the startup or running configs

\2.   An authorized change to a config file that made the network device unstable and caused network downtime

\3.   An unauthorized/malicious change to the config file from an insider or external bad actor compromising network security and business productivity

Apart from the aforementioned factors, a network device failure can add to the woes.

An automated Network change and configuration management (NCCM) solution addresses all of the above issues by backing up the configurations from hundreds of network devices, and restoring the last known good configuration on to the replacement router or switch, bringing it up-to-speed. Minimal downtime!

What’s more important is collecting the event logs from the NCCM system and centralizing it on the SIEM solution for real-time alerts and detailed audit trails with accurate time-stamps.

This integration is immensely helpful in continuously monitoring and detecting critical security events such as logon failures, account modifications and changes to global security settings.

![Picture1](https://cdn.networkmanagementsoftware.com/wp-content/uploads/Picture1.png)

### 4. Monitor switch port usage and track endpoint devices

User device tracking and mapping these devices to the specific switch ports improve your IT security by detecting and tracking all endpoint devices, immaterial of whether your network is BYOD-friendly or not.

It’s important to monitor and manage switch port utilizations to know important events, like for example, to which switch and switch port is a suspect device attached, what type of a device is that (laptop or mobile), the user identity logged on to that device, logon histories and other details such as IP address, MAC address and hostname.

You should also identify and blacklist rogue devices so that you can be alerted when the device hooks on to a switch.

### 5. Monitor user activity

Did you know that there were 79,790 security incidents in 2014, and 55% were from privileged account abuse? [Source: [Verizon 2015 Data Breach Investigations Report](http://higherlogicdownload.s3.amazonaws.com/GOVERNANCEPROFESSIONALS/a8892c7c-6297-4149-b9fc-378577d0b150/UploadedImages/Landing Page Documents/DBIR Executive Summary vv 4-10-15.pdf)].

And, that’s exactly why continuous monitoring of user activity is very important.

A network engineer or systems administrator must be alerted immediately if a specific server or network device is being accessed by a user on a holiday or outside of logon hours.

It could be a valid operation by the user, but also could be a suspicious activity.

Similarly, all other activities such as adding users to admin accounts, modifying privileged accounts, logon failures, etc., must trigger an alert so that the IT personnel concerned can take the right action to prevent a plausible attack or security compromise.

But, before anything, stop sharing passwords among administrators – this is only going to muddle credentials management, making it difficult to investigate when something goes wrong.

A disgruntled ex-employee might just login with that shared password, and wipe everything on a router or server.

And, you’d never know.

### 6. Monitor SQL server activity

Apart from monitoring user activity on network devices and servers, you must also be aware of the security incidents happening on database servers.

Based on the configuration of the SQL server and the SIEM solution collecting the logs and events, you should be aware of activities such as database error or exceptions, change attempts on database objects, duplicate connection attempts, alerts when an unknown users attempt to logon to the DB, cross-site scripting attempts, and so on.

Database security for business critical databases helps protect confidential information from being compromised.

### 7. Meet regulatory compliance standards

In the media and entertainment company data theft incident, we also found out that the data was transferred on to a USB hard drive – a seemingly simple, but uncontrollable activity at that time.

The situation would have been different if only we had followed some compliance regulations, and implemented an SIEM solution that alerted and ejected USB drives the moment they’re inserted into a USB port, anywhere in the organization.

There are a number of regulatory compliance standards, such as SOX, PCI DSS, DISA STIG, HIPAA, to name a few. The question you must ask yourself is, how easily you are demonstrating one or a few of these standards.

Investing in an [SIEM solution](http://www.solarwinds.com/siem-security-information-event-management-software.aspx?CMP=BIZ-Blog-NMS-7NetSecEssntls-LEM-PP-Q415) provides you with built-in rules and reports to meet most compliance standards, helping you avoid regulatory fines and preventing expensive data breaches.

Though network security is the primary responsibility of the network engineer or information security manager, the end-users in the organization must also be made aware of the implications of security with proper training and periodic dissemination of information on common attacks like phishing and opening unknown executable files.

[A powerful SIEM solution](http://www.solarwinds.com/siem-security-information-event-management-software.aspx?CMP=BIZ-Blog-NMS-7NetSecEssntls-LEM-PP-Q415), in combination with change management, device tracking and software patch processes will address most of the existing security threats. We can then look further into scaling up to address future threats that will come our way.