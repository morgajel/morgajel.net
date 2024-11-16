---
author: Jesse Morgan
categories:
- Uncategorized
date: "2019-09-27T20:18:06Z"
guid: http://morgajel.net/?p=1304
id: 1304
title: 'Unfinished Drafts: Proposal for New Server Implementation'
url: /2019/09/27/1304
---

<div><div>***This was originally written in at some point in 2013. It was never finished, and other than some light editing, I’ve left it in the original state.***</div><div></div><div></div><div><span style="font-size: 25.6px; letter-spacing: -0.03em;">Current Situation</span></div></div>My current employer has a problem with managing scale. Bad habits and lack of consistency have led to an environment of never-ending one-offs that result in extended downtime, employee burnout, and loss of productivity. To fully grasp the scope of the current situation, We must look at the issues we currently suffer from, and the cost incurred by them.

### Two issues: Builds and …Everything Else

Builds have been a sore point for our for our team for some time. Common complaints involve:

- Reliance on a proprietary tool (HP RDP), which is windows based and owned by another team
- Reliance on DNS entries for the build process, which may take days to go through
- Lack of Tribal knowledge of the build process (only 2 team members are fully educated in it)
- Lack of visibility and documentation of the process and details
- Lack of centralized account management ownership
- Slow to resolve issues with build (no default jdk install, ulimit)
- Newly built servers are not up to date (patched)
- Aged distributions (SLES 9, SLES 10) require hardware-specific drivers on newer hardware.

Beyond our build problems, we have further issues:

- Lack of centralized, Tiered, or Channeled patching.
- Unreliable naming conventions.
- Heavy ramp-up time

While we have done our best to address some of these non-build issues, only a full revamp of the build process will address the underlying problems.

### Resulting Costs: Time and Money

The repercussions of our build issues have both obvious and indirect costs.

#### Things that Cost Time

- **Builds require DNS Changes:** RDP requires DNS entries, which require Change Request windows. This can roadblock a project for up to two days.
- **Inconsistency:** Tracking down simple production issues require intimate domain knowledge due to the sheer number of one offs.
- **Lack of Visibility:** Without domain knowledge, the steps to tracking down an issue requires extensive sleuthing to fight the right servers, pools, projects, irules, etc.
- **Lack of Auditing:** With no mechanism within the team to “circle back” and clean up after ourselves, unresolved issues sit for months, resulting in confusion later.
- **Lack of up-to-date Documentation:** Much of our documentation is woefully out of date, leading to poor decisions based on bad intel.
- **Lack of Instrumentation:** Applications consist of multiple layers, but due to firewall, code, authentication and DNS constraints, Applications cannot easily be tested at all layers.
- **High Ramp-up time for New Employees:** Time is wasted for both the new employee and trainer to learn all of the nuances.
- **Context Thrashing:** Humans aren’t nearly as good at multitasking as they think. The constant thrash of interruptions reduce efficiency.

#### Things that Cost Money

- **Licensing:** Only a small minority of our servers have valid SLES licenses, making update costs somewhat dubious. Updates via OpenSuse/CentOS are a viable option, but places us in a hybrid environment. 
    - Suse quoted around $260k to fully license and support
    - Red Hat quoted significantly more to fully license and support
- **Support:** Hardware support, software support, offshore support are not cheap.

## Suggested Solution

The suggested solution to this predicament is a ground up redesign of our environment, starting with our baseline installation and building on our recently introduced conventions. Simplification and refactoring are the targets, since they will allow for better management at scale. Whenever a design decision is made, the ops team should be involved to discuss it.

### Baseline Build: Commercial/Community Hybrid model

Two things prevent us from going with a completely community-supported build- Business Insecurity and third-party support.

- **Business Insecurity** is an internal requirement to “call someone if something breaks,” which may or may not be used (or even helpful). Finding a solution is often quicker and easier through community support via online chat, google searches and social networking.
- **Third-party support** is an external requirement where a company like Oracle will only support their product on a blessed distribution, despite the difference being in name only. As long as you are running on a licensed distribution, you are usually supported, regardless of the individual packages installed, meaning a RHEL-licensed server could pull packages from a CentOS source.

The primary differences between SLES/OpenSuse and RHEL/CentOS is the source of the packages and the trademarks. Regardless of distribution, maintaining our packages via an internal centralized source is possible, with licensing only used when “Vendor support” is required by a third party application.

RHEL/CentOS is suggested for baseline build for a number of reasons:

- **Market Penetration:** RHEL has a 60-70% market share, meaning third party support will be better and sysadmin skills will be more commonplace (hence cheaper).
- **Larger Community Support:** based on Support channels and various other sources, RHEL has the larger community.
- **Owns JBoss:** RHEL could provide support and training at discounted rates.
- **Clean Slate:** Switching distributions forces a clean-slate re-evaluation of our practices.

### Base Package Set and Base Configuration Overlay

Server installation

### Conventions over Configuration

Plainly Labeled

Consolidation

Upgrade path

### How This Reduces Costs and Man-Hours

### Concerns

## Implementation Examples to resolve outstanding issues