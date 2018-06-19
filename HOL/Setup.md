
# Enterprise Class Networking setup

## Requirements

You must have a working Azure subscription to carry out this hands-on lab step-by-step without a spending cap to deploy the Barracuda firewall from the Azure Marketplace.


## Before the hands-on lab

Duration: 15 minutes

If you are working on a machine that cannot run PowerShell, carry out this task. Only do this if you are not running the commands on your local machine and are provisioning a VM to perform the steps.

### Task 1: Create a virtual machine to execute the lab in

1.  Launch a browser, and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If asked, choose whether your account is an organization account or just a Microsoft Account.

1.  Select **+NEW**, and in the search box, type in **Visual Studio Community 2017 on Windows Server 2016 (x64)**, and press enter. Select the Visual Studio Community 2017 image running on Windows Server 2016 with the latest update.

3.  In the returned search results, select the image name.

    ![In the Azure Portal, Everything blade, the search field is set to Visual Studio Community 2017 on Windows Server 2016 (x64), and under Results, Visual Studio Community 2017 on Windows Server 2016 (x64) is selected.](images/Setup/image3.png "Azure Portal, Everything blade")

4.  In the Marketplace solution blade, at the bottom of the page keep the deployment model set to **Resource Manager**, and choose **Create**.

    ![Resource Manager is selected from the Select a deployment model drop-down list box.](images/Setup/image4.png "Select a deployment model ")

5.  Set the following configuration on the Basics tab, and choose **OK**.

    -   Name: **LABVM**

    -   VM disk type: **SSD**

    -   User name: **demouser**

    -   Password: **demo\@pass123**

    -   Subscription: **If you have multiple subscriptions, choose the subscription to execute your labs in.**

    -   Resource Group: **OPSLABRG**

    -   Location: **Choose the closest Azure region to you.**

    ![All fields in the Basics blade are set to the previously defined settings.](images/Setup/image5.png "Basics blade")

6.  Choose the **DS1\_V2 Standard** or **F2S** instance size on the Size blade.

    > Note: You may have to select the View All link to see the instance sizes.

    ![On the Choose a size blade, the DS1\_V2 Standard option is selected.](images/Setup/image6.png "Choose a size blade")

**Note**: If the Azure Subscription you are using is [NOT]{.underline} a trial Azure subscription, you may want to choose the DS2\_V2 to have more power in this LABMV. If you are using a Trial Subscription or one that you know has a restriction on the number of cores, stick with the DS1\_V2.

7.  Select **Configure required settings** to specify a storage account for your virtual machine if a storage account name is not automatically selected for you.

    ![On the Settings blade, Configure required settings is selected.](images/Setup/image7.png "Settings blade")

8.  Select **Create New**.

    ![Screenshot of the Create new button.](images/Setup/image8.png "Create new button")

9.  Specify a unique name for the storage account (all lower letters and alphanumeric characters), and ensure the green checkmark shows the name is valid.

    ![In the Name field, a green checkmark is called out.](images/Setup/image9.png "Name field")

10. Select **OK** to continue.

    ![Screenshot of the OK button.](images/Setup/image10.png "OK button")

11. Select **Configure required settings** for the Diagnostics storage account if a storage account name is not automatically selected for you. Repeat the previous steps to select a unique storage account name. This storage account will hold diagnostic logs about your virtual machine that you can use for troubleshooting purposes.

    ![Screenshot of the Configure required settings option.](images/Setup/image11.png "Configure required settings")

12. Accept the remaining default values on the Settings blade, and choose **OK**. On the Summary page, select **OK**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    ![Screenshot of the Deploying Visual Studio Community 2017 announcement.](images/Setup/image12.png "Deploying Visual Studio Community")

NOTE: Please wait for the LABVM to be provisioned prior to moving to the next step.

13. Move back to the Portal page on your local machine, and wait for **LABVM** to show the Status of **Running**. Choose **Connect** to establish a new Remote Desktop Session.

    ![The Connect button is circled on the Azure Portal top menu bar.](images/Setup/image13.png "Azure Portal")

14. Depending on your Remote Desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

15. Log in with the credentials specified during creation:

    a.  User: **demouser **

    b.  Password: **demo\@pass123**

16. You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Choose **Yes** to continue with the connection.

    ![The Remote Desktop Connection warning displays, and the Yes button is selected.](images/Setup/image14.png "Remote Desktop Connection warning")

17. When logging on for the first time, there will be a prompt asking about network discovery. Select **No**.

    ![On the Network Discovery prompt, the No button is selected.](images/Setup/image15.png "Network Discovery prompt")

18. Notice that Server Manager opens by default. Select **Local Server**.

    ![On the Server Manager menu, Local Server is selected.](images/Setup/image16.png "Server Manager menu")

19. On the side of the pane, choose **On** by **IE Enhanced Security Configuration**.

    ![In the Essentials section, IE Enhanced Security Configuration is set to On, and is selected.](images/Setup/image17.png "Essentials section")

20. Change to **Off** for Administrators, and select **OK**.

    ![In the Internet Explorer Enhanced Security Configuration dialog box, Administrators are set to Off, and the OK button is selected.](images/Setup/image18.png "Internet Explorer Enhanced Security Configuration dialog box")

### Task 2: Update Azure PowerShell version

1.  While logged into **LABVM** via Remote Desktop, open Internet Explorer, and navigate to <http://aka.ms/webpi-azps>. This will download an executable. After the download is finished, select **Run** to execute it.

    ![In the Download box, the Run button is selected.](images/Setup/image19.png "Download box")

2.  A Web Platform Installer dialog box will open. Choose **Install** to install the latest version of the Azure PowerShell module (your version may differ from the screenshot). Note: the version on the virtual machine may already be up-to-date.

    ![In the Microsoft Azure PowerShell Web Platform Installer dialog box, an Install button is selected.](images/Setup/image20.png "Microsoft Azure PowerShell dialog box")

3.  Accept the license terms by select **I Accept**.

    ![In the Web Platform Installer 5.0 license terms dialog box, the I Accept button is selected.](images/Setup/image21.png "Web Platform Installer 5.0 license terms dialog box")

4.  Select **Finish** to complete the installation.

    ![In the Web Platform Installer 5.0 Finish dialog box, the Finish button is selected.](images/Setup/image22.png "Web Platform Installer 5.0 Finished dialog box")

5.  After the installation is complete, **reboot** the machine you installed Azure PowerShell on.

### Task 3: Download hands-on lab step-by-step support files

1.  After the reboot has completed, download the zipped hands-on lab step-by-step student files by selecting this link: <https://cloudworkshop.blob.core.windows.net/enterprise-networking/ECN-Hackathon.zip>

2.  Extract the downloaded files into the directory **C:\\ECN-Hackathon**.

    ![In File Explorer, ECN-Hackathon is selected, and from its right-click menu, Extract All is selected.](images/Setup/image23.png "File Explorer")

    ![In the Extract Files window, Files are being extracted to C:\\ECH-Hackathon, and the Extract button is selected.](images/Setup/image24.png "Extract Files window")

