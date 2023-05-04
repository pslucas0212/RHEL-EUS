# Red Hat Enterprise Linux and Extended Update Support
By Paul Lucas and Bernie Hoefer.  

## Introduction

In this tutorial we will learn about Extended Update Support (EUS) for Red Hat Entperise Linux (RHEL).  We look at the various options for managing the repos assoiciated with EUS.  

Red Hat Enterprise Linux subscriptions include an industry leading 10 year support lifecycle.  At the end of the 10 year lifecyle you can extend the life of your Red Hat Enterprise Linux instance for an additional two years with an Extended Lifecycle Support (ELS) add-on subscription.  

During the full support lifecycle a new minor release of RHEL occurs aproximately every six months.  If you do not have a Premium support subscription or the Extended Update Support (EUS) add-on subscription, you will need to update your RHEL instance with each minor release to receive support from Red Hat.

EUS allows you to stay on a minor release for and additonal 18 months beyond the end of support for the point release.  Typically you would have 24 months of support for a minor release of RHEL.



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

Make sure your system is registered with the customer portal for this example I use an activation key to register my RHEL instances.  I set my activation key to consume 1 Red Hat Enterprise Linux Server with Smart Management, Premium (Physical or Virtual Nodes) subscription.  For more information on SCA or activation keys, see the appendix for additional articles.

Where it makes sense, you will see separate examples for RHEL 8 and RHEL 9.  

**RHEL 8**
```
$ sudo subscription-manager register --org=xxxxxxxxx --activationkey=your_key_here
[sudo] password for pslucas: 
The system has been registered with ID: 7a5b0af5-d1c7-48ae-b5b6-cad1b4a1ee13
The registered system name is: eus086.example.com
```

**RHEL 9**
```
$ sudo subscription-manager register --org=xxxxxxxxx --activationkey=your_key_here
[sudo] password for pslucas: 
The system has been registered with ID: 7a5b0af5-d1c7-48ae-b5b6-cad1b4a1ee13
The registered system name is: eus090.example.com
```

Let's check the system status.  We see that Content Access Mode is set Simple Content Access (SCA).  With SCA enabled we only need to enable the instance and we don't need to assign a specific subscription to the instance.
```
$ sudo subscription-manager status
[sudo] password for pslucas: 
+-------------------------------------------+
   System Status Details
+-------------------------------------------+
Overall Status: Disabled
Content Access Mode is set to Simple Content Access. This host has access to content, regardless of subscription status.

System Purpose Status: Disabled
```
Let's see what repos are enabled on our systems.  

**RHEL 8**
```
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-8-for-x86_64-appstream-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/appstrea
           m/os
Enabled:   1

Repo ID:   rhel-8-for-x86_64-baseos-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/baseos/o
           s
Enabled:   1

```
**RHEL 9**
```
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-9-for-x86_64-baseos-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/baseos/o
           s
Enabled:   1

Repo ID:   rhel-9-for-x86_64-appstream-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/appstrea
           m/os
Enabled:   1
```
For RHEL 8 the rhel-8-for-x86_64-appstream-rpms and rhel-8-for-x86_64-baseos-rpms repos are from the `master branch`.  They have all the RPMs that will bring you up to the latest minor version.  In this example, RHEL 8.7 and then some for RHEL 8.

For RHEL 8 the rhel-9-for-x86_64-appstream-rpms and rhel-9-for-x86_64-baseos-rpms repos are from the `master branch`.  They have all the RPMs that will bring you up to the latest minor version.  In this example, and RHEL 9.1 and then some for RHEL 9.

Let's enable the EUS repositories for RHEL 8 and 9.  We can find the repository names here [How to Access EUS](https://access.redhat.com/articles/rhel-eus#c5).  

**RHEL 8** enable EUS repos. 
```
$ sudo subscription-manager repos --disable=rhel-8-for-x86_64-appstream-rpms --disable=rhel-8-for-x86_64-baseos-rpms
Repository 'rhel-8-for-x86_64-appstream-rpms' is disabled for this system.
Repository 'rhel-8-for-x86_64-baseos-rpms' is disabled for this system.
$ sudo subscription-manager repos --enable=rhel-8-for-x86_64-appstream-eus-rpms --enable=rhel-8-for-x86_64-baseos-eus-rpms
Repository 'rhel-8-for-x86_64-appstream-eus-rpms' is enabled for this system.
Repository 'rhel-8-for-x86_64-baseos-eus-rpms' is enabled for this system.
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-8-for-x86_64-baseos-eus-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel8/$releasever/x86_64/baseos/os
Enabled:   1

Repo ID:   rhel-8-for-x86_64-appstream-eus-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - AppStream - Extended Update Support
           (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel8/$releasever/x86_64/appstream/os
Enabled:   1
```  

**RHEL 9** enable EUS repos. 
```
$ sudo subscription-manager repos --disable=rhel-9-for-x86_64-appstream-rpms --disable=rhel-9-for-x86_64-baseos-rpms
Repository 'rhel-9-for-x86_64-appstream-rpms' is disabled for this system.
Repository 'rhel-9-for-x86_64-baseos-rpms' is disabled for this system.
$ sudo subscription-manager repos --enable=rhel-9-for-x86_64-appstream-eus-rpms --enable=rhel-9-for-x86_64-baseos-eus-rpms
Repository 'rhel-9-for-x86_64-appstream-eus-rpms' is enabled for this system.
Repository 'rhel-9-for-x86_64-baseos-eus-rpms' is enabled for this system.
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-9-for-x86_64-appstream-eus-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - AppStream - Extended Update Support
           (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel9/$releasever/x86_64/appstream/os
Enabled:   1

Repo ID:   rhel-9-for-x86_64-baseos-eus-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - BaseOS - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel9/$releasever/x86_64/baseos/os
Enabled:   1
```
## Appendix
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)
- [Red Hat Enterprise Linux (RHEL) Extended Update Support (EUS) Overview](https://access.redhat.com/articles/rhel-eus)
- [How to register and subscribe a RHEL system to the Red Hat Customer Portal using Red Hat Subscription-Manager?](https://access.redhat.com/solutions/253273)
- [Enabling Simple Content Access and registering to Red Hat Insights with Subscription Manager](https://www.redhat.com/en/blog/enabling-simple-content-access-and-registering-red-hat-insights-subscription-manager)
