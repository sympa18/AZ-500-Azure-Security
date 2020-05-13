---
lab:
    title: '10 - Key Vault (Implementing Secure Data by setting up Always Encrypted)'
    module: 'Module 03 - Secure data and applications'
---

# Lab 10 - Key Vault (Implementing Secure Data by setting up Always Encrypted)

# Student lab manual

## Lab scenario

Contoso has a new application that has a compliance requirement for the data to be encrypted at all times, at rest in transit and in use. Furthermore they also need to ensure that they keep any and all secrets, and certificates in a secure location. 

In this lab, you will get started with Azure Key Vault to create a hardened container (a vault) in Azure, to store and manage cryptographic keys and secrets in Azure. First you will use Azure PowerShell. Then you will store a password as a secret that could then be used with an Azure application.

## Objectives

Contoso would like to create proof of concept appplication that makes use of Always encrypted in SQL. They would like to ensure that they store and secrets, certificates and keys in Azure Key Vault where possible. They would also like to combine this with some of the advantages in Azure app registration to improve the security posture of the application.

In this lab, you will:

+ Deploy an Azure Key Vault
+ Add a secret to Azure Key Vault 
+ Enable a client application
+ Use Key Vault to Encrypt Data with Azure SQL Database 

## Instructions

### Exercise 1

#### Task 1: xxx


## Exercise 1: Introduction to Azure Key Vault


### Task 1: Download SQL Server Management Studio

1.  To download the latest version of SQL Management Studio required for this lab visit the following link and select download SQL Management Studio **`https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017`**

     **Note:** You do not need to wait for the SQL Management Studio to install   before continuing.

### Task 2: Use PowerShell to create a Key Vault


In this exercise, you will use PowerShell to create an Azure Key Vault.


1.  Start PowerShell by clicking **Start > PowerShell**

1.  Use the following command to authenticate to Azure using the account for your Azure subscription.

    ```
    Login-AzAccount
    ```

1.  Create a new Resource Group 

    ```
    New-AzResourceGroup -Name 'AZ500LAB10' -Location 'eastus'
    ```


1.  Create a key vault in the resource group. **The VaultName must be unique therefore change <keyvault name> to something unique.**

    ```
    New-AzKeyVault -VaultName '<keyvault name>' -ResourceGroupName 'AZ500LAB10' -Location 'eastus'
    ```

    **Note**: The output of this shows important pieces of information: Vault Name in this case that is KeyVaultPS and the Vault URI: `https://KeyVaultPS.vault.azure.net`



1.  In the Azure Portal open the **AZ500LAB10** Resource Group.

1.  Click on the KeyVaultPS to examine what you have created.

1. Click **Access Policies** > **+ Add Access Policy**

1. Select **Key, Secret and Certificate Management** from **Configure from template (optional)**

1. Make sure for each of **Key, Secret and Certificate** permissions you select the dropdown box and click **Select All**

1. You should have 16 selected for Key, 8 selected for Secret and 16 selected for Certificate.

1. Click **Select Principal** and search for and then click on your account, then click on **Select**

1. Click **Add** and then **Save**

### Task 3: Add a key and secret to Key Vault

1.  Return to the PowerShell window.

1.  Add a software-protected key to the Key Vault using this command. Be sure to change the placeholder text to your vault name.

    ```
    $key = Add-AZKeyVaultKey -VaultName '<YourVaultName>' -Name 'MyLabKey' -Destination 'Software'
    ```

1.  Move back to **KeyVaultPS** in the Azure portal. Click **Keys** under Settings in the left navigation pane.

1.  Click **MyLabKey**

1.  Click the Current Version.

1.  Examine the information about the key you created.

    **Note**: You can always reference this key by using its URI. To get the most current version, just reference `https://keyvaultps.vault.azure.net/keys/MyLabKey/` or if need be the exact version: `https://keyvaultps.vault.azure.net/keys/MyLabKey/da1a3a1efa5dxxxxxxxxxxxxxd53c5959e`


1.  Move back to the PowerShell window. To display the current version of the key, enter the following command.

    ```
    $Key.key.kid
    ```


1.  To view the Key you just created you can use the Get-AzureKeyVaultKey cmdlet. Be sure to change the placeholder text to your vault name.

    ```
    Get-AZKeyVaultKey -VaultName '<YourVaultName>'
    ```


### Task 4: Add a Secret to Key Vault

