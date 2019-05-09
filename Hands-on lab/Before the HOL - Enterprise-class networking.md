![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
May 2019
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Enterprise-class networking in Azure before the hands-on lab setup guide](#enterprise-class-networking-in-azure-before-the-hands-on-lab-setup-guide)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Create a virtual machine to execute the lab in](#task-1-create-a-virtual-machine-to-execute-the-lab-in)
        - [Task 2: Update Azure PowerShell version](#task-2-update-azure-powershell-version)
        - [Task 3: Download hands-on lab step-by-step support files](#task-3-download-hands-on-lab-step-by-step-support-files)

<!-- /TOC -->

# Enterprise-class networking in Azure before the hands-on lab setup guide

## Requirements

You must have a working Azure subscription to carry out this hands-on lab step-by-step without a spending cap to deploy the Barracuda firewall from the Azure Marketplace.

## Before the hands-on lab

Duration: 15 minutes

If you are working on a machine that cannot run PowerShell, carry out this task. Only do this if you are not running the commands on your local machine and are provisioning a VM to perform the steps.

### Task 1: Create a virtual machine to execute the lab in

1.  Launch a browser, and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If asked, choose whether your account is an organization account or just a Microsoft Account.

1.  Select **+Create a resource**, and in the search box, type in **Visual Studio**, and press enter. In the list of results, select the **Visual Studio Community 2017 (latest release) on Windows Server 2016 (x64)** image.

1.  On the **Create a virtual machine** blade, on the **Basics** tab, set the following configuration and choose **Next : Disks**:

    -  Subscription: **If you have multiple subscriptions, choose the subscription to execute your labs in**.

    -  Resource Group: **OPSLABRG**

    -  Virtual machine name: **LABVM**

    -  Region: **Choose the closest Azure region to you**

    -  Availability options: **No infrastructure redundancy required**

    -  Image: **Visual Studio Community 2017 (latest release) on Windows Server 2016 (x64)**

    -  Size: **Standard D2 v3** or **Standard DS1 v2**

    -  User name: **demouser**

    -  Password: **demo\@pass123**

    -  Public inbound ports: **Allow selected ports**

    -  Select inbound ports: **RDP**

    -  Already have a Windows license?: **No**

    -  VM disk type: **SSD**

    >**Note**: If the Azure Subscription you are using is **NOT** a trial Azure subscription, you may want to choose the **Standard D2 v3** to have more power in this LABMV. If you are using a Trial Subscription or one that you know has a restriction on the number of cores, stick with **Standard DS1 v2**.

1.  On the **Create a virtual machine** blade, on the **Disks** tab, set the following configuration and choose **Review + create**:

    -  OS disk type: **Standard SSD**

1.  On the **Create a virtual machine** blade, on the **Disks** tab, set the following configuration and choose **Review + create**:

1.  Ensure that the validation passed and select **Create**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    >**Note:** Please wait for the LABVM to be provisioned prior to moving to the next step.

1.  Wait for deployment status of **LABVM** to complete. Once the deployment blade displays the message **Your deployment is complete**, select **Go to resource**. 

1.  On the **LABVM** blade, first select **Connect** and then select **Download RDP file** to establish a Remote Desktop session.

    ![The Connect button is circled on the Azure Portal top menu bar.](images/Setup/image13.png "Azure Portal")

1.  Depending on your Remote Desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

1.  Log in with the credentials specified during creation:

    a.  User: **demouser**

    b.  Password: **demo\@pass123**

1.  You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Choose **Yes** to continue with the connection.

    ![The Remote Desktop Connection warning displays, and the Yes button is selected.](images/Setup/image14.png "Remote Desktop Connection warning")

1.  When logging on for the first time, there will be a prompt asking about network discovery. Select **No**.

    ![On the Network Discovery prompt, the No button is selected.](images/Setup/image15.png "Network Discovery prompt")

1.  Notice that Server Manager opens by default. Select **Local Server**.

    ![On the Server Manager menu, Local Server is selected.](images/Setup/image16.png "Server Manager menu")

1.  In the details pane, ensure the **IE Enhanced Security Configuration** is set to **Off**. If that is not the case, select **On**. 

    ![In the Essentials section, IE Enhanced Security Configuration is set to On, and is selected.](images/Setup/image17.png "Essentials section")

1.  Change the setting to **Off** for Administrators, and select **OK**.

    ![In the Internet Explorer Enhanced Security Configuration dialog box, Administrators are set to Off, and the OK button is selected.](images/Setup/image18.png "Internet Explorer Enhanced Security Configuration dialog box")

### Task 2: Update Azure PowerShell version

1.  While logged into **LABVM** via Remote Desktop, from the Start menu, open Windows PowerShell. In the **Administrator: Windows PowerShell** window, run the following:

    ```
    Install-Module -Name Az -AllowClobber
    ```

1.  When prompted for confirmation, press the **Y** key twice. This will automatically install the Az PowerShell module.

### Task 3: Download hands-on lab step-by-step support files

1.  Within the Remote Desktop session to **LABVM**, open Internet Explorer and download the zipped hands-on lab step-by-step student files by selecting this link: <https://github.com/microsoft/MCW-Enterprise-class-networking/tree/master/Hands-on%20lab/labfiles/ECN-Hackathon.zip>

1.  Extract the downloaded files into the directory **C:\\ECN-Hackathon**.

    ![In File Explorer, ECN-Hackathon is selected, and from its right-click menu, Extract All is selected.](images/Setup/image23.png "File Explorer")

    ![In the Extract Files window, Files are being extracted to C:\\ECH-Hackathon, and the Extract button is selected.](images/Setup/image24.png "Extract Files window")

You should follow all steps provided *before* performing the Hands-on lab.
