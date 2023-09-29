# Red Hat Enterprise Linux and Extended Update Support
By Paul Lucas and Bernie Hoefer.  

## Introduction

Red Hat Enterprise Linux subscriptions include an industry leading 10 year support lifecycle.  Red Hat provides additional support options so that you can choose the option that best fits your business requirements.  For example, at the end of the 10 year lifecyle you can extend the life of your Red Hat Enterprise Linux instance for an additional four years with an Extended Lifecycle Support (ELS) add-on subscription when ELS becomes availabe for RHEL 7 (see appendix below).  

During the full support lifecycle, the first 5 years of the RHEL support lifecycle, a new minor release of RHEL occurs aproximately every six months.  If you do not have a Premium support subscription or the Extended Update Support (EUS) add-on subscription, you will need to update your RHEL instance with each minor release to receive efficient support from Red Hat.  

If you have a business requirement or need to stay on a specific RHEL minor release or your application development lifecycle prevent you from updating RHEL every 6 months, EUS will support your update lifceycle requirements.

In this tutorial we will learn about Extended Update Support (EUS) for RHEL.  We will look at the  options for managing the repos assoiciated with EUS that best meet your business needs. 

Extended Update Support (EUS) is included with a RHEL 8 and 9 Premium support subscription or can be purchased as an add-on subscription to a RHEL 8 and 9 Standard support subscription. 

EUS allows you to stay on a minor release for and additonal 18 months beyond the end of support for the point release.  With EUS you would have 24 months of support for a minor release of RHEL.

Starting with RHEL 9 you have the option to purchase a new EUS add-on subscription called Enhanced Extened Update Support for both standard and premium support RHEL offerings that will extend EUS to 48 months of support for a minor relese of RHEL 9.  More information on Enhanced EUS can be found via the article links in the Appendix section below.  

Starting with RHEL 8.2 and RHEL 9.0, even numbered minor releases are eligible for EUS.

| Extended Update Support Summary |
|-------------------------|
| Applicable to even numbered minor releases starting with RHEL 8.2 and RHEL 9.0 |
| Extends minor release support to 24 months of support |
| Available with a RHEL Premium Support subscription or an EUS add-on subscription |
| Only Critical and Important impact security updates and urgent-priority bug fixes are made available at Red Hat’s discretion. |
| Limited new features or hardware enablement may be added to the EUS maintenance streams. |

EUS End Dates for current release of RHEL 8 and 9
| RHEL 8 EUS | RHEL 9 EUS |
|------------|-------------|
| 8.4 (ended May 31, 2023) | - |
| 8.6 (ends May 31, 2024) | 9.0 (ends May 31, 2024) |
| 8.8 (ends May 31, 2025) | 9.2 (ends May 31, 2025) |  
## Setup for this Tutorial
For this article we will be using two virtual machines running Red Hat Enterprise Linux 8.8 and Red Hat Enterprise Linux 9.2 to demonstrate the enablement of EUS repos on a RHEL instance.  Simple Content Access (SCA)is enabled on the Red Hat account associated with these Red Hat Enterprise Linux (RHEL) instances.

Make sure your system is registered with the Red Hat customer portal before starting the tutorial. For this tutorial I use an activation key to register my RHEL instances.  I set my activation key to consume 1 Red Hat Enterprise Linux Server with Smart Management, Premium (Physical or Virtual Nodes) subscription.  For more information on SCA or activation keys, see the appendix for additional articles.

Where it makes sense, you will see separate examples for RHEL 8 and RHEL 9.  

**RHEL 8** system registraion
```
$ sudo subscription-manager register --org=xxxxxxxx --activationkey=your_activationkey_here
[sudo] password for pslucas: 
The system has been registered with ID: 1b2d515f-e6fe-45a8-a8cf-9ffaf3839e0c
The registered system name is: eus88.example.com 
```

**RHEL 9** system registration
```
$ sudo subscription-manager register --org=xxxxxxxx --activationkey=your_activationkey_here
[sudo] password for pslucas: 
The system has been registered with ID: a9eb6fc7-a04a-408c-beff-33deb66c6820
The registered system name is: eus92.example.com
```

Let's check the system status.  We see that Content Access Mode is set Simple Content Access (SCA).  With SCA enabled we only need to enable the instance and we don't need to assign a specific subscription to the instance.
```
$ sudo subscription-manager status
+-------------------------------------------+
   System Status Details
+-------------------------------------------+
Overall Status: Disabled
Content Access Mode is set to Simple Content Access. This host has access to content, regardless of subscription status.

System Purpose Status: Disabled
```  

Let's see what repos are enabled on our systems.  
**RHEL 8** enabled repos
```
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-8-for-x86_64-appstream-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/appstream/os
Enabled:   1

Repo ID:   rhel-8-for-x86_64-baseos-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/baseos/os
Enabled:   1

```
**RHEL 9** enabled repos
```
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-9-for-x86_64-appstream-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/appstream/os
Enabled:   1

Repo ID:   rhel-9-for-x86_64-baseos-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/baseos/os
Enabled:   1
```

For RHEL 8 the rhel-8-for-x86_64-appstream-rpms and rhel-8-for-x86_64-baseos-rpms repos are from the `master branch`.  They have all the RPMs that will bring you up to the latest minor version.  In this example, RHEL 8.8 and then some for RHEL 8.

