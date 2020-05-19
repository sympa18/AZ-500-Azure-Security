---
lab:
    title: '04 - MFA, Conditional Access and AAD Identity Protection'
    module: 'Module 01 - Manage Identity and access'
---

# Lab 04: MFA, Conditional Acccess and AAD Identity Protection

All tasks in this lab are performed from the Azure portal, except for steps in Exercise 2 performed within a Remote Desktop session to an Azure VM.

Lab files:

-  **2020\\Allfiles\\Labs\\LAB_04\\az-101-04b_azuredeploy.json**

-  **2020\\Allfiles\\Labs\\LAB_04\\az-101-04b_azuredeploy.parameters.json**

### Scenario

Adatum Corporation wants to take advantage of Azure AD Premium features for Identity Protection.


### Objectives

After completing this lab, you will be able to:

 -  Deploy an Azure VM by using an Azure Resource Manager template

 -  Implement Azure MFA

 - Implement a  Conditional Access Policy 

 -  Implement Azure AD Identity Protection



### Exercise 0: Prepare the lab environment

The main tasks for this exercise are as follows:

 1. Deploy an Azure VM by using an Azure Resource Manager template


#### Task 1: Deploy an Azure VM by using an Azure Resource Manager template

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab.

1. In the Azure portal select **Create a resource**.

1. From the **New** blade, search Azure Marketplace for **Template deployment (deploy using custom templates)**.

1. Use the list of search results to navigate to the **Template deployment (deploy using custom templates)** blade. 

1. Click **Create**

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **2020\\Allfiles\\Labs\\LAB_04\\az-500-04_azuredeploy.json**.

     > **Note**: Review the content of the template and note that it defines deployment of an Azure VM hosting Windows Server 2016 Datacenter.

1. Save the template and return to the **Custom deployment** blade.

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **2020\\Allfiles\\Labs\\LAB_04\\az-500-04_azuredeploy.parameters.json**.

1. Save the parameters and return to the **Custom deployment** blade.

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: **Create new** and name it **AZ500LAB04**

    - Location: the name of the Azure region which is closest to the lab location and where you can provision Azure VMs

    - Vm Size: **Standard_DS1_v2**

    - Vm Name: **az500-04-vm1**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az500-04-vnet1**

     > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

1. Click on **I agree to the terms and conditions stated above**

1. Click **Purchase**

> **Note**: Do not wait for the deployment to complete but proceed to the next exercise. You will use the virtual machine included in this deployment in the last exercise of this lab.


> **Result**: After you completed this exercise, you have initiated a template deployment of an Azure VM **az500-04-vm1** that you will use in the next exercise of this lab.



### Exercise 1: Implement Azure MFA

The main tasks for this exercise are as follows:

 1. Create a new Azure AD tenant

 1. Activate Azure AD Premium v2 trial

 1. Create Azure AD users and groups

 1. Assign Azure AD Premium v2 licenses to Azure AD users

 1. Configure Azure MFA settings, including fraud alert, trusted IPs, and app passwords

 1. Validate MFA configuration


#### Task 1: Create a new Azure AD tenant

1. In the Azure portal, click on **Create a resource**.

1. From the **New** blade, search Azure Marketplace for **Azure Active Directory** and select it from the drop down menu.

1. Click **Create**

1. From the **Create directory** blade, create a new Azure AD tenant with the following settings:

  - Organization name: **AdatumLab500-04**

  - Initial domain name: a unique name consisting of a combination of letters and digits.

  - Country or region: **United States**

     > **Note**: Take a note of the initial domain name. You will need it later in this lab.

1. Click **Create**


#### Task 2: Activate Azure AD Premium v2 trial

1. In the Azure portal, either click on the link to manage your new directory or set the **Directory + subscription** filter to **AdatumLab500-04** (the newly created Azure AD tenant.)

     > **Note**: The **Directory + subscription** filter is located to the right of the Cloud Shell icon in the toolbar of the Azure portal

     > **Note**: You might need to refresh the browser window if the **AdatumLab500-04** entry does not appear in the **Directory + subscription** filter list.

1. In the Azure portal, navigate to the **AdatumLab500-04 - Overview** blade.

1. Select **Azure Active Directory**

1. Under **Manage**, click the **Licenses** icon.

1. Under **Manage** select the **All products** icon.

1. click **+ Try / Buy**, under Azure AD Premium P2 click **Free Trial** , and then click **Activate**.


#### Task 3: Create Azure AD users and groups.

1. In the Azure portal, navigate to the **Azure Active Directory**, then click the **Users** blade of the AdatumLab500-04 Azure AD tenant.

