---
lab:
    title: '01 - Role Based Access Control'
    module: 'Module 01 - Manage Identity and access'
---

# Lab 01 - Role Based Access Control

# Student lab manual

# Module 1: Lab 01: Role Based Access Control 

## Lab scenario

In this module, you'll learn about Role-Based Access Control as the foundation to organizing and managing an organization's administrative access based on the principle of least privilege. You will also review Azure Active Directory concepts, as well as gain insight into the threat landscape and security risks that are exposed to IT organizations through breach of privileged access. Lessons include:

### Objectives

After completing this lab, you will be able to:
- Configure Role-Based Access Control


## Exercise 1: Role-Based Access Control

### Task 1: Create a User

1.  Sign in to the Azure portal **`https://portal.azure.com/`**

1.  Select **Azure Active Directory** and on the overview blade note down your tenant domain for later use.


1.  Select **Users**, and then select **New user**.


1.  On the **User** page, fill out the blade with the following information:

      - **User name**: bill
      - **Name**: Bill Smith

1. Click on the copy icon next to the **User name** and note your **user name** later use.

1. Under **Inital password** Click **Show password**. note this password for later use in the initial sign-in process.

1.  Select **Create**.

    The user is created and added to your Azure AD tenant.

1.  Launch **Azure Cloud Shell** by clicking on the PowerShell icon at the top right of the Azure Portal and select PowerShell if prompted. (If prompted click **Create storage**)

  
1.  **Enter the following commands** to create a user in the PS cloud shell **replacing yourdomain** with your domain noted down erlier

      ```powershell
      $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
      ```
      ```powershell
      $PasswordProfile.Password = "Pa55w.rd"
      ```
      ```
      Connect-AzureAD
      ```
      ```powershell
      New-AzureADUser -DisplayName "Mark" -PasswordProfile $PasswordProfile     -UserPrincipalName "Mark@yourdomain.onmicrosoft.com" -AccountEnabled $true -MailNickName "Mark"
      ```
 
1.  Run the following comamand to get a list of the users in Azure AD 

      ```powershell
      Get-AzureADUser 
      ```
 
1.  from the top left menu in Azure Cloud Shell select **Bash** by using the drop down menu and click **Confirm**

1.  Enter the following command in **azure CLI** to create a user in Azure CLI **replacing yourdomain** with the domain you noted earlier.
 
      ```cli
      az ad user create --display-name Tracy --password Pa55w.rd --user-principal-name Tracy@yourdomain.onmicrosoft.com
      ```
      ```cli
      az ad user list --output table
      ```

1. You should now have 3 users in your Azure AD
1. Close the Azure Cloud Shell.


### Task 2: Create Groups In Portal, PowerShell, and CLI

1.  In the top left of the Azure Portal screen. Click on the **three lines** that represent the portal menu and then click on **Azure Active Directory**

1. Under **Manage** click **Groups** and select **New group**.
 
1.  Fill in the details with the following details:
  
       - **Group Type**: Security
       - **Group Name**: Senior Admins Group 
    
1.  Under  **Members** click **No owners selected** and select **Bill Smith**, click **Select**

1.  Click **Create**

1.  Launch the **Cloud Shell Bash** by clicking the PowerShell icon at the top of the Azure Portal.

1.  In the **Cloud Shell** enter the following command to create a new security group named ServiceDesk:

      ```cli
      az ad group create --display-name ServiceDesk --mail-nickname ServiceDesk
      ```

20.  Change the Cloud Shell to **PowerShell** and enter the following command to create a new Security group called JuniorAdmins:

      ```powershell
      Connect-AzureAD
      ```
      ```powershell
      New-AzureADGroup -DisplayName "Junior Admins" -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
      ```
 
1.  Exit the **Cloud Shell**.

1.  In the **Active Directory blade** click **Groups** and confirm you have **3** groups

1. Close the Cloud Shell session.


## Exercise 2: Practice - RBAC

