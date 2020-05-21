---
lab:
    title: '01 - Role Based Access Control'
    module: 'Module 01 - Manage Identity and Access'
---

# Lab 01 - Role Based Access Control

# Student lab manual

## Lab scenario

You have been asked to create new users and groups.

- Create a Senior Admins group with member Joseph Price. 
- Create a Junior Admins group with member Isabel Garcia. 
- Create a Service Desk group with member Dylan Williams.  

You have also been tasked to assign roles to groups.

- Create a resource group and assign the Service Desk group Virtual Machine Contributor permissions. 

## Lab objectives

In this lab, you will use different tools to:

- Exercise 1: Create the Senior Admins group with member Joseph Price. 
- Exercise 2: Create the Junior Admins group with member Isabel Garcia.
- Exercise 3: Create the Service Desk group with member Dylan Williams. 
- Exercise 4: Assign the Service Desk group Virtual Machine Contributor permissions.

## Exercise 1: Create the Senior Admins group with member Joseph Price. 

### Estimated timing: 10 minutes

In this exercise, you will complete:

- Task 1: Use the Portal to create the user account for Joseph Price.
- Task 2: Use the Portal to create the Senior Admins group and assign the Joseph to the group.

#### Task 1: Use the Portal to create the user account for Joseph Price. 

In this task, you will create a user account for Joseph Price. 

1. Sign-in to the Azure portal **`https://portal.azure.com/`**.

1. Under **Azure services** select **Azure Active Directory**. You may need to scroll right to locate this service.

1. On the **Overview blade** note your tenant domain. You will need this in later steps. This takes the form *xxx.onmicrosoft.com*.

1. Under **Manage** select **Users**, and then select **New user**.

1. On the **User** page, fill out the blade with the following information:

      - **User name**: Joseph
      - **Name**: Joseph Price

1. Click on the copy icon next to the **User name**. 

1. Under **Initial password** check **Show password**. You would need to provide this information to Joseph. 

1. Click **Create**.

1. Refresh the **Users** page to verify the new user was created in your Azure AD tenant.

### Task 2: Create the group in the Portal

In this task, you will create the *Senior Admins* group and assign Joseph as the owner. 

1. Continue in the Portal and the Azure Active Directory blade.

1. Under **Manage** click **Groups**, and then select **New group**.
 
1. Complete the group details.
  
       - **Group Type**: Security
       - **Group Name**: Senior Admins 
    
1. Under  **Members** click **No members selected** and select **Joseph Price**, click **Select**.

1. Click **Create**.

> Exercise result: Used the Azure Portal to create a user and a group, and assigned the user to the group. 

## Exercise 2: Create the Junior Admins group with member Isabel Garcia.

### Estimated timing: 10 minutes

In this exercise, you will:

- Task 1: Use PowerShell to create a user account for Isabel Garcia.
- Task 2: Use PowerShell to create the Junior Admins group and assign Isabel to the group. 

#### Task 1: Use PowerShell to create a user account for Isabel Garcia.

In this task, you will create a user account for Isabel Garcia.

1. Open the Cloud Shell by clicking the first icon in the top right of the Azure Portal. If prompted, select **PowerShell** and **Create storage**.