1. From the **Users - All users** blade, create a new user with the following settings:

    - User name: **aaduser1** 

      > **Note**: Take a note of the full user principl name. You will need it later in this lab.

    - Name: **aaduser1**
    
    - Password: ensure that the option **Auto-generate password** is selected, check the checkbox **Show Password** and note the string appearing in the **Password** text box. You will need it later in this lab.

    - Groups: **0 groups selected**

    - Directory role: Choose and **Select** the **Global administrator** role

    - Usage Location: **United States**  

    1. Click **Create**
    
1. From the **Users - All users** blade, create a new user with the following settings:

    - User name: **aaduser2**

      > **Note**: Take a note of the full user principl name. You will need it later in this lab.

    - Name: **aaduser2**

    - Password: ensure that the option **Auto-generate password** is selected, check the checkbox **Show Password** and note the string appearing in the **Password** text box. You will need it later in this lab.

    - Groups: **0 groups selected**

    - Directory role: **User**

     - Usage Location: **United States**  

1. Click **Create**

1. From the **Users - All users** blade, create a new user with the following settings:

    - User name: **aaduser3**

      > **Note**: Take a note of the full user principl name. You will need it later in this lab.

    - Name: **aaduser3**

    - Password: ensure that the option **Auto-generate password** is selected, check the checkbox **Show Password** and note the string appearing in the **Password** text box. You will need it later in this lab.

    - Groups: **0 groups selected**

    - Directory role: **User**

     - Usage Location: **United States**  

1. Click **Create**

#### Task 4: Assign Azure AD Premium v2 licenses to Azure AD users

1. From the **aaduser1 - Profile** blade, under **Manage** click the **Licenses** icon, click **+ Assignments** and assign to the user an **Azure Active Directory Premium P2** license with all licensing options enabled. Click **Save**

1. From the **aaduser2 - Profile** blade,  under **Manage** click the **Licenses** icon, click **+ Assignments** and assign to the user an Azure Active Directory Premium P2 license with all licensing options enabled. Click **Save**

1. From the **aaduser3 - Profile** blade,  under **Manage** click the **Licenses** icon, click **+ Assignments** and assign to the user an Azure Active Directory Premium P2 license with all licensing options enabled. Click **Save**

1. From **your user account profile**, Click **Edit** and under **Settings** change the **Usage location** to **United States** then click **Save**

1. From **your user account profile**, under **Manage** click the **Licenses** icon, click **+ Assignments** and assign to the user an **Azure Active Directory Premium P2** license with all licensing options enabled. Click **Save**

1. Sign out from the portal and sign back in using the same account you are using for this lab.

     > **Note**: This step is necessary in order for the license assignment to take effect.


#### Task 5: Configure Azure MFA settings.

1. In the Azure portal, navigate to the **Azure Active Directory** blade of the AdatumLab500-04 Azure AD tenant.

1. Under **Manage** Click the **Security** icon

1. Under **Manage** click **MFA**

1. From the **Geting Started** blade, under **Configure** select the link **Additional cloud-based MFA settings**. This will bring up a seperate tab to configure MFA

1. On the **multi-factor authentication** portal, in **service settings** tab, review its settings, and the **verification options**, note that **Text message to phone**, **Notification through mobile app**, and **Verification code from mobile app or hardware token** are enabled. Click **Close**

1. On the **multi-factor authentication** portal, switch to the **users** tab, select **aaduser1** entry, and enable its multi-factor authentication status.

1. On the **multi-factor authentication** portal, note that the multi-factor authentication status of **aaduser1** changed to **Enabled** and that, once you select the user entry again, you have the option of changing it to **Enforced**.

     > **Note**: Changing the user status from enabled to enforced impacts only legacy, Azure AD integrated apps which do not support Azure MFA and, once the status changes to enforced, require the use of app passwords.

1. On the **multi-factor authentication** portal, with the **aaduser1** entry selected, display the **Manage user settings** window and review its options, including:

    - Require selected users to provide contact methods again

    - Delete all existing app passwords generated by the selected users

    - Restore multi-factor authentication on all remembered devices

1. Click **Cancel** and switch back to the Azure portal, without making any changes.

1. In the Azure portal, navigate to the **Azure Active Directory** blade of the AdatumLab500-04 Azure AD tenant.

1. Under **Manage** Click the **Security** icon

1. Under **Manage** click **MFA**

1. Under **Settings** click the **Fraud alert** icon and configure the following settings:

    - Allow users to submit fraud alerts: **On**

    - Automatically block users who report fraud: **On**

    - Code to report fraud during initial greeting: **0**

