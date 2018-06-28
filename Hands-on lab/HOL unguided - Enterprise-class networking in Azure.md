![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
April 2018
</div>



Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Enterprise-class networking in Azure hands-on lab unguided](#enterprise-class-networking-in-azure-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Help references](#help-references)
    - [Exercise 1: Create a Virtual Network and provision subnets](#exercise-1-create-a-virtual-network-and-provision-subnets)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 2: Create second Virtual Network and provision subnets](#exercise-2-create-second-virtual-network-and-provision-subnets)
        - [Tasks to complete](#tasks-to-complete-1)
        - [Exit criteria](#exit-criteria-1)
    - [Exercise 3: Create route tables with required routes](#exercise-3-create-route-tables-with-required-routes)
        - [Tasks to complete](#tasks-to-complete-2)
        - [Exit criteria](#exit-criteria-2)
    - [Exercise 4: Create n-tier application and validate functionality](#exercise-4-create-n-tier-application-and-validate-functionality)
        - [Tasks to complete](#tasks-to-complete-3)
        - [Exit criteria](#exit-criteria-3)
    - [Exercise 5: Build the management station](#exercise-5-build-the-management-station)
        - [Tasks to complete](#tasks-to-complete-4)
        - [Exit criteria](#exit-criteria-4)
    - [Exercise 6: Virtual Network Peering](#exercise-6-virtual-network-peering)
        - [Task to Complete](#task-to-complete)
        - [Exit criteria](#exit-criteria-5)
    - [Exercise 7: Provision and configure partner firewall solution](#exercise-7-provision-and-configure-partner-firewall-solution)
        - [Tasks to complete](#tasks-to-complete-5)
        - [Exit criteria](#exit-criteria-6)
    - [Exercise 8: Configure the firewall to control traffic flow](#exercise-8-configure-the-firewall-to-control-traffic-flow)
        - [Tasks to complete](#tasks-to-complete-6)
        - [Exit criteria](#exit-criteria-7)
    - [Exercise 9: Configure Site-to-Site connectivity](#exercise-9-configure-site-to-site-connectivity)
        - [Tasks to complete](#tasks-to-complete-7)
        - [Exit criteria](#exit-criteria-8)
    - [Exercise 10: Validate connectivity from 'on-premises' to Azure](#exercise-10-validate-connectivity-from-on-premises-to-azure)
        - [Tasks to complete](#tasks-to-complete-8)
        - [Exit criteria](#exit-criteria-9)
    - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->

# Enterprise-class networking in Azure hands-on lab unguided

## Abstract and learning objectives

In this hands-on lab, you will setup and configure a virtual network with subnets in Azure. You will also learn how to secure the virtual network by deploying a network virtual appliances and configure route tables on the subnets in your virtual network. Additionally, you will set up access to the virtual network with a jump box and a site-to-site VPN connection.

At the end of this hands-on lab, you will be better able to configure Azure networking components. 

## Overview

You have been asked by Woodgrove Financial Services to provision a proof of concept deployment that will be used by the Woodgrove team to gain familiarity with a complex Virtual Networking deployment, including all of the components that enable the solution. Specifically, the Woodgrove team will be learning about:

-   How to bypass system routing to accomplish custom routing scenarios

-   How to capitalize on load balancers to distribute load and ensure service availability

-   How to implement a partner firewall solution to control traffic flow based on policies

The result of this proof of concept will be an environment resembling this diagram:

## Solution architecture

![This image represents an entire overview of an environment for the result of this proof of concept.](images/Hands-onlabunguided-Enterprise-classnetworkinginAzureimages/media/image2.png "Solution architecture")

## Requirements

You must have a working Azure subscription to carry out this hands-on lab unguided without a spending cap to deploy the pfSense firewall from the Azure Marketplace.

## Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| IP Addressing and Subnetting for New Users   | http://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html  |
| CIDR / VLSM Supernet Calculator  | <http://www.subnet-calculator.com/cidr.php>  |
| Virtual Network documentation  | <https://azure.microsoft.com/en-us/documentation/services/virtual-network/>  |
| Network Security Group documentation  | <https://azure.microsoft.com/en-us/documentation/articles/virtual-networks-nsg/>  |
| IP addresses in Azure      |  https://azure.microsoft.com/en-us/documentation/articles/virtual-network-ip-addresses-overview-arm/ |
| User-Defined Routing and IP Forwarding   | <https://azure.microsoft.com/en-us/documentation/articles/virtual-networks-udr-overview/>  |
| Load Balancer       | <https://azure.microsoft.com/en-us/documentation/articles/load-balancer-overview/>  |
| Implementing a DMZ between Azure and your on-premises data center    |  <https://azure.microsoft.com/en-us/documentation/articles/guidance-iaas-ra-secure-vnet-hybrid/>  |



## Exercise 1: Create a Virtual Network and provision subnets

Duration: 15 minutes

In the first exercise, you will provision a Virtual Network and the subnets required to create an enterprise class network in Azure. Make sure to closely review and follow the IP Address scheme which has been documented in the Overview diagram.

### Tasks to complete

-   Provision a Virtual Network using the Address Space 10.7.0.0/16 in your Azure subscription. It should have a gateway subnet, a management subnet. This will support future tasks that will enable traffic segregation commensurate with an n-tier application deployment in an enterprise.

-   Provision a management subnet. This subnet will have a management server from which management traffic can flow to the internal subnets.

### Exit criteria

-   One Virtual Network is provisioned

-   Two subnets are configured

    -   Gateway

    -   Management

## Exercise 2: Create second Virtual Network and provision subnets

### Tasks to complete

-   Provision a second Virtual Network using the Address Space 10.8.0.0/16 in your Azure subscription. It should have a perimeter subnet, a web subnet and a data subnet. This will support future tasks that will enable traffic segregation commensurate with an n-tier application deployment in an enterprise.

### Exit criteria

-   One Virtual Network is provisioned

-   Three subnets are configured

    -   Perimeter 10.8.0.0/29

    -   Data 10.8.2.0/24

    -   Web 10.8.1.0/24

## Exercise 3: Create route tables with required routes

Duration: 15 minutes

In this exercise, you will create User Defined Routes (UDRs), for the Subnets of the VNet that was created. These UDRs will then force traffic through a Firewall device that you will deploy later in the lab.

### Tasks to complete

-   Configure route tables with appropriate routes for each subnet in the Virtual Network (with the exception of the perimeter subnet). The goal is to provide directed traffic flow to a network virtual appliance (NVA) that you will deploy in a later task. All traffic between subnets and to the Internet must be managed by the firewall. The only appliance that should be deployed in your perimeter subnet is the NVA. As such, it will take the first IP address available in the subnet. So, if your subnet address range is 10.8.0.0/29, your NVA will be allocated 10.8.0.4 (the first four addresses are reserved by Azure fabric). Use route tables and appropriate routes to direct traffic to this address.

-   **Do not associate the route tables with their corresponding subnet until directed to do so in a later task.**

### Exit criteria

-   A route table configured for each subnet (except for the perimeter subnet)

-   Each route table will have routes to the other subnets and to the Internet, all with a 'next hop' configured as the NVA

## Exercise 4: Create n-tier application and validate functionality

Duration: 60 minutes

In this exercise, you will deploy a web application using an ARM template. Once this is deployed and you have validated the installation was a success you will then provision an Internal Load Balancer.

### Tasks to complete

-   Perform a template deployment of the CloudShop Application. The template is named **CloudShop.json**, and it is found your **C:\\ECN-Hackathon** directory as a part of the Student Files download.

<!-- -->

-   After the deployment completes, you should be able to browse the website using the Public IP address WGWEB1. Next, RDP to WGWEB1, and browse to http://WGEB2 to make sure it is also configured.

    ![The Cloud shop webpage displays, with a message displaying, saying that Products are running on WGWEB1. Below that, a drop-down list of products display.](images/Hands-onlabunguided-Enterprise-classnetworkinginAzureimages/media/image24.png "Cloud shop webpage")

-   Create an Internal load balancer in the WebTier Subnet of the VNet and assign it a static IP Address of 10.7.1.10

-   RDP to WGWEB1 and browse to <http://10.7.1.10> validate the CloudShop app has been configured behind the internal load balancer and connecting to both web servers

-   After the website is validated, remove the Public IP address from WGWEB1

### Exit criteria

-   Two IIS-based web servers deployed in the Web tier subnet

-   One SQL server deployed in the Data tier subnet

-   One internal load balancer with a static IP address deployed in the Web tier subnet, and configured with:

    -   A backend pool containing both web servers

    -   An HTTP health probe (checking website availability on both web servers)

    -   A load balancing rule directing traffic to both web servers

-   A functional CloudShop web application, accessible from both web servers and from the load-balancer internal IP (this will be validated later when access is made available through the firewall)

## Exercise 5: Build the management station

Duration: 15 minutes

In this exercise, you will build a 'jump-box', which will be used to manage the Vnet once it is locked down using a Firewall.

### Tasks to complete

-   Provision a server in the management subnet. This server will be used as a 'jump box' allowing administrators to RDP to other Azure-based servers from it. The server should not have a Public IP address since RDP access will be accomplished using NATting through the firewall appliance (configured in a later task).

### Exit criteria

-   A server is provisioned in the management subnet to function as a management station

## Exercise 6: Virtual Network Peering

Duration: 15 minutes

### Task to Complete 

Configure a Virtual Network peering from both Virtual Network bidirectional.

### Exit criteria

-   VNet peering must be configured from each VNet to each other

## Exercise 7: Provision and configure partner firewall solution

Duration: 15 minutes

In this exercise, you will provision and configure an Enterprise grade firewall solution in your Azure Vnet.

### Tasks to complete

-   Provision a partner firewall solution from the Azure Marketplace into the Perimeter subnet. The **barracuda cloudgen firewall for azure** was used during the creation of this hands-on lab unguided, but other Marketplace options should work as well. For this PoC, the firewall solution does not need to be an HA pair. A firewall with a single network interface is sufficient, as the Azure fabric will handle routing.

-   The network interface assigned to the firewall must have both a Public IP address and a Private IP address. It is recommended to configure the Public IP address as static.

-   The network interface assigned to the firewall appliance must have IP Forwarding enabled.

### Exit criteria

-   A partner firewall appliance is provisioned into a perimeter subnet with a single network interface (IP forwarding enabled)

## Exercise 8: Configure the firewall to control traffic flow

Duration: 30 minutes

In this exercise, you will 'wire up' the configuration to allow access to the CloudShop application through the firewall.

### Tasks to complete

-   Configure partner firewall NATting and firewall rules to accomplish the following goals

    -   Only one IP address is exposed to the Internet. All other servers in the environment should **not** have Public IP addresses when this exercise is complete.

    -   All Azure VMs access the Internet through the firewall only

    -   Served from the internal load-balancer IP (NATted through the firewall), CloudShop website is accessible from the Internet.

    -   From the Internet, RDP access will **only** be available to the management station (NATed through the firewall). RDP access to the Azure-based servers from the management station will be enabled via the firewall.

-   Associate the previously created route tables to their corresponding subnets. This will bypass system routes and should force all traffic to flow from each subnet to the firewall as the next hop.

-   Remove any Public IP addresses from all servers with the exception of the firewall appliance

### Exit criteria

-   The CloudShop web application is accessible via HTTP using the Public IP address of the firewall only

-   Several refreshes of the CloudShop web application should eventually display both web server names at the top of the site validating the load balancer is functioning

    ![The same Cloud shop webpage displays, with WGWEB! circled in the same top message.](images/Hands-onlabunguided-Enterprise-classnetworkinginAzureimages/media/image25.png "Cloud Shop webpage")

-   RDP access from the Internet is functional to the management server **only** through the firewall

The outcome of the above exercises demonstrates leveraging a firewall in an Azure Virtual Network to manage all inter-subnet and Internet traffic. Also, a web application was made available on the Internet via the firewall.

## Exercise 9: Configure Site-to-Site connectivity

Duration: 60 minutes

In this task, we will set up another Virtual Network in a separate Azure region. This will simulate an on-premises environment. Then, we will connect the two Virtual Networks via a Site-to-Site connection.

### Tasks to complete

-   Create a new Azure Virtual Network in a separate Azure region. This VNet only needs a single subnet.

-   Create a Site-to-Site connection between the two Virtual Networks

### Exit criteria

-   One new Azure Virtual Network with a single subnet

-   Site-to-site connectivity between the VNets

## Exercise 10: Validate connectivity from 'on-premises' to Azure

Duration: 30 minutes

In this exercise, you will configure an Azure route table with appropriate routes and firewall rules to allow connectivity from your simulated on-premises environment (the new Virtual Network) to the CloudShop application. You will then set up a virtual machine in the new 'on-premises' Virtual Network to simulate on-premises connectivity to the **internal** load-balancer via the firewall.

### Tasks to complete

-   Create a route table with appropriate route rules to direct VNet to VNet traffic to the firewall appliance configured earlier

-   Create firewall rules to permit the traffic from the new Virtual Network to the Web subnet where CloudShop is deployed

-   Create a virtual machine in this new Virtual Network. It should have a Public IP address enabling direct RDP connectivity to it

### Exit criteria

-   One route table with appropriate routes associated with the subnet created in Exercise 7

-   One firewall rule allowing traffic from the 'on-premises' network to flow to the Web subnet where the CloudShop application is deployed

From the VM in your 'on-premises' VNet, validate you can browse to the **internal** load balancer IP address, through the firewall, and see the CloudShop application

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Enterprise-class networking in Azure hands-on lab unguided, you will want to delete the Resource Groups. This will free up your subscription from future charges.

