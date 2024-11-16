---
author: Jesse Morgan
categories:
- Work
date: "2012-01-16T20:39:25Z"
excerpt: "This concept is something I've carried around with me for my last 3 jobs,
  and since I'm writing it up for my current employer, I figured I should document
  it here as well. I've mainly worked in Linux/Windows environments, so you may sense
  a bit of bias away from older systems. It's not intentional, just a result of my
  experience.\r\n\r\nPurpose\r\n\r\nThe purpose of this documentation is to provide
  a clean-cut and straight-forward convention for naming servers. The goal is to look
  towards the future and not be shackled by our past. This document should be read
  through once for a basic understanding, and used as a reference when new hosts and
  services are set up.It should provide a general convention, and may be modified
  in the future for clarity. All changes should be made by the owner of this document
  to ensure consistency."
guid: http://morgajel.net/?p=1049
id: 1049
tags:
- linkedin
title: Hostname Conventions
url: /2012/01/16/1049
---

This concept is something I’ve carried around with me for my last 3 jobs, and since I’m writing it up for my current employer, I figured I should document it here as well. I’ve mainly worked in Linux/Windows environments, so you may sense a bit of bias away from older systems. It’s not intentional, just a result of my experience. Thanks to Mick for introducing me to this schema.

<div># Purpose

The purpose of this documentation is to provide a clean-cut and straight-forward convention for naming servers. The goal is to look towards the future and not be shackled by our past. This document should be read through once for a basic understanding, and used as a reference when new hosts and services are set up.It should provide a general convention, and may be modified in the future for clarity. All changes should be made by the owner of this document to ensure consistency.

# Basic Domain Structure

The domain **example.corp** we be our designated placeholder for this discussion.

# Naming Conventions

Host names are broken into two classifications: **Physical Host Name** and **Service Host Name**.

- **Physical host names** can be compared to the DNA of a server and are by necessity somewhat cryptic. 
    - Used when managing the physical server inventory
    - Used exclusively by the operations team
    - Used for resource monitors such as IO, CPU and Memory.
- **Service host names** are more dynamic, user friendly, and may jump between physical servers as applications are migrated. 
    - Used exclusively when discussing specific functionality of the server.
    - Used for managing cluster or application functionality e.g. “pushing out a new attributes.xml file to all shopcart jboss app servers”
    - Used for functional service monitoring.

## Physical Host Name

Physical host names represent a unique way to refer to each and every physical server, however unlike the machine’s serial number, it has the flexibility to change. Each character of it’s name must have meaning or provide clarity. If the purpose of a host is changed, the host name may be changed to suit the new purpose, although this will happen infrequently (and make sure to define a procedure for changing a Physical Host Name). Names are broken into 6 key pieces of information:

**\[Location\] \[Service Level\] \[OS\] \[OS Major Version\] \[Purpose\] \[Identification Number\]**

### Location

Location refers to the datacenter in which the hardware is physically present. Each entry will consist of a unique 2 character code representing the city where the datacenter is located. The following is a definitive list of locations currently allowed. If a new entry is needed, please contact the owner of this document.

<div>| Designation | City |
|---|---|
| ax | Alexandria, VA |
| gr | Grand Rapids, MI |
| mh | Madison Hills, MI |

</div>### Service Level

Service level is loosely defined by the importance of the applications running on it, and what our perceived response should be. We currently have 3 designations:

<div>| Designation | Urgency | Purpose |
|---|---|---|
| 1 | Top Priority | This machine should be treated as a customer facing production host. |
| 2 | Medium Priority | Downtime on this server has a significant impact on productivity. |
| 3 | Low Priority | Downtime on this server has minimal impact. |

</div>Note that no server should be completely ignored if it is in distress, this just provides a general guideline as to the urgency of the problem.

### Operating System / OS Major Version

Automating system updates and configuration roll-outs is a key responsibility of the operations team. Embedding OS identification into the host name not only allows convention-based automation, but provides context for alerts during a production event. OS Identification is broken into two categories; Operating System and the Major Version number of that OS.

<div>| Designation | Operating System |
|---|---|
| C | CentOS |
| O | Oracle Solaris |
| P | Proprietary One-Off/ appliance |
| R | RedHat |
| S | Suse |
| V | Vmware ESXi |
| W | Windows |
| Z | Solaris Zone |

</div><div>| Designation | Major OS Version |
|---|---|
| 0 | 10 |
| 1 | 1 or 11 |
| 2 | 2 or 12 |
| etc. |  |

</div>### Purpose

Purpose refers to the overall usage of the server; whether it’s a database server, application server, etc. definitions are purposefully loose to allow for similar services to share the same host. This list will grow as we better define our environment. Purpose designations should refer to generic uses rather than implementation specific (db rather than Oracle or Sybase).