For RHEL 9 the rhel-9-for-x86_64-appstream-rpms and rhel-9-for-x86_64-baseos-rpms repos are from the `master branch`.  They have all the RPMs that will bring you up to the latest minor version.  In this example, and RHEL 9.2 and then some for RHEL 9.

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
Repo ID:   rhel-8-for-x86_64-appstream-eus-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - AppStream - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel8/$releasever/x86_64/appstream/os
Enabled:   1

Repo ID:   rhel-8-for-x86_64-baseos-eus-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel8/$releasever/x86_64/baseos/os
Enabled:   1
```  

**RHEL 9** enable EUS repos. 
```
$ sudo subscription-manager repos --disable=rhel-9-for-x86_64-appstream-rpms --disable=rhel-9-for-x86_64-baseos-rpms
Repository 'rhel-9-for-x86_64-appstream-rpms' is disabled for this system.
Repository 'rhel-9-for-x86_64-baseos-rpms' is disabled for this system.
$sudo subscription-manager repos --enable=rhel-9-for-x86_64-appstream-eus-rpms --enable=rhel-9-for-x86_64-baseos-eus-rpms
Repository 'rhel-9-for-x86_64-appstream-eus-rpms' is enabled for this system.
Repository 'rhel-9-for-x86_64-baseos-eus-rpms' is enabled for this system.
$ sudo subscription-manager repos --list-enabled
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   rhel-9-for-x86_64-baseos-eus-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - BaseOS - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel9/$releasever/x86_64/baseos/os
Enabled:   1

Repo ID:   rhel-9-for-x86_64-appstream-eus-rpms
Repo Name: Red Hat Enterprise Linux 9 for x86_64 - AppStream - Extended Update Support (RPMs)
Repo URL:  https://cdn.redhat.com/content/eus/rhel9/$releasever/x86_64/appstream/os
Enabled:   1
```  
Enabling the EUS repos will give us access to all EUS errata before and during a particular release (8.0 → 8.8), and
errata that comes out after the next minor release (8.9) during the defined minor version’s EUS period.  With out setting a specific release, we are not limiting the errata to 8.8 updates only.

If we wanted to limit the errata to a specific EUS releae we would use the release --set option with the subscription-manager.  

**RHEL 8** subscription-manager release
```
$ sudo subscription-manager release --show
Release not set
$ sudo subscription-manager release --list
+-------------------------------------------+
          Available Releases
+-------------------------------------------+
8
8.0
8.1
8.2
8.3
8.4
8.5
8.6
8.7
8.8
$ sudo subscription-manager release --set=8.8
Release set to: 8.8
```  

**RHEL 9** subscription-manager release
```
$ sudo subscription-manager release --show
Release not set
$ sudo subscription-manager release --list
+-------------------------------------------+
          Available Releases       
+-------------------------------------------+
9
9.0
9.1
9.2
$ sudo subscription-manager release --set=9.2
Release set to: 9.2
```  

Setting the release to a specifie RHEL minor release, we will limit EUS errata through the minor release for which EUS is enabled.

### Summary
Red Hat provides you with a variety support options for your Red Hat Enterprise Linux instance so you can choose the support options that best suite your particular business needs.  In this tutorial we briefly reviewed RHEL support options include Extended Update Support.  Extended Update Support provides another support option designed to best meet your specifice business requirements.


## Appendix
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)
- [Red Hat Enterprise Linux (RHEL) Extended Update Support (EUS) Overview](https://access.redhat.com/articles/rhel-eus)
- [Red Hat Enterprise Linux 9 Extended Update Support (EUS) and Enhanced Extended Update Support (Enhanced EUS) FAQ](https://access.redhat.com/articles/rhel9-eus-faq#p_enhacedeus)
- [How to Subscribe to Enhanced Extended Update Support for Red Hat Enterprise Linux on RHEL 9](https://access.redhat.com/articles/7028082)
- [How to register and subscribe a RHEL system to the Red Hat Customer Portal using Red Hat Subscription-Manager?](https://access.redhat.com/solutions/253273)
- [Enabling Simple Content Access and registering to Red Hat Insights with Subscription Manager](https://www.redhat.com/en/blog/enabling-simple-content-access-and-registering-red-hat-insights-subscription-manager)
- [Announcing up to 4 years of Extended Life Cycle Support (ELS) for Red Hat Enterprise Linux 7](https://www.redhat.com/en/blog/announcing-4-years-extended-life-cycle-support-els-red-hat-enterprise-linux-7)


## RHEL Lifecycle Table


**Red Hat Enterpise Linux Lifecycle support definitions**
| Full Support | Maintenance Support | Extended Lifecycle Support |
|--------------|---------------------|----------------------------|
| Applicable to current minor release | Applicable to last minor RHEL release 6.10, 7.9, 8.10, 9.10 | Applicable to last minor RHEL release 6.10, 7.9, 8.10, 9.10 |
| Included in current subscription | Included in current subscription | Add-on subscription |
| Red Hat defined Critical & Important security errata | Red Hat defined Critical & Important security errata| Red Hat defined Critical & Important security errata| Urgent & (at Red Hat’s discretion) High priority bug fixes |
| New & improved hardware enablement and software functionality – at Red Hat’s discretion. | New hardware enablement and software functionality are not planned | New hardware enablement and software functionality are not planned |
| New installation media | No new installation media | No new installation media | 