1.  Next, you will add a secret to the **KeyVaultPS**. To do this, add a variable named **$secretvalue** using the following code.

    ```
    $secretvalue = ConvertTo-SecureString 'Pa55w.rd1234' -AsPlainText -Force
    ```


1.  Next add the secret to the Vault with this command. Be sure to change the placeholder text to your vault name.

    ```
    $secret = Set-AZKeyVaultSecret -VaultName 'YourVaultName' -Name 'SQLPassword' -SecretValue $secretvalue
    ```

1.  Move back to the Azure Portal on **KeyVaultPS** and click **Secrets**

1.  Click the Secret **SQLPassword**

1.  Click the current version

1.  Examine the Secret that you created

    **Note**: You can always reference this key by using its URI. To get the most current version just reference `https://keyvaultps.vault.azure.net/secrets/SQLPassword` or if need be the exact version: `https://keyvaultps.vault.azure.net/secrets/SQLPassword/c5aada85d3acxxxxxxxxxxe8701efafcf3`


1.  Click the **Show secret value** button -- notice that the password appears.

1.  To view the Secret, use the Get-AzureKeyVaultSecret cmdlet. Be sure to change the placeholder text to your vault name.

    ```
    Get-AZKeyVaultSecret -VaultName 'YourVaultName'
    ```

### Task 5: Enable a Client Application


You will enable your client application to access the Azure SQL Database service. This will be done by setting up the required authentication and acquiring the Application ID and Secret that you will need to authenticate your application. These steps will be accomplished in the Azure portal.


1.  Open the Azure portal and navigate to Azure Active Directory.

1.  Click **App Registrations** under **Manage** in the left navigation pane.

1.  **Click + New registration**

1.  Provide the name **sqlApp** for your application. Under **Redirect URI (optional)**, select **Web**, and for the SIGN-ON URL type **`https://sqlapp`**

5.  Click **Register**.

6.  Once the App Registration is complete click on **sqlApp** if it does not automatically appear.

7.  Copy your Application (client) ID as you will need it later.

8.  Click **Certificates & secrets**

9.  Click **+ New client secret**

10.  In the **Description** section, enter **Key1** for the description. Select **1 year** from the **Expires** list, then click **Add**


1.  Copy the Key1 value as you will need it later. If you close and reopen the blade, the value will show as hidden.

### Task 6: Add a Key Vault Policy allowing the application access to the Key Vault.

1.  In the **Azure portal** open your **Resource Group** created at the beginning of the lab

1.  Select the **Azure Key** vault

1.  Click **Access Policies**

1.  Select the account associated with your Azure subscription

1.  In the **Key Permissions** drop down select **Select All** to highlight all permissions

1.  Select **Save**

    **Important**! You must click save otherwise the permissions will not be committed 


8.  Run the following Powershell in the **Powershell ISE** to set the sqlApp key permissions replacing the placeholder text with **your account details**

    
    ```
    $subscriptionName = '[Azure_Subscription_Name]'
    $applicationId = '[Azure_AD_Application_ID]'
    $resourceGroupName = '[Resource_Group_with_KeyVault]'
    $location = '[Azure_Region_of_KeyVault]'
    $vaultName = '[KeyVault_Name]' 
    ```
    
    ```
    Login-AzAccount
    ```
    
    ```
    Set-AZKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
    ```


 
### Task 7: Use Key Vault to Encrypt Data with Azure SQL Database


**Scenario**

In this task, you will create a blank Azure SQL Database, connect to it with SQL Server Management Studio and create a table. You will then encrypt two data columns using an autogenerated key from the Azure Key Vault. Then you will create a Console application using Visual Studio to Load data into the Encrypted Columns and then access that data securely using a connection string that accesses the key via Key Vault.


1.  From the Azure Portal click **+ Create a resource> Databases > SQL Database**

1.  Provide the following details on the SQL Database blade and click **Create**.

      - Resource Group: (use existing) **AZ500LAB10**
      
      - Database Name: **medical**

      - Server: **Create new**

          - Server name: **[Unique Server Name]**

          - Server Admin Login: **demouser**

          - Password: **Pa55w.rd1234**

          - Location: **[same location as KeyVaultPS]**

          - Then click **OK**

      - Pricing Tier: Standard S0



1.  Once everything above is configured, select **Review + create,** then **Create**

