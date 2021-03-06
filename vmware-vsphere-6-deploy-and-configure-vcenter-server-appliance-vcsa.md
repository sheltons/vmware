---
copyright:
  years: 1994, 2017
lastupdated: "2017-12-14"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Deploying and Configuring a vCenter Server Appliance (vCSA) with VMware vSphere 6  

Use the following steps to deploy a VMware vCenter Server Appliance.
{:shortdesc}

**Prerequisites**
* Windows system with a compatible browser installed
  * Internet Explorer / Firefox / Chrome
* vSphere Hosts with adequate resources to support a VMware vCenter Server Appliance
  * For more information about required system resources, see [Minimum requirements for the VMware vCenter Server 6.0 Appliance ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kb.vmware.com/s/article/2106572){: new_window}.
* vCSA ISO Image
  <!--* In the vmware section of IBM Cloud Download Services site or vmware.com: http://downloads.service.softlayer.com/vmware/VMware-VCSA-all-6.*.iso-->
* Ability to mount an ISO
* VPN Session with access to IBM Cloud Private Network

**Deploying and configuring a vCenter Server Appliance**

1. Mount vCenter Server Appliance (vCSA) ISO.
2. Open the vcsa-setup.html in a compatible browser.
3. Review and accept the installation of the VMware Client Integration plug-in.
4. Click **Allow**
5. Click **Install**
6. Review and accept terms of the license agreement and click **Next**.
7. Enter the FQDN or Private IP address and user name and password of the VMware Host that you would like to install the vCSA. Click **Next**.
8. Click **Yes** to accept the certificate warning.
9. Enter an appropriate Appliance Name and OS Password and click **Next**.
10. For the deployment type, select 'Install vCenter Server with an Embedded Platform Services Controller', then click **Next**.
11. Click 'Create an SSO domain' and click **Next**. <!-- if "create a new" is in the UI, it needs to be changed to "Create an SSO..."-->
12. Choose an Appliance Size and click **Next**.
13. Select a datastore and click **Next**.
14. Select 'Use an embedded database (PostgreSQL)' and click **Next*.
15. Set up your network configurations by entering the following information:
  1. Network
  * IP address Family
  * Network Type
  * Network Address
  * System name
  * Subnet mask
  * Network gateway
  * Network DNS Servers
      * For more information, see [What are Local DNS Resolvers](/docs/infrastructure/dns/dns-faq.html#what-are-the-local-dns-resolvers-).
  * Configure time sync
  * Click **Next**
16. Review the settings, then click **Finish**. Your vCSA instance is provisioned.

