![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
June 2019
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
    - [Exercise 1: Create a Virtual Network and provision subnets](#exercise-1--create-a-virtual-network-and-provision-subnets)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 2: Create second Virtual Network and provision subnets](#exercise-2--create-second-virtual-network-and-provision-subnets)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 3: Create route tables with required routes](#exercise-3--create-route-tables-with-required-routes)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 4: Deploy n-tier application and validate functionality](#exercise-4-deploy-n-tier-application-and-validate-functionality)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 5: Build the management station](#exercise-5--build-the-management-station)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 6: Virtual Network Peering](#exercise-6--virtual-network-peering)
        - [Task to Complete](#task-to-complete)
        - [Exit criteria](#exit-criteria)
   - [Exercise 7: Provision and configure Azure firewall solution](#exercise-7-provision-and-configure-azure-firewall-solution)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 8: Configure Site-to-Site connectivity](#exercise-8--configure-site-to-site-connectivity)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 9: Configure Network Security Groups and Application Security Groups](#exercise-9-configure-network-security-groups-and-application-security-groups)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 10: Configure Service Endpoints](#exercise-10-configure-service-endpoints)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [Exercise 11: Validate connectivity from 'on-premises' to Azure](#exercise-11--validate-connectivity-from-on-premises-to-azure)
        - [Tasks to complete](#tasks-to-complete)
        - [Exit criteria](#exit-criteria)
    - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->

# Enterprise-class networking in Azure hands-on lab unguided

## Abstract and learning objectives

In this hands-on lab, you will setup and configure a virtual network with subnets in Azure. You will also learn how to secure the virtual network by deploying a network virtual appliances and configure route tables on the subnets in your virtual network. Additionally, you will set up access to the virtual network with a jump box and a site-to-site VPN connection.

At the end of this hands-on lab, you will be better able to configure Azure networking components. 

## Overview

You have been asked by Woodgrove Financial Services to provision a proof of concept deployment that will be used by the Woodgrove team to gain familiarity with a complex Virtual Networking deployment, including all of the components that enable the solution. Specifically, the Woodgrove team will be learning about:

-   How to bypass system routing to accomplish custom routing scenarios.

-   How to capitalize on load balancers to distribute load and ensure service availability.

-   How to implement Azure Firewall to control hybrid and cross-virtual network traffic flow based on policies.

-   How to implement a combination of Network Security Groups (NSGs) and Application Security Groups (ASGs) to control traffic flow within virtual networks.

-   How to leverage service endpoints to provide restricted access to Azure PaaS services.

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
| What is Azure Firewall?    |  <https://docs.microsoft.com/en-us/azure/firewall/overview/>  |
| Security groups    |  <https://docs.microsoft.com/en-us/azure/virtual-network/security-overview/>  |
| Virtual Network Service Endpoints |  <https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview/>  |


## Exercise 1: Create a Virtual Network and provision subnets

Duration: 15 minutes

In the first exercise, you will provision a Virtual Network and the subnets required to create an enterprise class network in Azure. Make sure to closely review and follow the IP Address scheme which has been documented in the Overview diagram.

### Tasks to complete

-   Provision a Virtual Network using the Address Space 10.7.0.0/24 with the gateway subnet (10.7.0.0/29) in your Azure subscription. 

-   Provision a management subnet (10.7.0.8/29). This subnet will have a management server from which management traffic can flow to the internal subnets.

### Exit criteria

-   One virtual network is provisioned.

-   Two subnets are configured:

    -   Gateway

    -   Management

## Exercise 2: Create second Virtual Network and provision subnets

### Tasks to complete

-   Provision a second Virtual Network using the Address Space 10.7.1.0/24 in your Azure subscription. It should have the AzureFirewallSubnet subnet (10.7.1.0/26). 

-   Provision a subnet named AppSubnet (10.7.1.128/25). This will support future tasks that will enable traffic segregation commensurate with an n-tier application deployment in an enterprise.

### Exit criteria

-   One Virtual Network is provisioned.

-   Two subnets are configured:

    -   AppSubnet

    -   AzureFirewallSubnet

## Exercise 3: Create route tables with required routes

Duration: 15 minutes

In this exercise, you will create User Defined Routes (UDRs), for the Subnets of the VNet that was created. These UDRs will then force traffic through a Firewall device that you will deploy later in the lab.

### Tasks to complete

-   Configure route tables with appropriate routes for each subnet in the virtual networks (with the exception of the AzureFirewallSubnet subnet). The goal is to provide directed traffic flow to an Azure Firewall instance that you will deploy in a later exercise. All traffic between subnets and to the Internet must be managed by the firewall. The only service that should be deployed in the AzureFirewallSubnet subnet is the Azure Firewall. As such, it will take the first IP address available in the subnet. So, if your subnet address range is 10.7.1.0/26, the Azure Firewall will be allocated 10.7.1.4 (the first four addresses are reserved by Azure fabric). Use route tables and appropriate routes to direct traffic to this address.

### Exit criteria

-   A route table configured for each subnet (except for the perimeter subnet).

-   Each route table will have routes to the other subnets and to the Internet, all with a 'next hop' configured as the NVA.

## Exercise 4: Deploy n-tier application and validate functionality

Duration: 60 minutes

In this exercise, you will deploy a web application using an ARM template. Once this is deployed and you have validated the installation was a success you will then provision an Internal Load Balancer.

### Tasks to complete

-   Perform a template deployment of the CloudShop Application. The template is named **CloudShop.json**, and it is found your **C:\\ECN-Hackathon** directory as a part of the Student Files download.

-   After the deployment completes, you should be able to browse the website using the Public IP address WGWEB1. Next, RDP to WGWEB1, and browse to http://WGEB2 to make sure it is also configured.

    ![The Cloud shop webpage displays, with a message displaying, saying that Products are running on WGWEB1. Below that, a drop-down list of products display.](images/Hands-onlabunguided-Enterprise-classnetworkinginAzureimages/media/image24.png "Cloud shop webpage")

-   Create an Internal load balancer in the WebTier Subnet of the VNet and assign it a static IP Address of 10.7.1.254.

-   RDP to WGWEB1 and browse to <http://10.7.1.254> validate the CloudShop app has been configured behind the internal load balancer and connecting to both web servers.

-   After the website is validated, remove the Public IP address from WGWEB1.

### Exit criteria

-   Two IIS-based web servers deployed in the Web tier.

-   One SQL server deployed in the Data tier.

-   One internal load balancer with a static IP address 10.7.1.254, and configured with:

    -   A backend pool containing both web servers.

    -   An HTTP health probe (checking website availability on both web servers).

    -   A load balancing rule directing traffic to both web servers.

-   A functional CloudShop web application, accessible from both web servers and from the load-balancer internal IP (this will be validated later when access is made available through the firewall).

## Exercise 5: Build the management station

Duration: 15 minutes

In this exercise, you will build a 'jump-box', which will be used to manage the Vnet once it is locked down using a Firewall.

### Tasks to complete

-   Provision a server in the management subnet. This server will be used as a 'jump box' allowing administrators to RDP to other Azure-based servers from it.

### Exit criteria

-   A server is provisioned in the management subnet to function as a management station.

## Exercise 6: Virtual Network Peering

Duration: 15 minutes

### Task to Complete 

Configure a Virtual Network peering from both Virtual Network bidirectional.

### Exit criteria

-   VNet peering must be configured from each VNet to each other.

## Exercise 7: Provision and configure Azure firewall solution

Duration: 15 minutes

In this exercise, you will provision and configure an Azure Firewall instance in your Azure Vnet.

### Tasks to complete

-   Provision an Azure Firewall instance into the AzureFirewallSubnet subnet. 

-   Create a NAT rule collection allowing inbound HTTP traffic to 10.7.1.254 from Internet

-   Create a network rule collection allowing inbound HTTP and HTTPS traffic from any source IP address to 10.7.1.254

-   Create a network rule collection allowing inbound RDP traffic from the management subnet to 10.7.1.128/25

-   Associate rules to the correspondning subnets

-   Enable DDoS Standard for WGVNetRG2

### Exit criteria

-   An instance of Azure Firewall is provisioned into the AzureFirewallSubnet subnet with appropriately configured rules.

## Exercise 8: Configure Site-to-Site connectivity

Duration: 60 minutes

In this task, we will set up another Virtual Network in a separate Azure region. This will simulate an on-premises environment. Then, we will connect the two Virtual Networks via a Site-to-Site connection.

### Tasks to complete

-   Create a new Azure Virtual Network in a separate Azure region. This VNet only needs a single subnet.

-   Create a Site-to-Site connection between the two Virtual Networks.

### Exit criteria

-   One new Azure Virtual Network with a single subnet.

-   Site-to-site connectivity between the VNets.

## Exercise 9: Configure Network Security Groups and Application Security Groups

Duration: 20 minutes

In this exercise, you will restrict traffic between tiers of n-tier application by using network security groups and application security groups.

### Exit criteria

-   One new Azure Virtual Network with a single subnet.

-   Site-to-site connectivity between the VNets.

### Tasks to complete

-   Create application security groups for the web tier and database tier VMs

-   Configure application security groups by associating them with the appropriate NICs

-   Create network security group controlling traffic between the application security groups you configured earlier in the previous task

## Exercise 10: Configure Service Endpoints

In this exercise, you will configure service endpoints to restrict access to Azure Storage.

Duration: 20 minutes

### Tasks to complete

-   Create and configure a storage account with the service endpoint from the management subnet

-   Configure Azure Storage firewall and add a service endpoint


## Exercise 11: Validate connectivity from 'on-premises' to Azure

Duration: 30 minutes

In this exercise, you will configure an Azure route table with appropriate routes and firewall rules to allow connectivity from your simulated on-premises environment (the new Virtual Network) to the CloudShop application. You will then set up a virtual machine in the new 'on-premises' Virtual Network to simulate on-premises connectivity to the **internal** load-balancer via the firewall.

### Tasks to complete

-   Create a virtual machine to validate connectivity in the virtual network emulating the on-premises environment

-   Configure routing for simulated 'on-premises' to Azure traffic. When packets arrive from the simulated 'on-premises' Virtual Network (OnPremVNet) to the 'Azure-side' (WGVNet1), they arrive at the gateway WGVNet1Gateway. This gateway is in a gateway subnet (10.7.0.0/29). For packets to be directed to the Azure firewall, we need another route table and route to be associated with the gateway subnet on the 'Azure-side'.

### Exit criteria

-   Test your Enterprise Class Network from one region to another. Your testing can include the following scenarios:

      -  On your lab VM, open Internet Explorer and browse to the web application deployed to the WGVnet2 via the public IP address of the Azure Firewall (20.45.4.50). Note that this IP address can be also be reached by any of the Azure virtual machines provisioned in this lab.

      -  On the 'on-premises' virtual machine (OnPremVM), attempt to initiate a Remote Desktop session to any virtual machine on the AppSubnet (10.7.1.128/25). Note that this should fail since it is blocked by Azure Firewall.

      -  On the 'on-premises' virtual machine (OnPremVM), open Internet Explorer and browse to the web application deployed to the WGVnet2 via the private IP address of the Azure Load Balancer (10.7.1.254). Note that this traffic is routed (and allowed) via Azure Firewall.

      -  On the jump host virtual machine (WGMGMT1), open Internet Explorer and browse to the web application deployed to the WGVnet2 via the private IP address of the Azure Load Balancer(10.7.1.254). Note that this traffic is not routed via Azure Firewall.

      -  On the jump host virtual machine (WGMGMT1), initiate a Remote Desktop session to the WGWEB1 via its private IP address (10.7.1.132). This should be successful since it is allowed by Azure Firewall. However, an attempt to connect via Remote Desktop to the WGSQL1 via its private IP address shoudl fail since it is blocked by a network security group.

      -  On the jump host virtual machine (WGMGMT1), initiate a Remote Desktop session to the WGWEB2 via its private IP address (10.7.1.133). This should be successful since it is allowed by Azure Firewall. However, an attempt to connect via Remote Desktop to the WGSQL1 via its private IP address shoudl fail since it is blocked by a network security group.

      -  On the jump host virtual machine (WGMGMT1), initiate a Remote Desktop session to the WGSQL1 via its private IP address (10.7.1.134). This should be successful since it is allowed by Azure Firewall.

      -  Create a file share in the storage account created in exercise 10 and map a drive to it from both the jump host virtual machine (WGMGMT1) and one of the virtual machines on the WGVNet2 virtual network (WGWEB1, WGWEB2, or WGSQL1). This should be successful since it is allowed by Service Endpoints. Next, attempt mapping a drive from the 'on-premises' virtual machine (OnPremVM). This should fail since the direct route associated with Service Endpoint does not apply to this virtual machine. 

    >**Note:** You will need to create the file share from the jump host virtual machine (WGMGMT1), not from your lab virtual machine due to the Azure Storage firewall settings you configured in exercise 10.




### Exit criteria

-   One route table with appropriate routes associated with the subnet created in Exercise 7.

-   One firewall rule allowing traffic from the 'on-premises' network to flow to the Web subnet where the CloudShop application is deployed.

From the VM in your 'on-premises' VNet, validate you can browse to the **internal** load balancer IP address, through the firewall, and see the CloudShop application.

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Enterprise-class networking in Azure hands-on lab unguided, you will want to delete the Resource Groups. This will free up your subscription from future charges.

You should follow all steps provided *after* attending the Hands-on lab.