1. Ensure **PowerShell** is selected in the upper-left drop-down menu of the Cloud Shell pane.

	> To copy and paste to the shell, right-click in the pane and **Paste as plain text**. 

	> Bookmark the [Azure PowerShell page](https://docs.microsoft.com/en-us/powershell/module/az.resources/?view=azps-4.1.0#resources). The Reference section provides commands grouped by category, like Active Directory and Virtual Machines. 

1. Create a password profile object.

    ```
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```

1. Create a password.

    ```
    $PasswordProfile.Password = "Pa55w.rd"
    ```

1. Connect to Azure Active Directory. This is required when using PowerShell. 

    ```
    Connect-AzureAD
    ```
      
1. Create a user, Isabel Garcia. Substitute *yourdomain* with your domain. 

	```
    New-AzureADUser -DisplayName "Isabel Garcia" -PasswordProfile $PasswordProfile   -UserPrincipalName "Isabel@yourdomain.onmicrosoft.com" -AccountEnabled $true -MailNickName "Isabel" 
    ```

1. Notice the UserPrincipalName for Isabel. You will need this to add her to the Junior Admins group. 

1. Get a list of users. Both, Mark and Isabel, should be listed. 

	```
	Get-AzureADUser 
	```

### Task 2: Use PowerShell to create the Junior Admins group and assign Isabel to the group.

In this task, you will create the Junior Admins group and assign Isabel to the group. 

1. Continue in the Cloud Shell. 

1. Create a new security group named Junior Admins.
	
	```
	New-AzureADGroup -DisplayName "Junior Admins" -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
	```

1. Verify the group was created. You should now have two groups, Senior Admins and Junior Admins.  

    ```
    Get-AzureADGroup
    ```

1. Assign Isabel to the Junior Admins group. Use *yourdomain*. 
	
	```
	Add-AzADGroupMember -MemberUserPrincipalName "isabel@yourdomain.onmicrosoft.com" -TargetGroupDisplayName "Junior Admins" 
	```

1. Check the Junior Admins group membership and ensure Isabel is a member.

	```
	Get-AzADGroupMember -GroupDisplayName "Junior Admins"
	```

> Exercise result: Using PowerShell you created a user and a group, and assigned the user to the group. 


## Exercise 3: Create the Service Desk group with member Dylan Williams. 

### Estimated timing: 10 minutes

In this exercise, you will:

- Task 1: Use the CLI to create a user account for Dylan Williams.
- Task 2: Use the CLI to create the Service Desk group and assign Dylan to the group. 

#### Task 1: Use the CLI to create a user account for Dylan Williams.

	> Bookmark the [CLI reference page](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest). The Reference section has a command list with usage information. 

In this task, you will create a user account for Dylan Williams.. 

1. Continue in the Cloud Shell.

1. Use the top left drop-down to select **Bash** and then click **Confirm**.

1. Create a user, Dylan Williams. Use *yourdomain*.
 
      ```
      az ad user create --display-name "Dylan Williams" --password Pa55w.rd --user-principal-name Dylan@yourdomain.onmicrosoft.com
      ```
      
1. Get a list of users. Joseph, Isabel, and Dylan should be listed. You will need Dylan's ObjectID to add him to the Service Desk group. 
	
      ```
      az ad user list --output table
      ```

#### Task 2: Use the CLI to create the Service Desk group and assign Dylan to the group. 

In this task, you will create the Service Desk group and assign Dylan to the group. 

1. Continue in the Cloud Shell.

1. Create a new security group named Service Desk.

   	```
	az ad group create --display-name "Service Desk" --mail-nickname ServiceDesk
   	```
 
1. Verify the Service Desk group was created. You should also have the Senior Admins and Junior Admins groups.  

   	```
	az ad group list -o table
	```

1. Assign Dylan to the Service Desk group. Use Dylan's ObjectID for Member-id. 

   	```
	az ad group member add --group "Service Desk" --member-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   	```

1. Check the Service Desk group membership and ensure Dylan is a member.

	```
	az ad group member list --group -o table
	```

> Exercise result: Using the CLI you created a user and a group, and assigned the user to the group. 


## Exercise 4: Assign the Service Desk group Virtual Machine Contributor permissions. 

### Estimated timing: 10 minutes

In this exercise, you will:

- Task 1: Create a resource group. 
- Task 2: Assign the Service Desk Virtual Machine Contributor permissions to the resource group.  

#### Task 1: Create a resource group

1. Sign-in to the Azure portal **`https://portal.azure.com/`**.

1. Under **Azure services** select **Resource groups**. 

1. Click **Add**. Consult your instructor for the preferred region. 

- Resource group name: **AZ500Lab01**
- Subscription: **your subscription**
- Location: **East US**

1. Click **Review + create** and then **Create**.

1. Use the **Notification** icon (top right) to get information on deployment status.

1. Wait for the resource group to deploy.

1. Click on the Portal Menu (Top left three lines) and select **Resource Groups**.

1. Verify your new resource group is listed. 

### Task 2: Assign the Service Desk Virtual Machine Contributor permissions. 

1. In the list of **Resource groups**, choose the new **AZ500LAB01** resource group.

1. Select **Access control (IAM)**.

1. In the **Add role assignment** pane, select **Add**.

	> If you don't have permissions to assign roles, you won't see the **Add** option.

1. In the **Role** drop-down list, select **Virtual Machine Contributor**.

1. Use the Information icon to view the role privileges. 

1. In the **Select** list, select **Service Desk**.

1. Choose **Save** to create the role assignment.

1. From the **Access control (IAM)** blade, select **Role assignments**.

1. Verify under **Virtual Machine Contributor** the **Service Desk** group is listed. 

1. Select **Check access**.

1. Under **Find** in the second text box type **Dylan Williams**.

1. Select the user account to review Dylan's access as part of the Service Desk group.

1. Return to **Check access**.

1. Check the access for **Joseph Price**. 

> Exercise result: You have created a resource group and assigned Virtual Machine Contributor permissions to the resource group. 


-----------------------
------------------------
------------------------

** George - I'm not sure how much more is needed for this lab. This is a lot of scripting. ** 

After few moments, the user is assigned the Virtual Machine Contributor role at the AZ500LAB01 resource group scope.

  
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
  
      ```
      $subScope = "/subscriptions/"+(Get-AzSubscription).id
      ``` 
  
1.  Assign the Reader role to the user at the subscription scope by using the following command (**replacing your domain with the tenant domain you noted earlier**):
  
      ```
      New-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Reader" -Scope $subScope  
      ```
  
  
1.  Assign the Contributor role to the user at the resource group scope using the following command:
  
      ```
      New-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Contributor" -ResourceGroupName "AZ500LAB01"
      ```

  
### Task 2: List access  
  
1.  To verify the access for the subscription, use the Get-AzRoleAssignment command to list the role assignments use the following command:
  
      ```
      Get-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -Scope $subScope
      ```

       
    In the output, you can see that the Reader role has been assigned to Bill Smith at the subscription scope.

2.  To verify the access for the resource group, use the Get-AzRoleAssignment command to list the role assignments using the following command:
  
    ```
    Get-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com     -ResourceGroupName "AZ500LAB01"
    ```


 In the output, you can see that both the Contributor and Reader roles have been assigned to the Bill Smith. The Contributor role is at the AZ500LAB01 resource group scope and the Reader role is inherited at the subscription scope.

### Task 3: Remove access
  

To remove access for users, groups, and applications, use `Remove-AzRoleAssignment` to remove a role assignment.


1.  Use the following command to remove the Contributor role assignment for the user at the resource group scope.
  
    ```
    Remove-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Contributor" -ResourceGroupName "AZ500LAB01"
    ```
  
  
1.  Use the following command to remove the Reader role assignment for the user at the subscription scope.

    ```
    Remove-AzRoleAssignment -SignInName bill@yourdomain.onmicrosoft.com -RoleDefinitionName "Reader" -Scope $subScope
    ```
 
1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):
  
    ```
    Remove-AzResourceGroup -Name "AZ500LAB01"
    ```

1.  Close the **Cloud Shell**.  


**Results**: You have now completed this lab.
