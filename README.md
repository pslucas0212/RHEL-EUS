# Red Hat Enterprise Linux and Extended Update Support
By Paul Lucas and Bernie Hoefer. 
## Introduction

Some introductory words here. Blah, blah, blah.  

**Red Hat Enterpise Linux Lifecycle**
| Full Support | Maintenance Support | Extended Lifecycle Support |
|--------------|---------------------|----------------------------|
| Applicable to current minor release | Applicable to last minor RHEL release 6.10, 7.9, 8.10, 9.10 | Applicable to last minor RHEL release 6.10, 7.9, 8.10, 9.10 |
| Included in current subscription | Included in current subscription | Add-on subscription |
| Red Hat defined Critical & Important security errata | Red Hat defined Critical & Important security errata| Red Hat defined Critical & Important security errata| Urgent & (at Red Hat’s discretion) High priority bug fixes |
| New & improved hardware enablement and software functionality – at Red Hat’s discretion. | New hardware enablement and software functionality are not planned | New hardware enablement and software functionality are not planned |
| New installation media | No new installation media | No new installation media | 


**Extended Update Support**  
Extended Update Support (EUS) is included with a RHEL Premium support subscription or can be purchased as an add-on subscription to a RHEL Standard support subscription.


During the full support lifecycle a new minor release of RHEL occurs aproximately every six months.  If you do not have a Premium support subscription or the Extended Update Support (EUS) add-on subscription, you will need to update your RHEL instance with each minor release to receive support from Red Hat.

EUS allows you to stay on a minor release for and additonal 18 months beyond the end of support for the point release.  Typically you would have 24 months of support for a minor release of RHEL.

Starting with RHEL 8, even numbered minor releases are eligible for EUS.

| Extended Update Support Summary |
|-------------------------|
| Applicable to even numbered minor releases starting with RHEL 8.2 and RHEL 9.0 |
| Extends minor release support to 24 months of support |
| Available with a RHEL Premium Support subscription or an EUS add-on subscription |
| Only Critical and Important impact security updates and urgent-priority bug fixes. At Red Hat’s discretion. |
| Limited, new features or hardware enablement may be added to the EUS maintenance streams. |




## Setup
For this article we will be using two virtual machines running Red Hat Enterprise Linux 8.6 and Red Hat Enterprise Linux 9.0.  Simple Content Access is enabled on the Red Hat account associated with these Red Hat Enterprise Linux (RHEL) instances.

Make sure your system is registered with the customer portal for this example I use an activation key to register my RHEL instances.  

**RHEL 8**
```
$ sudo subscription-manager register --org=xxxxxxxxx --activationkey=your_key_here
[sudo] password for pslucas: 
The system has been registered with ID: 7a5b0af5-d1c7-48ae-b5b6-cad1b4a1ee13
The registered system name is: eus086.example.com
```

**RHEL 9**


## Appendix
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)
- [Red Hat Enterprise Linux (RHEL) Extended Update Support (EUS) Overview](https://access.redhat.com/articles/rhel-eus)
