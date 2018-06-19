![](images/HeaderPic.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
April 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners

**Contents**

<!-- TOC -->

- [Enterprise-class networking in Azure hands-on lab step-by-step](#enterprise-class-networking-in-azure-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution Architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Help References](#help-references)
    - [Exercise 1: Create a Virtual Network and provision subnets](#exercise-1--create-a-virtual-network-and-provision-subnets)
        - [Task 1: Create a Virtual Network](#task-1--create-a-virtual-network)
        - [Task 2: Configure subnets](#task-2--configure-subnets)
    - [Exercise 2: Create second Virtual Network and provision subnets](#exercise-2--create-second-virtual-network-and-provision-subnets)
        - [Task 1: Create a second Virtual Network](#task-1--create-a-second-virtual-network)
        - [Task 2: Configure Subnets](#task-2--configure-subnets)
    - [Exercise 3: Create route tables with required routes](#exercise-3--create-route-tables-with-required-routes)
        - [Task 1: Create route tables](#task-1--create-route-tables)
        - [Task 2: Add routes to each route table](#task-2--add-routes-to-each-route-table)
    - [Exercise 4: Deploy n-tier application and validate functionality](#exercise-4--deploy-n-tier-application-and-validate-functionality)
        - [Task 1: Use the Azure portal for a template deployment](#task-1--use-the-azure-portal-for-a-template-deployment)
        - [Task 2: Validate the CloudShop application is up after the deployment](#task-2--validate-the-cloudshop-application-is-up-after-the-deployment)
        - [Task 3: Create a load balancer to distribute load between the web servers](#task-3--create-a-load-balancer-to-distribute-load-between-the-web-servers)
        - [Task 4: Configure the load balancer](#task-4--configure-the-load-balancer)
    - [Exercise 5: Build the management station](#exercise-5--build-the-management-station)
        - [Task 1: Build the management VM](#task-1--build-the-management-vm)
    - [Exercise 6: Virtual Network Peering](#exercise-6--virtual-network-peering)
        - [Task 1: Configure VNet peering WGVNET1 to WGVNET2](#task-1--configure-vnet-peering-wgvnet1-to-wgvnet2)
        - [Task 2: Configure VNet peering WGVNET2 to WGVNET1](#task-2--configure-vnet-peering-wgvnet2-to-wgvnet1)
    - [Exercise 7: Provision and configure partner firewall solution](#exercise-7--provision-and-configure-partner-firewall-solution)
        - [Task 1: Provision the firewall appliance](#task-1--provision-the-firewall-appliance)
        - [Task 2: Enable IP forwarding on the firewall network interface](#task-2--enable-ip-forwarding-on-the-firewall-network-interface)
    - [Exercise 8: Configure the firewall to control traffic flow](#exercise-8--configure-the-firewall-to-control-traffic-flow)
        - [Task 1: Log on to Barracuda firewall and monitor the traffic and firewall rules](#task-1--log-on-to-barracuda-firewall-and-monitor-the-traffic-and-firewall-rules)
        - [Task 2: Configure firewall rules](#task-2--configure-firewall-rules)
        - [Task 3: Associate route tables to subnets](#task-3--associate-route-tables-to-subnets)
    - [Exercise 9: Configure Site-to-Site connectivity](#exercise-9--configure-site-to-site-connectivity)
        - [Task 1: Create OnPrem Virtual Network](#task-1--create-onprem-virtual-network)
        - [Task 2: Configure gateway subnets for on premise Virtual Network](#task-2--configure-gateway-subnets-for-on-premise-virtual-network)
        - [Task 3: Create the first gateway](#task-3--create-the-first-gateway)
        - [Task 4: Create the second gateway](#task-4--create-the-second-gateway)
        - [Task 5: Connect the gateways](#task-5--connect-the-gateways)
    - [Exercise 10: Validate connectivity from 'on-premises' to Azure](#exercise-10--validate-connectivity-from-on-premises-to-azure)
        - [Task 1: Create a virtual machine to validate connectivity](#task-1--create-a-virtual-machine-to-validate-connectivity)
        - [Task 2: Configure routing for simulated 'on-premises' to Azure traffic](#task-2--configure-routing-for-simulated-on-premises-to-azure-traffic)
    - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->
# Enterprise-class networking in Azure hands-on lab step-by-step

## Abstract and learning objectives

Students will learn how to setup and configure a Virtual Network with Subnets in Azure. Students will also learn how to secure the Virtual Network with Firewall rules and route tables. Additionally, students will set up access to the Virtual Network with a \"jump box\" and a Site-to-Site VPN connection.

Attendees will be better able to plan and design Virtual Networks in Azure with multiple subnets to filter and control network traffic. In addition,

-   Create a Virtual Network and provision subnets

-   Create route tables with required routes

-   Build a management jump box

-   Configure firewall to control traffic flow

-   Configure Site-to-Site connectivity


## Overview

You have been asked by Woodgrove Financial Services to provision a proof of concept deployment that will be used by the Woodgrove team to gain familiarity with a complex Virtual Networking deployment, including all of the components that enable the solution. Specifically, the Woodgrove team will be learning about:

-   How to bypass system routing to accomplish custom routing scenarios

-   How to capitalize on load balancers to distribute load and ensure service availability

-   How to implement a partner firewall solution to control traffic flow based on policies.

The result of this proof of concept will be an environment resembling this diagram:

## Solution Architecture

![This image represents an entire overview of an environment for the result of this proof of concept.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image2.png "Solution architecture")

## Requirements

You must have a working Azure subscription to carry out this hands-on lab step-by-step without a spending cap to deploy the Barracuda firewall from the Azure Marketplace.

## Help References

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

### Task 1: Create a Virtual Network

1.  From your **LABVM**, connect to the Azure portal, select **New**, and in the list of Marketplace categories, select **Networking** followed by selecting **Virtual Network**. See the following screenshot for more details.

    ![In the Azure Portal, On the left, New is selected. Under Azure Marketplace, Networking is selected. Under Featured, Virtual network is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image25.png "Azure Portal")

2.  On the **Create virtual network** blade, enter the following information:

    a.  Name: **WGVNet1**

    b.  Address space: **10.7.0.0/16**

    c.  Subscription: **Choose your subscription**

    d.  Resource group: Select **Create new**, and enter the name **WGVNetRG1**

    e.  Location: **Southcentral**

    f.  Subnet name: **GatewaySubnet**

    g.  Subnet address range: **10.7.0.0/29**

Upon completion, it should look like the following screenshot. Validate the information is correct, and select **Create**.

 ![](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image26.jpeg)

1.  Monitor the deployment status by selecting **Notifications** in the portal. In a minute or so, you should see a confirmation of the successful deployment. Select **Go to Resource**.

    ![This is a successful notification of a deployment.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image27.jpeg "Deployment succeeded")

### Task 2: Configure subnets

1.  Go to the WGVNetRG Group, and select **WGVNet1 Virtual Network** blade, and click **Subnets.**

    ![In the Virtual Network blade, under Settings, Subnets is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image28.png "Virtual Network blade")

2.  In the **Subnets** blade click on **+Subnet**.

    ![In the Subnets blade, the add Subnet button is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image29.png "Subnets blade")

3.  On the **Add subnet** blade, enter the following information:

    a.  Name: **Management**

    b.  Address range: **10.7.0.8/29**

    c.  Network security group: **None**

    d.  Route table: **None**

        When your dialog looks like the following screenshot, click **OK** to create the subnet.

    ![Field values in the Add Subnet blade are set to the previously defined settings.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image30.png "Add Subnet blade")

## Exercise 2: Create second Virtual Network and provision subnets

Duration: 15 minutes

### Task 1: Create a second Virtual Network

1.  On the **Create virtual network** blade referenced above in Exercise 1 Task 1, enter the following information:

    a.  Name: **WGVNet2**

    b.  Address space: **10.8.0.0/16**

    c.  Subscription: **Choose your subscription**

    d.  Resource group: Select **Create new**, and enter the name **WGVNetRG2**

    e.  Location: **south central**

    f.  Subnet name: **perimeter**

    g.  Subnet address range: **10.8.0.0/29**

    Upon completion, it should look like the following screenshot. Validate the information is correct, and click **Create**.

    ![This represents the properly filled out fields when creating a second Virtual Network.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image31.jpeg "Create virtual network")

### Task 2: Configure Subnets

1.  Go to the WGVNetRG2 Resource Group, and select **WGVNet2** blade, and click **Subnets**.

    ![This is a subnet configuration.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image32.jpeg "Subnets button")

2.  In the **Subnets** blade click on **+Subnet**.

    ![In the Subnets blade, the add Subnet button is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image29.png "Subnets blade")

3.  On the **Add subnet** blade, enter the following information:

    a.  Name: **DataTier**

    b.  Address range: **10.8.2.0/24**

    c.  Network security group: **None**

    d.  Route table: **None**

4.  Repeat steps 3 to create the **WebTier** subnet.

    e.  Name: **WebTier**

    f.  Address range: **10.8.1.0/24**

    g.  Network security group: **None**

    h.  Route table: **None**

    When your dialog looks like the following screenshot, click **OK** to create the subnet.

    The result should look like the following screenshot:

    ![This is the confirmation to a successful creation of a Virtual Network.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image33.jpeg "Subnets blade")

## Exercise 3: Create route tables with required routes

Duration: 15 minutes

Route Tables are containers for User Defined Routes (UDRs). The route table is created and associated with a subnet. UDRs allow you to direct traffic in ways other than normal system routes would. In this case, UDRs will direct traffic from 'internal' subnets to the firewall appliance.

### Task 1: Create route tables

1.  On the main portal menu to the left, click **More services** located at the bottom of the menu. Type **route** into the search box, and click on **Route tables**.

    [In the left pane of the Azure portal, More services is selected. Route is typed in the search box, and Route tables is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image34.png "Azure Portal")

2.  On the **Route tables** blade, click **Add**.

    ![The Add button is selected on the Route tables blade top menu bar.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image35.png "Route tables blade")

3.  On the **Route table** blade enter the following information:

    a.  Name: **MgmtRT**

    b.  Subscription: **Choose your subscription**

    c.  Resource group: Select **Use existing**, click the drop-down menu, and select **WGVNetRG1**

    d.  Location: **South Central**

        When the dialog looks like the following screenshot, click Create.

    ![This represents completed fields when initially creating a route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image36.jpeg "Create route table")

4.  After a few seconds, if the new route table does not show in the portal, click **Refresh**.

    ![The Refresh button is selected on the Route tables blade top menu bar.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image37.png "Route tables blade ")

5.  After you see the route table you created, complete steps 2 and 3 again to create the **DataRT** route table:

    e.  Name: **DataRT**

    f.  Subscription: **Choose your subscription**

    g.  Resource group: Select **Use existing**, click the drop-down menu, and select **WGVNetRG2**

    h.  Location: **south central**

6.  After you see the **DataRT** route table created (you may need to click **Refresh** again), complete steps 2 and 3 again to create the **WebRT** route table:

    i.  Name: **WebRT**

    j.  Subscription: **Choose your subscription**

    k.  Resource group: Select **Use existing**, click the drop-down menu, and select **WGVNetRG2**

    l.  Location: south central

7.  Once route tables are created, your **Route tables** blade should look like the following screenshot:

    ![Route Table listing.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image38.jpeg "Route table link")

### Task 2: Add routes to each route table

1.  Click on the **DataRT** route table, and click **Routes**.

    ![On the Route table blade, under Settings, Routes is selected. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image39.png "Route table blade ")

2.  On the **Routes** blade, click the +**Add** button. Enter the following information, and click **OK**:

    a.  Route name: **DataToInternet**

    b.  Address prefix: **0.0.0.0/0**

    c.  Next hop type: **Virtual appliance**

    d.  Next hop address: **10.8.0.4**

    ![Continued filled in entries while creating a Route Table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image40.jpeg "Add route configuration")

3.  Repeat this procedure to add the **DataToMgmt** route using the following information:

    e.  Route name:

    f.  Address prefix: **10.7.0.8/29**

    g.  Next hop type: **Virtual appliance**

    h.  Next hop address: **10.8.0.4**

    ![The DataToMgmt route is being added.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image41.jpeg "Edit route")

4.  Repeat this procedure to add the **DataToWeb** route using the following information:

    i.  Route name: **DataToWeb**

    j.  Address prefix: **10.8.1.0/24**

    k.  Next hop type: **Virtual appliance**

    l.  Next hop address: **10.8.0.4**

    ![This is a repetition to the earlier steps in creating a route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image42.jpeg "Edit route")

        Upon completion, your routes in the **DataRT** route table should look like the following screenshot:

    ![DataRT route table is seen here if successful.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image43.jpeg "Route table ")

5.  Using the breadcrumb menu at the top of the portal, click on **Route tables** to go back to that blade.

    ![Route tables is selected on the Azure Portal top menu.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image44.png "Azure Portal menu")

6.  Click on **WebRT** followed by **Routes**.

    ![Under Name, WebRT is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image45.png "WebRT")

7.  On the **Routes** blade, click the +**Add** button. Enter the following information, and click **OK**:

    m.  Route name: **WebToInet**

    n.  Address prefix: **0.0.0.0/0**

    o.  Next hop type: **Virtual appliance**

    p.  Next hop address: **10.8.0.4**

    ![Route table is being added.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image46.jpeg "Add route ")

8.  Repeat this procedure to add the **WebToData** route using the following information:

    q.  Route name: **WebToData**

    r.  Address prefix: **10.8.2.0/24**

    s.  Next hop type: **Virtual appliance**

    t.  Next hop address: **10.8.0.4**

    ![C](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image47.jpeg)

9.  Repeat this procedure add the **WebToMgmt** route using the following information:

    u.  Route name: **WebToMgmt**

    v.  Address prefix: **10.7.0.8/29**

    w.  Next hop type: **Virtual appliance**

    x.  Next hop address: **10.8.0.4**

    ![This is in repetition to earlier steps.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image48.jpeg "Edit route")

    Upon completion, your routes in the **WebRT** route table should look like the following screenshot:

    ![Successful completion.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image49.jpeg "Route table")

10. Using the breadcrumb menu at the top of the portal, click on **Route tables** to go back to that blade.

    ![Route tables is selected on the Azure Portal top menu.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image44.png "Azure Portal menu")

11. Click on **MgmtRT**, and click **Routes**.

    ![Under Name, MgmtRT is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image50.png "MgmtRT")

12. On the **Routes** blade, click the +**Add** button. Enter the following information, and click **OK**:

    y.  Route name: **MgmtToInternet**

    z.  Address prefix: **0.0.0.0/0**

    a.  Next hop type: **Internet**

    b.  Next hop address: leave it blank

    ![Continuation of steps to adding another route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image51.jpeg "Add route")

13. Complete step 12 to add the **MgmtToData** route using the following information:

    c.  Route name: **MgmtToData**

    d.  Address prefix: **10.8.2.0/24**

    e.  Next hop type: **Virtual appliance**

    f.  Next hop address: **10.8.0.4**

    ![Continuation of steps to adding another route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image52.jpeg "Add route")

14. Complete step 12 to add the **MgmtToWeb** route using the following information:

    g.  Route name: **MgmtToWeb**

    h.  Address prefix: **10.8.1.0/24**

    i.  Next hop type: **Virtual appliance**

    j.  Next hop address: **10.8.0.4**

    ![Continuation of steps to adding another route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image53.png "Add route")

    Upon completion, your routes in the **MgmtRT** route table should look like the following screenshot:

    ![Continuation of steps to adding another route table.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image54.jpeg "Route table")

**Note:** The route tables and routes you have just created are not associated with any subnets yet, so they are not impacting any traffic flow yet. This will be accomplished later in the lab.

## Exercise 4: Deploy n-tier application and validate functionality

Duration: 60 minutes

In this task, you will provision the CloudShop application using an ARM template deployment. This application has a web tier and a data tier.

### Task 1: Use the Azure portal for a template deployment

NOTE: If you have not downloaded the student files see this section in the before getting started section of this hands-on lab.

1.  On your LABVM, open the **C:\\ECN-Hackathon** which contains the student files for this lab.

2.  Sign into the Azure portal at <http://portal.azure.com>.

3.  Click **New**, and search for template deployment.

    ![In the search field of the Azure Portal, template deployment displays. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image55.png "Azure Portal")

4.  Select the template deployment link, and click Create.

    ![In the Everything blade, under Name, Template deployment is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image56.png "Everything blade")

5.  On the Custom deployment blade, click **Build your own template in the editor**

    ![In the Custom deployment blade, the link to Build your own template in the editor is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image57.png "Custom deployment blade")

6.  Click **Load file** and select the **CloudShop.json** file from your **C:\\ECN-Hackathon** directory and then click **Save**.

    ![In the Edit template blade, Load file is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image58.png "Edit template blade")

7.  Update the following parameters to reference the **WGNet2** virtual network in the **WGBNetRG2** resource group.

    ![A screenshot that shows the Existing Virtual Network Name and Existing Virtual Network Resource group parameters.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image59.png "Template parameters")

8.  Update the **Custom deployment** blade using the following inputs, agree to the terms, and click **Purchase**. This deployment will take approximately 30-40 minutes.

    a.  Resource Group: Create new / WGVMRGTMT

    b.  Location: South Central

    ![Deployment purchase in updating the Custom deployment blade.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image60.jpeg "Template blade")

### Task 2: Validate the CloudShop application is up after the deployment

1.  Using the Azure portal, open the **WGVMRG** Resource group and review the deployment.

2.  Open the **WGWEB1** blade in the Azure portal, and click **Connect**.

    ![CloudShop application connection test](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image61.jpeg "Virtual machine blade")

3.  Depending on your Remote Desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

4.  Log in with the credentials specified during creation:

    a.  User: **demouser **

    b.  Password: **demo\@pass123**

5.  You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click **Yes** to continue with the connection.

    ![The Remote Desktop Connection dialog box displays, warning you that the remote computer\'s identity cannot be verified, and asking you if you want to connect anyway. The Don\'t ask me again for connections to this computer is selected, as is the Yes button at the bottom.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image62.png "Remote Desktop Connection warning dialog box")

6.  When logging on for the first time, you will see a prompt on the right asking about network discovery. Click **No**.

    ![The Networks prompt asks if you want to find other computers, devices and content automatically. The No button is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image15.png "Networks prompt")

7.  Notice that Server Manager opens by default. On the left, click **Local Server**.

    ![Local Server is selected on the Server Manager menu.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image63.png "Server Manager menu")

8.  On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![IE Enhanced Security Configuration is set to On, and is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image64.png "Under Essentials")

9.  Change to **Off** for Administrators, and click **OK**.

    ![In the Internet Explorer Enhanced Security Configuration dialog box, the Administrators Off radio button is selected, and the OK button is selected as well.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image18.png "Internet Explorer Enhanced Security Configuration")

10. You will now ensure the CloudShop application is up and running. Open Internet explorer, and browse to both the WGWEB1 and WGWEB2 servers.

http://wgweb1

http://wgweb2

### Task 3: Create a load balancer to distribute load between the web servers

1.  In the Azure portal, click **New** in the upper left-hand corner, then Networking, Load Balancer.

    ![In the Azure Portal, New is selected. Under Azure Marketplace, Networking is selected, and under Featured, Load Balancer is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image65.png "Azure Portal")

2.  In the **Create load balancer** blade, enter the following values:

    a.  Name: **WGWEBLB**

    b.  Type: **Internal**

    c.  **Virtual network: WGVNet2**

    d.  **Subnet:** **WebTier**

    e.  **IP address assignment:** click **Static** and enter the IP address **10.8.1.10**

    f.  **Subscription:** choose your subscription

    g.  **Resource group**: **Use existing** and select **WGVNETRG2**

    h.  **Location:** **South Central US**.

    Ensure your **Create load balancer** dialog looks like the following, and click **Create**.

    ![Load Balancer Check/Creation](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image66.jpeg "Create load balancer")

### Task 4: Configure the load balancer

1.  Open the WGWEBLB load balancer in the Azure portal.

2.  Click **Backend pools**, and click **+Add** at the top.

    ![In the Load balancer blade under Settings, Backend pools is selected, and the Add button is selected as well.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image67.png "Load balancer blade")

3.  Enter **LBBE** for the pool name. Under **Associated to**, choose **Availability set**.

    ![In the Name field in the Add backend pool blade is LBBE, and under Associated to, Availability set is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image68.png "Add backend pool blade")

4.  Next, select the **WebAVSet** Availability Set.

5.  Under **Target network IP configurations**, click **+ Add a target network IP configuration**.

    ![Under Target network IP configurations, Add a target network IP configuration is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image69.png "Target network IP configurations")

6.  Under **Target virtual machine**, choose **WGWEB1**.

    ![Under Target virtual machines, WGWEB1 size: Standard\_D1\_v2, network interfaces: 1 is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image70.png "Target virtual machines")

7.  Under **Network IP configuration**, choose **WGWEB1VMNIC1**.

8.  Click **+ Add a target network IP configuration** repeating these steps, but this time, adding **WGWEB2** along with its IP configuration.

9. Then, click **OK**.

    ![Backend pool addition.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image71.png "Add backend pool")

10. Wait to proceed until the Backend pool configuration is finished updating.

    ![Updating of the Backend pool configuration!](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image72.jpeg "Backend pool blade")

11. Next, under **Settings on the WGWEBLB Load Balancer blade** click on **Health** **Probes**. Click +**Add**, and use the following information to create a health probe.

    a.  Name: **HTTP**

    b.  Protocol: **HTTP**

    ![Under Settings, Health probes is selected. in the Add health probe blade, HTTP displays in the Name field, and Protocol is set to HTTP.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image73.png "Settings section, Add health probe blade")   ![Screenshot of the Add button.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image74.png "Add button")   ![In the Add health probe blade, Name is HTTP, and Protocol is HTTP.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image75.png "Add health probe blade")


13. Click **OK**.

    After the Health probe has updated. Click **Load balancing rules**. Click +**Add** and complete the configuration as shown below followed by clicking **OK**.\
    ![Configuration completion for adding a load balancer rule.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image76.jpeg "Add load balancing rule")

    **It will take 2-3 minutes for the changes to save.**

14. From an RDP session to WGWEB1, open your browser and point it at <http://10.8.1.10>. Press F5 until you see both servers responding. Note: If you are unable to get the servers to switch based on refreshing that is ok.

    ![A CloudShop Demo - Products - running on Web1 message displays. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image77.png "Server response")

    ![A CloudShop Demo - Products - running on Web2 message displays. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image78.png "Server response")

1. Using the portal, disassociate the public IP from the NIC of **WGWEB1** **VM**. Do this by selecting the Networking section below on the VM.

    ![Selection of the Networking section on the VM.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image79.png "Virtual machine networking blade")

16. Next, click the listed network interface **WGWEB1NetworkInterface** on the blue hyperlink.

17. Next, click the **IP configurations** section shown.

    ![Selection of the IP Configuration section.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image80.jpeg "Network interface blade")

18. Next, click the **ipconfig1** section on the NAME shown above.

19. Next, click and make sure that the **Public IP address settings** is shown disabled below, and click **Save**. This should remove the Public IP address from the network interface of the VM.

    ![Disabling of the Public IP address settings.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image81.png "IP configuration blade")

## Exercise 5: Build the management station

Duration: 15 minutes

In this exercise, management of the Azure-based systems will only be available from a management 'jump box.' In this section, you will provision this server.

### Task 1: Build the management VM

1.  In the Azure portal, click on the **New** button in the upper left of the portal. In the Marketplace category list, choose **Compute**. In the Featured Apps list, click **Windows Server 2016 Datacenter**.

    ![In the Azure Portal, the New button is selected. Under Azure Marketplace, Compute is selected. Under featured, Windows Server 2016 Datacenter is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image82.png "Azure Portal")

2.  On the **Basics** blade, shown in the following screenshot, enter the following information, and click **OK**.

    a.  Name: **WGMGMT1**

    b.  VM disk type: **SSD**

    c.  User name: **demouser**

    d.  Password: **demo\@pass123**

    e.  Subscription: **Choose your subscription**

    f.  Resource group: Choose **Create new** and enter **WGMGMTRG**

    g.  Location: **southcentral us**

    ![Fields in the Basics blade display the previously defined settings.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image83.png "Basics blade")

3.  On the **Choose a size** blade, click **F1S** (you will need to click **View all** and scroll down to find the F1S size). Click **Select**.

    ![In the Choose a size blade, F1S Standard is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image84.png "Choose a size blade")

4.  On the **Settings** blade, under **Storage**, select **No** for **Use managed disks**.

    ![Under Storage in the Settings blade, No is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image85.png "Settings blade")

5.  Under **Network**, click on the **Virtual network** section. On the **Choose Virtual Network** blade, click on **WGVNet**.

    ![In the Settings blade, under Network, Virtual network (new) WGMGMTRG-vnet is selected. In the Choose virtual network blade, WGVNet is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image86.png "Settings blade, Choose virtual network blade")

6.  In the **Subnet** section, click the subnet that was chosen, and choose **Management**.

    ![In the Settings blade, Subnet Management (10.7.0.8/29) is selected. In the Choose virtual network blade, Management WGVNetRG is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image87.png "Settings blade, Choose virtual network blade")

7.  In the **Public IP address** section, click the name that was pre-populated. Then, click the **Choose Public IP address** blade, and click **None**.

    ![In the Settings blade, Public IP address (new) WGMGMT1-ip is selected. In the Settings blade, Choose virtual network blade, None is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image88.png "Settings blade, Choose virtual network blade")

8.  Click on the **Network security group** **(firewall)** section, and in the **Choose network security group** blade, click **None**.

    ![In the Choose network security group blade, None is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image89.png "Choose network security group blade")

    **Note:** Because this server has no Public IP address and is only accessible through a firewall, an NSG is not required.

9.  Under **Monitoring**, for **Boot diagnostics**, choose **Disabled**.

    [In the Monitoring section, Boot diagnostics and Guest OS diagnostics are both disabled.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image90.png "Monitoring section")

10. The remaining sections of the **Settings** blade are correct. Click **OK**. See the following screenshot for details.

    ![Fields are set to the previously defined settings in the various blades. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image91.png "Settings blade")

1. On the **Summary** blade, ensure the validation passes, and click **Create**. The virtual machine will take 5-10 minutes to provision.

## Exercise 6: Virtual Network Peering

Duration: 20 Minutes

### Task 1: Configure VNet peering WGVNET1 to WGVNET2

1.  Click on the resource group **WGVNetRG1**, and select the configuration blade for **WGVNET1**. Click **Peerings**.

2.  Click **Add**.

    ![Add Peerings.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image92.jpeg "Virtual network blade")

3.  Name the new peering **VNETPeering** and check the box for Allow forwarded traffic, and click **OK** to create peerings.

    ![OK to create Peerings screen.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image93.jpeg "Add peering blade")

### Task 2: Configure VNet peering WGVNET2 to WGVNET1

1.  Repeat the steps of task 1 -- step 1 to step 3 to create peering from resource group **WGVNetRG2 Resource Group** and the **WGVNET2 Virtual Network**.

## Exercise 7: Provision and configure partner firewall solution

Duration: 15 minutes

In this exercise, you will provision and configure a Barracuda firewall appliance in Azure. This appliance is offered as a 'Free Trial' but deployments with recommended compute node which is standard A2.

### Task 1: Provision the firewall appliance

1.  Within the Azure portal, click **New** in the upper left corner of the portal. In the search dialog, type **barracuda cloudgen firewall for azure** and press the **Enter** key on your keyboard.

2.  A list of Marketplace offers is returned. Find the one called **barracuda cloudgen firewall for azure BYOL**, and click that option.

    ![Firewall appliance creation and provision.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image94.jpeg "Barracuda CloudGen Firewall for Azure (BYOL)")

3.  Click **Create**.

4.  On the **Basics** blade, shown in the following screenshot, enter the following information:

    a.  Name: **Barracudafw**

    b.  VM disk type: **SSD**

    c.  User name: **demouser**

    d.  Authentication type: Select **Password**

    e.  Password: **demo\@pass123**

    f.  Subscription: **Select your subscription**

    g.  Resource group: Select **Create New** and enter the name **barracudafw**

    h.  Location: **south central **

    Click **OK**.

    > Note: Look at the below screenshot just as an example. **Names** in the **Name** field and **Resource Group** of the devices could differ.

    ![Firewall appliance creation and provision.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image95.jpeg "Create virtual machine blade")

5.  Choose the **A2 Standard** instance size on the **Size** blade, and click **Select** at the bottom of the blade.

    **Note**: You may have to click the **View All** link to see the instance sizes.

    ![A2 Standard instance size selection.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image96.jpeg "Choose a size")

6.  On the **Settings** blade, choose **No** for **Use managed disks**.

7.  If the **Storage account** section shows **Create new**, select it, and create a new unique storage account name.

8.  Under **Network**, click the Virtual Network name listed by default.

9.  The **Choose Virtual Network** blade opens. Click on **WGVNet2** to select the Virtual Network you created earlier in this hands-on lab-step by-step. Click **OK.**

10. Back on the **Settings** blade, take note of the subnet that was selected. If it is not set to **PerimeterFirewall appliance creation and provision**, click on the subnet name, and change it **Perimeter**.

    ![Back on the Settings blade, take note of the subnet that was selected. If it is not set to PerimeterFirewall appliance creation and provision, click on the subnet name, and change it Perimeter.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image97.jpeg "Settings")

11. Back on the **Settings** blade, under **Public IP Address**, click on the name that was pre-assigned. This opens the **Choose Public IP address** and **Create Public IP address** blades. choose the default name for public IP and under **Assignment** select **Static**. Click **OK**.

    ![The static radio button is selected in the Assignment section.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image98.jpeg "Assignment")

12. Back on the **Settings** blade, click the **Network security group** section, and in the **Choose network security group** blade choose the **barracudafw-nsg. **

> Note: **NSG could be left none as** this server is a hardened firewall that has built-in security at the network layer. As such, an NSG is not required.

13. Change the Auto-Shutdown to **Off**.

    ![Under Auto-shutdown, Enable auto-shutdown is set to Off.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image99.png "Auto-shutdown section")

14. All remaining settings on the **Settings** blade are correct. Click **OK** to accept these settings.

15. On the **Summary** blade, ensure the validation passes, and click **Create**.

### Task 2: Enable IP forwarding on the firewall network interface

Within 1-2 minutes, the resource group **barracudafwrg** will be created and the appliance will be in the creation process. Next, we will edit settings on the network interface associated with the firewall.

1.  On the main Azure menu click on **Resource groups**.

    ![In the Azure Portal, Resource groups is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image100.png "Azure Portal")

2.  Click on the **barracudafwrg** resource group. This resource group contains the objects associated with the firewall appliance. Click on the network interface.

    ![Click on the barracudafwrg resource group. This resource group contains the objects associated with the firewall appliance. Click on the network interface.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image101.jpeg "Resource group view")

3.  This opens the **Essentials** and **Settings** blade for the network interface. On the **Settings** blade, click **IP configurations**.

    ![In the Settings section, IP configurations is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image102.png "Settings blade, Settings section")

4.  On the **IP configurations** blade, beside **IP forwarding settings**, click **Enabled**. Then, click **Save** at the top of the blade.

    ![On the IP configurations blade, beside IP forwarding settings, click Enabled. Then, click Save at the top of the blade.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image103.jpeg "IP configuration blade")

## Exercise 8: Configure the firewall to control traffic flow

Duration: 30 minutes

In this exercise, you will configure the firewall appliance to allow the necessary traffic to flow so that:

-   The web application is accessible from the Internet.

-   Application traffic can flow between the tiers.

-   An administrator can RDP into the management station, and from there, RDP into other servers for management purposes.

### Task 1: Log on to Barracuda firewall and monitor the traffic and firewall rules

1.  When the provisioning of the Barracuda appliance is finished, its **Essentials** blade and **Settings** blade will open in the portal. Take note of the **Public IP address** in the **Essentials** blade.

    ![When the provisioning of the Barracuda appliance is finished, its Essentials blade and Settings blade will open in the portal. Take note of the Public IP address in the Essentials blade.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image104.jpeg "Virtual machine blade")

2.  Download the most recent version of the NG Admin console after registering on the Barracuda Customer Portal at [**https://login.barracudanetworks.com**](https://login.barracudanetworks.com/) .

    When you get to the **Barracuda Portal** click **Create a New User**. You will be prompted to register with your name, email, and company. You will receive an email with further instructions to activate your account.

    There is no charge associated with registration or using the product on trial basis.

    Once logged in, you can navigate to [**https://dlportal.barracudanetworks.com**](https://dlportal.barracudanetworks.com/). From the download portal, click NextGen Admin on the left. **Search** for **Barracuda NextGen Admin 7.2.0 EA1**. Click on that link, and scroll down on the next page, and click the **download** button. Save the file. Click the .exe to launch the client

3.  In the Barracuda NextGen Admin 7.2 Log In dialog box, specify the following, and click **Sign In**.

    ![In the Barracuda NextGen Admin 7.2 Log In dialog box, specify the following, and click Sign In.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image105.jpeg "Barracuda login page")

    **Management IP**: the public IP address of the Barracuda NextGen virtual machine you identified in the Azure portal

    **Username**: root

    **Password**: demo\@pass123

    Click **Sign in**.

    ![Click \"Trust\" if an Authentication Check dialog box is prompted.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image106.jpeg "Authentication check")

4.  If prompted by the **Authentication Check** dialog box, click **Trust**. See above.

5.  If presented with an option to manage the appliance through the web interface or through NexGen Admin, choose to manage with **NexGen Admin**. You will be prompted twice to "**Manage with NextGen Admin**" click this **twice**

6.  Once the login completes, you will be presented with the Dashboard page of the Admin console.

    ![Once the login completes, you will be presented with the Dashboard page of the Admin console. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image107.jpeg "Barracuda admin")

7.  Click **CONTROL** in the top-level menu followed by **Network** in the next level menu. Verify that the Interface of the Barracuda NextGen virtual appliance has been assigned the IP address of **10.8.0.4/29** as expected

8.  Click **FIREWALL** in the top-level menu, followed by **History** in the next level menu. This allows you to view the traffic that reached the network interface of Barracuda NextGen virtual appliance, and, effectively, the traffic that you can control. You will be able to see the traffic on all the VMs. User defined routing has already been created. If you do not see much traffic, right after associating the subnets with the custom routes, you will be able to see the traffic. Also, if you RDP to the web servers, you will be able to see the traffic as well. It takes 10-15 minutes to capture the traffic in the history.

    ![Options of FIREWALL and History are selected in this spreadsheet. ](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image108.jpeg "Barracuda admin")

### Task 2: Configure firewall rules

1.  In the Barracuda Next Gen Admin console, click **CONFIGURATION** followed by **Configuration Tree**. You may need to click "Reconnect" if your session has timed out.

    ![Configuration option in the top-level menu is chosen.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image109.jpeg "Barracuda admin")

2.  In the Box window, expand **Virtual Servers**, **S1 EXPORT**, **Assigned Services**, **NGFW (Firewall)** and double-click **Forwarding Rules**.

    ![Forwarding rules options is expanded.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image110.jpeg "Barracuda admin") 

3.  Click **Lock** in the upper right.

    ![Click on the option \"lock.\"](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image111.jpeg "Barracuda admin")

4.  Click on thick + icon at the right to **Insert Rules (it should be a green plus icon)**

    ![Click the \"+\" icon.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image112.jpeg "Barracuda admin")

5.  **Feel free to create some firewall rules on your own. **

    **Example**: An **AnytoAny** rule is created below to allow all the traffic through the firewall to the web servers. You can **create custom firewall** rules if you prefer to harden security and allow filtered traffic.

    ![This represents an example of creating customized firewalls.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image113.jpeg "Barracuda admin")

### Task 3: Associate route tables to subnets

1.  Using the Azure portal, open the **WGVNetRG2** resource group.

2.  Click on **DataRT**, followed by **Subnets**.

    ![On the Route table blade, under Settings, Subnets is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image114.png "Route table blade")

3.  Click the **+Associate**.

    ![Screenshot of the Associate button.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image115.png "Associate button")

4.  On the **Associate subnet** blade, click on **Virtual network**. Then, click on **WGVNet2**.

    ![Under Associate subnet, Virtual network - Choose a virtual network is selected. Under Resource, WGVNet westus is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image116.png "Associate subnet section")

5.  From the **Choose a subnet** blade, click on **DataTier**.

    ![In the Choose a subnet blade Subnet, choose a subnet is selected, as is DataTier WGVNetRG.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image117.png "Choose a subnet blade")

6.  Click **OK** at the bottom of the **Associate subnet** blade.

7.  Click on the **WGVNetRG1** resource group, and click on **MgmtRT**, then **Subnets**.

8.  Click the **+Associate**.

    ![Screenshot of the Associate button.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image115.png "Associate button")

9.  On the **Associate subnet** blade, click on **Virtual network**. Click on **WGVNet1**.

    ![In the Associate subnet blade, Virtual network, Choose a virtual network is selected, as is WGVNet westus.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image116.png "Associate subnet blade")

10. The **Choose subnet** blade opens. Click on **Management**.

    ![In the Choose a subnet blade, Management WGVNetRG is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image118.png "Choose a subnet blade")

11. Click **OK** at the bottom of the **Associate subnet** blade.

12. Click on the **WGVNetRG2** resource group click on **WebRT** followed by **Subnets.**

13. Click the **+Associate**.

    ![Screenshot of the Associate button.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image115.png "Associate button")

14. On the **Associate subnet** blade, click on **Virtual network**. Then, click on **WGVNet2**.

    ![In the Associate subnet blade, Virtual network, Choose a virtual network is selected, as is WGVNet westus.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image116.png "Associate subnet blade")

15. The **Choose subnet** blade opens. Click on **WebTier**.

    ![In the Choose a subnet blade, Management WGVNetRG is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image119.png "Choose subnet blade")

16. Click **OK** at the bottom of the **Associate subnet** blade.

## Exercise 9: Configure Site-to-Site connectivity

Duration: 60 minutes

In this exercise, we will simulate an on-premises connection to the internal web application. To do this, we will first set up another Virtual Network in a separate Azure region followed by the Site-to-Site connection of the 2 Virtual Networks Finally, we will set up a virtual machine in the new Virtual Network to simulate on-premises connectivity to the internal load-balancer.

### Task 1: Create OnPrem Virtual Network

1.  Using the Azure Management portal, click **New**, **Networking**, and **Virtual network**.

![In the Azure Portal, New is selected. Under Azure Marketplace, Networking is selected, and under Featured, Virtual Network, Learn more is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image120.png "Azure Portal")

2.  See the following screenshot, and specify the configuration:

-   Name: **OnPremVNet**
-   Address space: **192.168.0.0/16**
-   Subscription: **Choose your Subscription**
-   Resource Group: Create new: **OnPremVNetRG**
-   Subnet name: **default**
-   Subnet address range: **192.168.0.0/24**
-   Location: **East US**
-   Make sure this is **not** the same location you have specified in the previous exercises.

    ![This represents the configuration of Creating an on-premises Virtual Network.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image121.jpeg "Create virtual network") |  |


### Task 2: Configure gateway subnets for on premise Virtual Network

1.  Click on the **OnPremVnetRG** Resource Group and then open the **OnPremVNet** blade, and click **Subnets.**

2.  Next, click +**Gateway subnet**.

    ![Click +Gateway subnet.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image122.jpeg "Virtual network blade")

3.  Specify the following configuration for the subnet, and click **OK**.

    -   Address range: **192.168.1.0/29**

    -   Route table: **None** (we will add later)

    ![Subnet Configuration selection followed by clicking ok.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image123.jpeg "Add subnet")

4.  Next, click on the Resource Group **OnPremVnetRG**, and click on the **OnPremVNet Virtual Network**. Add an on- premise Management Subnet to the **OnPremVNet shown below in the screenshot.**

    ![Addition of an on-premise Management Subnet.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image124.jpeg "Add subnet")

### Task 3: Create the first gateway

1.  Using the Azure Management portal, click **More services**, type **Virtual Network gateway** in the search window, and click **Virtual Networks Gateways**.

    ![In the Azure Portal, More services is selected. In the Search field, virtual network gateway is typed, and in the results, Virtual network gateways is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image125.png "Azure Portal")

2.  Click the **+Add** button on the toolbar.

    ![In the Virtual network gateways blade top menu, the Add button is circled.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image126.png "Virtual network gateways blade")

3.  Name the gateway **OnPremWGGateway**.

    ![Gateway name creation - OnPremWGGateway](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image127.jpeg "Choose virtual network")

4.  One of the last configurable options is the **Location**. Click to choose the Azure region where **onpremvnet** exists (East US if following this guide).

5.  In the **Virtual network** section, click **Choose a Virtual Network**, and click **OnPREMVNET**.

6.  Click the **Public IP address** tile, and click **Create new**.

7.  Name the IP **onpremwggateway**, and click **OK**.

    ![Name creation in the Create public IP address tile.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image128.jpeg "Create public IP address")

8.  Validate your settings look like the following screenshot, and click **Create**.

    ![Settings validation following by clicking Create.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image129.jpeg "Create virtual network gateway")

**NOTE:** The gateway will take 30-45 minutes to provision. Continue to the next section while waiting.

### Task 4: Create the second gateway

1.  Using the Azure Management portal, click **More services**, type **Virtual Network gateway** in the search window, and click **Virtual Networks Gateways**.

    ![In the Azure Portal, More services is selected. The Search field is set to virtual network gateway, and in the results, Virtual network gateways is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image125.png "Azure Portal")

2.  Click the **Add** button on the toolbar.

3.  Name the gateway **WGVNetGWVNet1**

4.  One of the last configurable options is the **Location**. Click to choose the Azure region where **WGVNet1** exists (Southcentral US if following this guide).

**Note: If you don't choose the location right after you put the name, right Virtual Network will not appear to create the Gateway.**

5.  In the **Virtual network** section, click **Choose a Virtual Network** followed by **WGVNet1**

    ![In the Virtual Network section, Virtual network, Choose a virtual network is selected, as is OnPremSimVNet.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image130.png "Virtual Network section")

6.  Click the **Public IP address** tile, and click **Create new**.

    ![In the Virtual network section, Public IP address, Choose a public IP address is selected, as is the Create new button.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image131.png "Virtual network section")

7.  Name the IP **VNETGWIP**, and click **OK**.

8.  Validate your settings look like the following screenshot, and click **Create**.

    ![Settings validation followed by clicking Create.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image129.jpeg "Create virtual network gateway")

Note: The gateway will take 30-45 minutes to provision. You will need to wait until both gateways are provisioned before proceeding to the next section.

9.  The Azure portal will notify you when the deployments have completed.

### Task 5: Connect the gateways

1.  Using the Azure Management portal, click **New**, type in **Connection**, and press **Enter**.

    ![In the Azure portal, both New and connection are selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image132.png "Azure Portal")

2.  Click **Connection**, and click **Create**.

    ![Under Results, Connection is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image133.png "Results section")

3.  On the **Basics** blade, leave the **Connection type** set to **VNet-to-VNet**. Select the existing **WGVNetRG1** resource group. Then, change the location of this connection to the Azure region the **WGVNet1** Virtual Network is deployed to (Southcentral). Click **OK**.

    ![Basics Tab - Connection Type/Subscription/Resource Group selections](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image134.jpeg "Basics")

4.  On the Settings tab, select **OnPremWGGateway** for the first Virtual Network gateway and **WGVNetGWVNet1** for the second Virtual Network gateway. Ensure **Establish bidirectional connectivity** is selected. Enter a Shared key, such as **A1B2C3D4** for example. After your settings reflect the below screenshot, click **OK**.

    ![On the Settings tab, select OnPremWGGateway for the first Virtual Network gateway and WGVNetGWVNet1 for the second Virtual Network gateway. Ensure Establish bidirectional connectivity is selected. Enter a Shared key, such as A1B2C3D4 for example. After your settings reflect the below screenshot, click OK.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image135.jpeg "Choose virtual network gateway")

    **Second gateway**

    ![Second Gateway - On the Settings tab, select OnPremWGGateway for the first Virtual Network gateway and WGVNetGWVNet1 for the second Virtual Network gateway. Ensure Establish bidirectional connectivity is selected. Enter a Shared key, such as A1B2C3D4 for example. After your settings reflect the below screenshot, click OK.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image136.jpeg "Choose virtual network gateway")

    Finally, the configuration will look like the following screenshot after selecting all the fields:

    ![Gateway Configuration Summary Confirmation](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image137.jpeg "Settings")

5.  Click **OK** on the **Summary** page to create the connection.

6.  Using the Azure Management portal, click **More services**. Then, type **connections** in the search window, and select **Connections**.

    ![In the Azure portal, Browse is selected. In the search field, connections is typed. In the results section, Connections is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image138.png "Azure Portal")

7.  Watch the progress of the connection status, and use the **Refresh** icon until the status changes for both connections from **Unknown** to **Connected**. This may take 5 -- 10 minutes or more.

    ![Connections - Watch the progress of the connection status, and use the Refresh icon until the status changes for both connections from Unknown to Connected. This may take 5 -- 10 minutes or more.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image139.jpeg "Connections blade")

## Exercise 10: Validate connectivity from 'on-premises' to Azure

Duration: 30 minutes

In this exercise, you will validate connectivity from your simulated on-premises environment to Azure.

### Task 1: Create a virtual machine to validate connectivity

1.  Create a new Virtual Machine in the second Virtual Network by clicking **New**, **Compute**, and **Windows Server 2016 Datacenter**.

2.  Specify the following configuration, and click **OK**. See the following screenshot for more details.

-   Name: **OnPremVM**
-   User name: **demouser** 
-   Password: **demo\@pass123**
-   Resource Group: Create new: **OnPremVMRG** 
-   Location: **the region you created the OnPremVNet Virtual Network in (East US).** 
  
    ![Configuration specification while creating a virtual machine](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image140.jpeg "Basics blade") 

3.  On the **Size** blade, choose **F1 Standard**, and click **Select**.

    ![The F1 Standard option displays on the Size blade.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image141.png "Size blade")

4.  On the **Settings** blade, change the Virtual network to **OnPremVNet**, and set the subnet to the default subnet named: **default**.

    ![Virtual network, OnPremSimVNet is selected in the Network section in the Settings blade.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image142.png "Settings blade")

5.  Click **OK** twice to provision the virtual machine.

### Task 2: Configure routing for simulated 'on-premises' to Azure traffic

When packets arrive from the simulated 'on-premises' Virtual Network (OnPremVNet) to the 'Azure-side' (WGVNET1), they arrive at the gateway WGVNETGWVNET1. This gateway is in a gateway subnet (10.7.0.0/29). For packets to be directed to the barracuda firewall, we need another route table and route to be associated with the gateway subnet on the 'Azure' side.

1.  On the main portal menu to the left, click **More services** at the bottom. Enter **route** in the search box, and click on **Route tables**.

    ![In the Azure Portal, More services is selected. The search field is set to route, and in the results section, Route tables is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image34.png "Azure Portal")

2.  On the **Route tables** blade, click **Add**.

3.  On the **Route table** blade, enter the following information:

    a.  Name: **WGAzureVNetGWRT**

    b.  Subscription: **Choose your subscription**

    c.  Resource group: Select **Use existing**, click the drop-down menu, and select **WGVNetRG1**

    d.  Location: Same region where WGVnet exists

    e.  Click **Create**.

4.  Click on **WGAzureVNetGWRT** route table.

    ![Selecting the WGAzureVNetGWRT route table. - Screenshot 1/2](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image143.jpeg "Create route table")

    ![Selecting the WGAzureVNetGWRT route table. - Screenshot 2/2](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image144.jpeg "Route tables")

5.  Click **Routes**.

6.  On the **Routes** blade, click the **+Add** button. Enter the following information, and click **OK**:

    f.  Route name: **OnPremToWebTier**

    g.  Address prefix: **10.8.1.0/24**

    h.  Next hop type: **Virtual appliance**

    i.  Next hop address: **10.8.0.4**

    ![Routes blade: information entry](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image145.jpeg "Add route")

7.  Using the breadcrumb menu at the top of the portal, navigate back to the **WGAzureVNetGWRT** route table settings.

    ![WGAzureVNetGWRT is selected on the Azure Portal menu.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image146.png "Azure Portal ")

8.  On the **Settings** blade, click on **Subnets**.

9.  On the **Subnets** blade click on the **Associate** link.

    ![Screenshot of the Associate link.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image147.png "Associate link")

10. On the **Associate subnet** blade click on **Virtual Network**. Then, click on **WGVNet1**.

    ![WGVNet1 Selection](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image148.jpeg "Associate subnet")

11. The **Choose subnet** blade opens. Click on **Gateway Subnet**. Then, click **OK** at the bottom to complete the association.

    ![Opening of the Choose subnet blade - Clicking ok to complete the association.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image149.jpeg "Choose subnet")

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Enterprise-class networking in Azure hands-on lab step-by-step, you will want to delete the Resource Groups. This will free up your subscription from future charges.