1.  Once the SQL Database is deployed, open it in the Azure Portal to locate and then copy the **ADO.NET Connection String**.

**Note**: When you save the connection string for future use, be sure to replace {your_username} with **demouser** and {your_password} with **Pa55w.rd1234**.


### Task 8: Create a Table in the SQL Database

1.  Use the Azure portal to locate the Server name where the Medical Database is located and copy the name.



1.  On this same blade click Set Server firewall.



1.  Next click **+ Add client IP** and then click **Save**.



1.  Open SQL Server Management Studio. Connect to the Server using these properties for the **Connect to Server** dialog.

    - Server Type: **Database Engine**

    - Server Name: **[found on the Database Overview Blade]**

    - Authentication: **SQL Server Authentication**

    - Login: **demouser**

    - Password: **Pa55w.rd1234**


### Task 9: Create and Encrypt a Table

1.  In SQL Server Management Studio expand **Databases > Right-click medical > New Query**.

1.  Paste the following code into the query window and click Execute

     ```
     CREATE TABLE [dbo].[Patients](
    
     [PatientId] [int] IDENTITY(1,1),

     [SSN] [char](11) NOT NULL,

     [FirstName] [nvarchar](50) NULL,

     [LastName] [nvarchar](50) NULL,

     [MiddleName] [nvarchar](50) NULL,

     [StreetAddress] [nvarchar](50) NULL,

     [City] [nvarchar](50) NULL,

     [ZipCode] [char](5) NULL,

     [State] [char](2) NULL,

     [BirthDate] [date] NOT NULL 

     PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
     ```


1.  After the table is created successfully, expand **medical > tables > right-click dbo.Patients** and select **Encrypt Columns**.



1.  Click **Next**.

1.  On the Column Selection Screen check **SSN** and **Birthdate**. Then set the Encryption Type for SSN to **Deterministic** and for Birthdate **Randomized**. Click **Next**.



1.  On the Master Key Configuration page on the Select the Key store provider, click **Azure Key Vault.** Click **Sign in** and authenticate. Select your Azure Key Vault. Click **Next**.



1.  On the Run Settings screen click **Next** and then **Finish** to Proceed with the encrypting.



1.  When the encryption process is complete, click **Close** and expand **medical > security > Always Encrypted Keys** and note that now there are keys found.



### Task 10: Build a Console Application to work with Encrypted Columns

1.  Open Visual Studio 2019 and Sign in using your Azure account.

1.  Click **File > New > Project**

    > NOTE in the next step **DO NOT CHOOSE** **Console App (.NET Core)**  
 
1.  Next select **Visual C#** > Seach for **Console App** > Choose **Console App (.NET Framework)** and provide the name **OpsEncrypt** in the location **C:\\** and then click **OK**.


1.  **Right-Click** the **OpsEncrypt** project > click **Properties**.



1.  Change the **Target Framework** to **.NET Framework 4.7.2.** Click **Yes** when prompted to change the **Target Framework.**



1.  Install the following **NuGet** packages by going to **Tools** > **NuGet Package Manager** > **Package Manager Console.**

    ```
    Install-Package     Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    ```

    ```
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1.  Open the **program.cs** file in notepad from Allfiles\\Labs\\Mod1_Lab02 and copy the code.

1.  Replace the code in **Program.cs** in Visual Studio with the code you just copied.

1.  Locate the **Connection string, clientId, and clientSecret** settings in the Main method and replace them with the values that you copied from the previous steps.

1.  **Click** the **Start Button** in **Visual Studio**.


1.  **The Console Application** will **Build** and then start. First it will ask for your password, then the app will add data to the database.

    - Server Password: **Pa55w.rd1234**

1.  **Leave** the **Console Application Running** and move to the **SQL Management Studio**. Right-Click the medical database and click **New Query**.



1.  Run the following query to see the data that was loaded into the database is encrypted.

    ```
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
    ```


1.  Now, move back to the console application where you will be asked to **Enter** a **Valid SSN**. This will query the encrypted column for the data. Notice that with the key called from the Key Vault, now the data is unencrypted and shown to the console window.

    ```
    999-99-0003
    ```

1.  To **Exit** you press enter.

### Task 11: Remove resources.

1. Open Cloud Shell in Powershell

1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):
    ```
    Remove-AzResourceGroup -Name "AZ500LAB10"
    ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Result**: In this exercise, you removed the resources used in this lab.

**Results** : You have now completed this Lab.