1. Click **Save**


#### Task 6: Validate MFA configuration

1. Open an InPrivate Microsoft Edge window.

1. In the new browser window, navigate to the Azure portal and sign in using the **aaduser1** user account. When prompted, change the password to a new value.

     > **Note**: You will need to provide a fully qualified name of the **aaduser1** user account, including the Azure AD tenant DNS domain name, as noted earlier in this lab. e.g. aaduser1@adatumlab50004.onmicrosoft.com where you replace **adatumlab50004** with your unique domain name. 

1. When prompted with the **More information required** message, continue to the **Additional security verification** page by clicking **Next** .

1. On the **How should we contact you?** page, note that you need to set up one of the following options:

    - **Authentication phone**

    - **Mobile app**

1. Select the **Authentication phone** option with the **Send me a code by text message** method.

1. Complete the verification and note the automatically generated app password if one is created for you.

1. When prompted, change the password from the one generated when you created the **aaduser1** account. Take note of the new password

1. Verify that you successfully signed in to the Azure portal.

1. Sign out as **aaduser1** and close the InPrivate browser window.

### Exercise 2: Implement Conditional Access Policy

The main tasks for this exercise are as follows:

1. Confidure a conditional access policy

1. Validate the conditional access policy by simulating the activity.



# Task 1 - Configure the Portal policy

In this task we will review conditional access policy settings and create a policy that requires MFA when signing in to the Portal. 

1. In the Azure portal, either manage your new directory or set the Directory + subscription filter to AdatumLab500-04 (the newly created Azure AD tenant.)
1. In the **Portal** search for and select **Azure Active Directory**.
1. Under **Manage** select **Security**.
1. Under **Protect** select **Conditional access**.
1. Click **New Policy**.
	* Name: **AZ500Policy1**
	* Users and groups > Select users and groups > Users and Groups > Select: **aaduser2** > Select **Done**
	* Cloud apps or actions > Select apps > Select: **Microsoft Azure Management** 
	* Review the warning that this policy impacts Portal access > Select **Done**
	* Conditions > Sign-in risk > Review the risk levels
	* Device platforms > Review the devices that can included, such as Android and iOS.
	* Locations > Review the physical location selections.
	* Under **Access controls** click **Grant**.
	* Review the Grant options such as MFA. You may require one or more of the controls.
	* Select **Require multi-factor authentication**, Click **Select**
	* For **Enable policy** select **On**. > Click **Create**. 

    > NOTE If you get an error stating that "Security defaults must be disabled to enable Conditional access policy". you will need to do the following step. From **Azure Active Directory** > **Properties**  > **Manage Security defaults** > Change to **No** > **My Organization is using Conditional Access** > Click **Save**. You can change this back after the lab ends.

# Task 2 - Test the policy


1. Open an InPrivate Microsoft Edge window.

1. In the new browser window, navigate to the Azure portal https://portal.zure.com and sign in using the **aaduser2** user account.
1. Click **Next**, provide a mobile number for verification and click **Next**
1. Type in the text code sent to you to verify. Click **Done**
1. Change the users password and take note of the new password
1. You have now verified that your conditional access policy has enforced MFA when aaduser2 signs into the Azure portal.
1. Sign back into the portal with your standard user account and navigate back to **Azure Active Directory**.
1. Under **Manage** select **Security**.
1. Under **Protect** select **Conditional access**.
1. Click the elipsis next to AZ500Policy1 and select **Delete**

### Exercise 3: Implement Azure AD Identity Protection:

The main tasks for this exercise are as follows:

1. Enable Azure AD Identity Protection

1. Configure user risk policy

1. Configure sign-in risk policy

1. Validate Azure AD Identity Protection configuration by simulating risk events


#### Task 1: Enable Azure AD Identity Protection

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using the Microsoft account you used to create the **AdatumLab500-04** Azure AD tenant.

     > **Note**: Ensure that you are signed-in to the **AdatumLab500-04** Azure AD tenant. You can use the **Directory + subscription** filter to switch between Azure AD tenants.

1. In the Azure portal, click **All services**.

1. From the **All services** blade, search for **Azure AD Identity Protection**.

1. Select the **Azure AD Identity Protection** in the list of search results.

#### Task 2: Configure user risk policy

1. From the **Azure AD Identity Protection** blade

1. Under **Protect** select **user risk policy**

1. Configure the **User risk remediation policy** with the following settings:

    - Assignments:

        - Users: **All users** (be sure to **exclude** your current admin account to avoid getting locked out of the tenant)

        - Conditions:

            - User risk: **Low and above**

    - Controls:

        - Access: **Allow access**

        - **Require password change**

    - Enforce Policy: **On**
    - Click **Save**