### Task 1: Create a resource group

1. Click on **Create a Resource** 

1. Type in **Resource Group** and select **Resource Group**.

1.  Click **Create**.

1.  For **Resource group name**, enter **myRBACrg**

1.  Select your subscription and the location of **East US**.

1.  Choose **Review + create** then **Create** to create the resource group.

1. When the Resource creation is complete. Click on the Portal Menu (Top left three lines) and select **Resource Groups** 

1. The new resource group appears in your resource groups list.

### Task 2: Grant access


In RBAC, to grant access, you create a role assignment.


1.  In the list of **Resource groups**, choose the new **myRBACrg** resource group.

1.  Choose **Access control (IAM)** to see the current list of role assignments.


1.  Choose **Add** to open the **Add role assignment** pane.

    If you don't have permissions to assign roles, you won't see the **Add** option.

1.  In the **Role** drop-down list, select **Virtual Machine Contributor**.

1.  In the **Select** list, select **Bill Smith**.

1.  Choose **Save** to create the role assignment.

   After a few moments, the user is assigned the Virtual Machine Contributor role at the myRBACrg resource group scope.

  
### Task 3: Remove access


In RBAC, to remove access, you remove a role assignment.


1.  Click the Role Assignments tab.

1.  In the list of role assignments, add a checkmark next to Bill Smith with the Virtual Machine Contributor role.
  
1.  Choose **Remove**.

1.  In the remove role assignment message that appears, choose **Yes**.  
   
  
## Exercise 3:  Role-based Access Control (RBAC) using PowerShell


In this exercise you use PowerShell to :

-   Use the `Get-AzRoleAssignment` command to list the role assignments
-   Use the `Remove-AzResourceGroup` command to remove access


### Task 1: Grant access
  

To grant access for the user, you use the New-AzRoleAssignment command to assign a role. You must specify the security principal, role definition, and scope.  


1.  Launch the **Cloud Shell** and select **PowerShell**.
  
1.  Get the ID of your subscription using the **`Get-AzSubscription`** command and put it into a variable.
  
      ```powershell
      $subScope = "/subscriptions/"+(Get-AzSubscription).id
      ``` 
  
1.  Assign the Reader role to the user at the subscription scope by using the following command (**replacing your domain with the tenant domain you noted earlier**):
  
      ```powershell
      New-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Reader" -Scope $subScope  
      ```
  
  
1.  Assign the Contributor role to the user at the resource group scope using the following command:
  
      ```powershell
      New-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Contributor" -ResourceGroupName "myRBACrg"
      ```

  
### Task 2: List access  
  
1.  To verify the access for the subscription, use the Get-AzRoleAssignment command to list the role assignments use the following command:
  
      ```powershell
      Get-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -Scope $subScope
      ```

       
    In the output, you can see that the Reader role has been assigned to Bill Smith at the subscription scope.

2.  To verify the access for the resource group, use the Get-AzRoleAssignment command to list the role assignments using the following command:
  
    ```powershell
    Get-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com     -ResourceGroupName "myRBACrg"
    ```


 In the output, you can see that both the Contributor and Reader roles have been assigned to the Bill Smith. The Contributor role is at the myRBACrg resource group scope and the Reader role is inherited at the subscription scope.

### Task 3: Remove access
  

To remove access for users, groups, and applications, use `Remove-AzRoleAssignment` to remove a role assignment.


1.  Use the following command to remove the Contributor role assignment for the user at the resource group scope.
  
    ```powershell
    Remove-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Contributor" -ResourceGroupName "myRBACrg"
    ```
  
  
1.  Use the following command to remove the Reader role assignment for the user at the subscription scope.

    ```powershell
    Remove-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Reader" -Scope $subScope
    ```
 
1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):
  
    ```powershell
    Remove-AzResourceGroup -Name "myRBACrg"
    ```

1.  Close the **Cloud Shell**.  


**Results**: You have now completed this lab.
