# Network Management – What Is it, Terminology and Components!

Visibility into the network is critical in any IT environment, whether you’re at home trying to figure out why your PS3 isn’t connecting to your media center, or you’re monitoring abnormal traffic patterns on your edge switches at your co-located data-center.

By way of initial overview of [network monitoring elements](https://www.networkmanagementsoftware.com/network-monitoring-tools/), we’re going to touch upon the various areas of the network, and some of the potential types of network management you may want or need to perform.

We’re going to start reviewing the tools that are available shortly (both free and paid), and hopefully give you some guidance as to the best tools for the task.

That said, let’s get right into it.

## Network Management Components include:

### ![Computer-icon](https://cdn.networkmanagementsoftware.com/wp-content/uploads/Computer-icon1.jpg)Endpoint Connectivity

There’s no better starting point then at your own desk. Managing endpoint connectivity is not an art, and there’s many tools you can use to manage endpoints. Depending on your environment, you may need to monitor certain nodes with simple ping-based tools, like [Servers Alive](http://www.woodstone.nu/salive/) to ensure the nodes are online, or you may want further control, and use a software package that will monitor the switches the endpoints are connected to, using something more powerful, like [Solarwinds Orion](http://www.solarwinds.com/products/orion/index.aspx), or [Ipswitch’s WhatsUp Gold](http://www.whatsupgold.com/) packages.

Whatever way you go, at minimum, you can see in real-time, or close to real-time, that your endpoint nodes are connected. If you’re in network operations, connectivity is where your job starts, and the desktop beyond that, well, that’s for the desktop guys to deal with. 🙂

### ![cisco-switches-icon](https://cdn.networkmanagementsoftware.com/wp-content/uploads/cisco-switches-icon2.jpg)Switch Management

All endpoints connect to a switch somewhere, and those floor switches may connect to aggregation switches, and finally back to core switchgear.

In order to manage your network effectively, or to diagnose upstream issues, visibility into the network is key. Without a solid network management infrastructure, managing network growth, diagnosing network issues, and providing effective monitoring become a difficult chore, or simply become impossible with the complex networks we have today.

Implementing a set of syslog servers, analyzing Netflow monitors, utilizing switch SNMP for configuration management, not to mention managing wireless configurations, should all be managed as centrally as possible, as all these elements are intertwined. In a complex network environment, without the proper management suite, how easy would it be to diagnose a switch interlink with a bad port, or a misconfigured QoS or VLAN rule?

If your idea of switch management means opening an SSH or telnet session to each of your switches to diagnose network problems, then you must like to make work for yourself… In a smaller environment, this might suffice, but what if you have a Campus Area Network serving 35,000 students and 5,000 faculty? Or, you just don’t have the time to dedicate to managing switches individually?

Upcoming tool reviews and recommendations will be up on the site soon, so be sure to watch for those solutions to these common questions.

### ![network-logging-icon](https://cdn.networkmanagementsoftware.com/wp-content/uploads/network-logging-icon2.jpg)Logging

Logging is critical to any network. If you’re not logging your switch information, then you won’t have any records to go back on, should something suspicious arise.

All switches, even the cheapest D-Link home router has the ability to log the activity against it. Depending on how critical your infrastructure is, you may want to log right down to the informational level. Otherwise, for the most of us, as long as you get the warnings and critical messages you’re OK.

We’ll cover a couple of ways of logging, including the ubiquitous syslog option. While syslog is powerful, it doesn’t help you identify issues, it just records them. Any syslog implementation you do that has more than a few devices logging to it, will need an analysis package as well, to tell you exactly what and where your problems are. This is another area that we’ll cover in depth in the future.

### ![network-connectivity-icon](https://cdn.networkmanagementsoftware.com/wp-content/uploads/network-connectivity-icon1.jpg)Server Connectivity

Hand-in-hand with endpoint connectivity is server connectivity management. While servers can have the same potential issues with network connectivity and management as a desktop, there’s also more potential issues that have to be dealt with, not to mention many other configuration possibilities.

With the proliferation of virtual machines in the enterprise today, managing VM connectivity is also important. In addition, add in multi-homed hosts, multiple subnet connectivity, remote connections, and production servers providing port-specific services (such as HTTP, or SQL connections), and you have a lot more to manage than your desktop connection.

We’ll try and cover some of the many options out there to manage and monitor these critical pieces of your infrastructure and give you some insight on how to make it easier to administering them.