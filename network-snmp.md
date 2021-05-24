# SNMP Basics: What is SNMP & How Do I Use It?

## What the Heck is SNMP Anyway?

SNMP stands for Simple Network Management Protocol. It is a standard way of monitoring hardware and software from nearly any manufacturer, from Juniper, to Cisco, to Microsoft, Unix, and everything in between. SNMP requires only a couple of basic components to work: a management station, and an agent.

a management station is required. The management station is simply software that collects information from your network. Most management stations will poll your network for information regularly. Management stations range from the very simple to highly complex.

Simple software is usually very feature-limited, but can be freely available and easy to configure. For example, the [free SolarWinds Network Device Manager](http://www.solarwinds.com/register/registrationb.aspx?Program=991&c=70150000000P9IB&CMP=BIZ-TAD-NMS-DOWNLOADS-NDM-DL-HMPG) that was recently released.

On the other hand, complex systems can manage your entire network. They will also do things like generate reports, perform inventory, and send email or SMS text alerts when systems fail. Networkmanagementsoftware.com recently reviewed [SolarWinds Network Performance Monitor](https://www.networkmanagementsoftware.com/solarwinds-orion-npm-10-1-review), – an excellent management solution.

Second, the hardware or software that you want to monitor must have an agent running. The agent collects information, and then sends it to the monitoring station when polled. Agents can also send notification to the management station without being polled, for example if an error is detected.

Agents are usually built-in to your network hardware and software – they simply need to be enabled and configured.

## What Can It Do?

SNMP is very simple, yet powerful. It has the ability to help you manage your network by:

- Provide Read/Write abilities – for example you could use it to reset passwords remotely, or re-configure IP addresses.
- Collect information on how much bandwidth is being used.
- Collect error reports into a log, useful for troubleshooting and identifying trends.
- Email an alert when your server is low on disk space.
- Monitor your servers’ CPU and Memory use, alert when thresholds are exceeded.
- Page or send an SMS text-message when a device fails.
- Can perform active polling, i.e. Monitoring station asks devices for status every few minutes.
- Passive SNMP – devices can send alerts to a monitoring station on error conditions.

[![snmpbasics](https://cdn.networkmanagementsoftware.com/wp-content/uploads/snmpbasics.jpg)](https://cdn.networkmanagementsoftware.com/wp-content/uploads/snmpbasics.jpg)

## Versions and Security (or Lack Thereof)

Several versions of SNMP are supported, v1, v2c, and v3. Nearly all monitoring stations support all three versions. So why not simply use the newest version all the time?

Version 1 is the simplest and most basic of the versions, and there may be times where it’s required to support older hardware. But version 2c adds several enhancements to the protocol, such as support for “Informs”. Because of this v2c has become most widely used.

However, a major weakness of v1 and v2c is security. Community strings – the equivalent of passwords – are transmitted in clear text and there is no support for authentication. This creates risk that your community strings could become compromised. This is not good, especially considering the power SNMP has to change device configuration.

SNMP v3 adds a security features that overcome the weaknesses in v1 and v2c, and it should generally be used if possible – especially if you plan to transmit information across unsecured links. However, the extra security makes it much more complex to configure.

## Ports and Firewalls

SNMP uses UDP as the transport protocol. If management traffic will traverse firewalls, make sure that the following default ports are open:

- UDP 161: Used when management stations communicate with agents, e.g. Polling
- UDP 162: Used when agents send unsolicited Traps to the management station

## MIBs, OIDs and Traps, Oh My!

The basics of SNMP are simple, but terminology is one of the needlessly complicated parts of SNMP. However, it’s easily understood. In the next article we’ll help clear the confusion around things like “strings”, and “MIBs”.

Terminology is one of the needlessly complicated parts of SNMP. But don’t fear, it’s really very simple once understood. This page will help you understand basic SNMP terminology. We’ll start with the more important concepts in the next few sections, and a glossary of other terms can be found at the bottom of this page.

## OIDs and MIBs

Two crucial SNMP concepts are OIDs (Object Identifier) and MIBs (Management Information Base).

SNMP works by querying “Objects”. An object is simply something that we can gather information about on a network device. For instance, an object might be something like Interface Status. Querying Interface Status would return a variable – the interface could be Up, or Down. SNMP identifies objects like with an Object Identifier, or OID.


[![SNMP_OID_MIB_Tree](https://cdn.networkmanagementsoftware.com/wp-content/uploads/SNMP_OID_MIB_Tree.png)](https://cdn.networkmanagementsoftware.com/wp-content/uploads/SNMP_OID_MIB_Tree.png)

Most network management software has the ability to display the OID tree in some way. For example in the screenshot below, the OID tree is displayed in a folder-style list:

[![ScreenshotOIDFolderView](https://cdn.networkmanagementsoftware.com/wp-content/uploads/ScreenshotOIDFolderView.png)](https://cdn.networkmanagementsoftware.com/wp-content/uploads/ScreenshotOIDFolderView.png)

Closely related to the concept of an OID, are MIBs. A MIB is like a translator that helps a Management Station to understand SNMP responses obtained from your network devices. All SNMP devices generally support something called [MIB-2](http://www.ietf.org/rfc/rfc1213.txt), which is a standard set of objects that can be monitored. The Interface status object mentioned earlier is part of this group, as are most system objects like CPU or memory utilization.

Just to complicate things a little further, manufacturers can supply custom MIB files for their hardware. As an example, some hardware supports environmental monitoring of temperature or fan speeds. These objects aren’t found in the standard MIB-2 group, so manufacturers supply custom MIB files to add support for these features. The custom MIB files are loaded in to your NMS to tell the software which SNMP objects can be queried, and how to display the results.

Manufacturers register their custom objects under a different part of the OID tree from MIB-2. In the OID tree graphic above, you can find manufacturer’s objects in the blue section, under “Enterprise.” So, Microsoft would be 1.3.6.1.4.1.311 in the hierarchy, and Juniper is 1.3.6.1.4.1.2636.

MIB files themselves are difficult to read, they are only meant to be imported, or “compiled” by a Management Station. Below is an example of a Cisco MIB file for environmental monitoring.

[![SampleMIBfile](https://cdn.networkmanagementsoftware.com/wp-content/uploads/SampleMIBfile.png)](https://cdn.networkmanagementsoftware.com/wp-content/uploads/SampleMIBfile.png)

Cisco provides a useful resource for looking up OID values, and downloading MIB files for any of their devices. [You can access it here.](http://tools.cisco.com/Support/SNMP/do/BrowseOID.do?local=en)

## Community Strings

Community strings are another important SNMP concept. Community Strings are like passwords. They are used to allow authorized users to access the SNMP agent on a device. Unfortunately, many admins never change community strings from the defaults “Public” for read-only, and “Private” for read-write.

Community Strings can be configured as read-only(RO), or read-write(RW). As the name implies, read-only strings only allow information to be pulled from the agent. However, read-write strings are much more powerful, and can allow re-configuration of many of a device’s properties. Use extreme caution when implementing read-write strings. Earlier versions of SNMP transmit strings in clear-text, raising another security risk. See versions below.

## More Terminology

Some general SNMP terminology that you may come across:

**• Agent:** A process that monitors devices for problems and sends alerts to a monitoring station.

**• Community Strings:** Like passwords. See earlier in this article for more information.

**• Get:** SNMP sends a get request to device it monitors to retrieve specific information.

**• Get Next:** A getnext command is used by SNMP to get the next variable in a set – for instance in a table.

**• Informs:** Like Traps, but sent by the agent with a request for the management station to confirm receipt. Supported in v2 and up.

**• Management Station** (aka Network Management Station or **NMS**): The software configured to receive and collect information sent from SNMP agents.

**• MIB:** Management Information Base. See the beginning of this article for more information on MIBs.

**• MIB Browser (or MIB Walker):** A tool that can pull data from SNMP enabled devices, helping to identify which objects respond to a query.

**• Notification:** Same as a “Trap”. V2c and up use the term Notifications to refer to a Trap.

**• OID:** Object Identifier. See the beginning of this article for more information on OIDs.

**• Object:** The things SNMP gathers information about. Examples are Interface status, or CPU utilization.

**• Polling:** An NMS will poll, or ask devices for their status regularly.

**• Set:** SNMP can use a Set command to change settings on a device.

**• Trap:** SNMP sends a trap, or unsolicited message sent from an agent to a management station when some important event is detected. This is the opposite of polling.

**• Variable:** Variables are the actual status of an object – up/down, 90% CPU used, etc.

**• Version 1:** Original version of SNMP, community strings sent in plain text, very weak security.

**• Version 2c:** SNMP v2c was developed to fix some of the problems in v1. However multiple versions were developed, none truly addressing the problems with v1. V2c is the most common flavour, and has enhanced protocol handling over v1, resulting in slightly improved operations. However, security is still an issue because it uses plain-text community strings.

**• Version 3:** The newest version of SNMP, v3 supports full security and authentication. Should be used if possible, especially on untrusted networks.