#### Task 3: Configure sign-in risk policy

1. Under **Protect** select **Sign-in risk policy**

1. Configure the **Sign-in risk remediation policy** with the following settings:

    - Assignments:

        - Users: **All users**

        - Conditions:

            - User risk: **Medium and above**

    - Controls:

        - Access: **Allow access**

        - **Require multi-factor authentication**

    - Enforce Policy: **On**

    - Click **Save**


#### Task 4: Validate Azure AD Identity Protection configuration by simulating risk events

   > **Note**: Before you start this task, ensure that the template deployment you started in Exercise 0 has completed.

1. In the Azure portal, set the **Directory + subscription** filter to the **Default Directory** (the original Azure AD tenant.)

1. In the Azure portal, click on Virtual Machines from the portal manu then navigate to the **az500-04-vm1** blade.

1. From the **az500-04-vm1** blade, connect to the Azure VM via Remote Desktop session and, when prompted to sign in, provide the following credentials:

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

1. Within the Remote Desktop session, in Server Manager, click **Local Server** and then click **IE Enhanced Security Configuration**.

1. In the **Internet Explorer Enhanced Security Configuration** dialog box, set both options to **Off** and click **OK**.

1. Within the Remote Desktop session, open an InPrivate Internet Explorer window.

1. In the new browser window, navigate to the ToR Browser Project at [**https://www.torproject.org/projects/torbrowser.html.en**](https://www.torproject.org/projects/torbrowser.html.en), download the ToR Browser, and install it with the default options.

1. Once the installation completes, start the ToR Browser, use the **Connect** option on the initial page, and navigate to the Application Access Panel at [**https://myapps.microsoft.com**](https://myapps.microsoft.com)

1. When prompted, sign in with the **aaduser3** account you created in the previous exercise.

1. You will be presented with the message **Your sign-in was blocked**. This is expected, since this account is not configured with multi-factor authentiation, which is required due to increased sign-in risk associated with the use of ToR Browser.

1. Use the **Sign out and sign in with a different account option** to sign in as **aaduser1** account you created and configured for multi-factor authentication in the previous exercise.

1. This time, you will be presented with the **Suspicious activity detected** message. Again, this is expected, since this account is configured with multi-factor authentiation. Considering the increased sign-in risk associated with the use of ToR Browser, you will have to use multi-factor authentication, according to the sign-in risk policy you configured in the previous task.

1. Use the **Verify** option and specify whether you want to verify your identity via text or a call.

1. Complete the verification and ensure that you successfully signed in to the Application Access Panel.

1. Sign out as **aaduser1** and close the ToR Browser window. 

1. Navigate back to the original browser session and select **Virtual Machines** then **az500-04-vm1**

1. Select **Stop** and then **OK** to Deallocate the VM.

1. Start Internet Explorer, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using the Microsoft account you used to create the **AdatumLab500-04** Azure AD tenant.

1. In the Azure portal, set the **Directory + subscription** filter to **AdatumLab500-04** (the newly created Azure AD tenant.)

     > **Note**: The **Directory + subscription** filter is located to the right of the Cloud Shell icon in the toolbar of the Azure portal

1. In the Azure portal, navigate to the **Azure AD Identity Protection - Risk Detections** blade and note that the entry representing **Sign-in from anonymous IP address**.

1. From the **Azure AD Identity Protection** Under **Report** select **Risk Detections**
Notice that we have multiple users with Risk detections from the sign in attempts.

1. Under **Report** select **Risky users** and note the entry representing **aaduser3**.

1. From the **Azure AD Identity Protection - Risky users** blade, navigate to the **Azure AD Identity Protection - Risky sign-ins** blade and note the entry representing **aaduser3** and **aaduser1**.

> **Result**: After you completed this exercise, you have enabled Azure AD Identity Protection, configured user risk policy and sign-in risk policy, as well as validated Azure AD Identity Protection configuration by simulating risk events



## Exercise 3: Clean up policies 

To avoid isues with the next lab lets remove the Identity protection policies and the Conditional access policies

1. From the **Azure AD Identity Protection** blade

1. Under **Protect** select **user risk policy**

1. Set **Enforce policy** to **off** > Click **Save**

1. Under **Protect** select **Sign-in risk policy**

1. Set **Enforce policy** to **off** > Click **Save**

1. NOTE Do not remove any of the resources from this lab as Lab 05 PIM has a dependancy on it. 