<div>| Designation | Purpose | Example |
|---|---|---|
| as | Application Server | Servers that run Web Applications; JBoss Application Server, Tomcat, IIS |
| ws | Web Server | Servers that host static or semi-static content; Apache, Nginx |
| db | Database | Servers that host Databases; Oracle, Sybase, MySQL |
| ci | Continuous Integration | Servers that host continuous integration; Hudson, TeamCity, CruiseControl |
| ut | Utility server | Servers used for by Operations; bind, ldap, pdsh, ssl cert creation |
| ts | Terminal Server | Terminal Server for Serial Access (not to be confused with MS Terminal Services) |
| vh | Virtual Host Server | Virtual Machine Host Server |

</div>### Numeric ID

The last segment of the physical host name is a simple three digit numeric ID. The Numeric ID can be used in several contexts:

- The numeric ID can increment when the rest of the host name is the same (mh1c6ci001, mh1c6ci002, etc).
- The numeric ID can be used to designate clustered servers (gr2c6as013,gr2c6as023, gr2c6as033, gr2c6as043).

### Samples

The follow is a list of sample host names:

- **mh2c6as011** – Madison Heights, Second Priority CentOS 6 Application Server 11 (App server hosting foo.com qa running on tomcat)
- **gr1r5db002** – Grand Rapids, Top Priority Red Hat 5 Database Server 2 (Production Oracle RAC server)
- **ax1c5ci001** – Alexandria, Top Priority CentOS 5 Continuous Integration Server 1 (Master node of Hudson)

## Service Host Name

Service Host Names are convenient alias that is associated with functionality rather than physical servers. Names are segmented into the following format:

**(application)-(purpose)(id).(subdomain).example.corp**

### Application

Application represents the specific functionality associated with a service.

<div>| Type | Example |
|---|---|
| In-House Application | register,webservices,shopcart |
| Application Server | oracle, mysql, sybase |

</div>### Purpose

The purpose designation will usually align with the purpose of the physical host name. See the reference list above for details.

### Numeric ID

Numeric ID is a two-digit incrementing ID based on the uniqueness of the rest of the hostname. e.g. register-**as01.dev**.example.corp,register-**as02.dev**.example.corp,register-as**01.qa**.example.corp

### Subdomain

Subdomain refers to either an environment, or a specialized infrastructure subdomain.

- **dev**.example.corp – Environment used for Active Development
- **qa**.example.corp – Environment used for Quality Assurance
- **stage**.example.corp – Environment used for loadtesting and User Acceptance Testing
- **prod**.example.corp – Environment used for Production
- **sn**.example.corp – Storage network used for Backups and NAS
- **mgt**.example.corp – Management network, used for ILO.

### Caveats

There are some exceptions to the Service Host Name conventions. The following is a list of examples.

<div>| Exception | Example | Reason |
|---|---|---|
| No Subdomain | nagios.example.corp | Some Infrastructure Services are not tied to a given subdomain. |
| No Purpose and ID | register.stage.example.corp | Load-balanced Service Hostnames point directly to the Load Balancer and do not require these fields |

</div>### Samples

#### Common Service Host Names

- register-as01.dev.example.corp
- shopcart-as06.prod.example.corp
- mysql-db01.qa.example.corp
- sybase-db02.stage.example.corp

#### Load Balances Service Host Names

- webservices.stage.example.corp
- register.dev.example.corp
- shopcart.qa.example.corp

#### Utility Service Host Names

- nagios.example.corp
- svn.example.corp
- hudson.example.corp

# FAQ

1. **Why are we limiting Physical Host Names to only 10 characters?**
    - Because it’s about as long as it can be and still be phonetically memorable: “mh1 c5 as 001”
    - rfc 1178 suggests keeping hostnames short.
2. **I’m not sure what to call the new host I’m building?**
    - Speak with the owner of this document if you have any doubts. Everyone on your team should be confident and competent enough to select a new name, but at the same time a centralized person will help ensure consistency.
3. **I’m building a \_\_\_\_\_\_ and it doesn’t fit into any of the purposes listed- what do I call it?**
    - Are you sure it doesn’t fit? We don’t want you arbitrarily shoe-horning servers into bad names, but we need to balance that with preventing an over-abundance of entries. If it truly doesn’t fit, work with the team to designate a new entry on this page.
4. **I’m building a \_\_\_\_\_\_ and it has multiple purposes, what do I call it?**
    - First ask yourself why it has multiple purposes; would one of those purposes be better suited elsewhere? Provided one does not have a better home elsewhere, use the one that is most appropriate; if a machine is running mysql as a backend for a JBoss application, it’s the JBoss application that people will be most interested in, so you’d label it **as** rather than **db**. It’s really a judgment call, and by all means, ask the owner of the doc if you need guidance.
5. **What about VMS/HP-UX/whatever that only allows 6 character names?**
    - You can’t win them all. At some point you have to draw a line on how much legacy you need to support. In this case, it doesn’t make sense to shackle systems made in the last 15 years because incredibly ancient machines have limitations.
6. **You’re missing \_\_\_\_ on your list.**
    - If there is a heinous omission of a location, purpose, application or operating system, by all means let us know. This isn’t set in set in stone, and can be modified if needed.

I’m always looking for feedback and new ideas; if you have any suggestions, I’d love to hear them.

</div>