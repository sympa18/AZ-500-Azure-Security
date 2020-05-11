---
lab:
    title: '03 - Protecting Azure Resources with Resource Manager Locks'
    module: 'Module 01 - Manage Identity and access'
---

# Lab 03 - Protecting Azure Resources with Resource Manager Locks

# Student lab manual

## Lab scenario
Resource Manager Locks provide a way for administrators to lock down Azure resources to prevent deletion or changing of a resource. These locks sit outside of the Role Based Access Controls (RBAC) hierarchy and when applied will place the restriction on the resource for all users. These are very useful when you have an important resource in your subscription which users should not be able to delete or change and can help prevent accidental and malicious changes or deletion.

There are two types of resource locks that can be applied:

 - **CanNotDelete** - This prevents anyone from deleting a resource whilst the lock is in place, however they may make changes to it.
 - **ReadOnly** - As the name suggests, it makes the resource read only, so no changes can be made and it cannot be deleted.
Resource locks can be applied to subscriptions, resource groups or individual resources as required. When you lock Subscription, all resources in that subscription (including ones added later) inherit the same lock. Once applied, these locks impact all users regardless of their roles. If it becomes necessary to delete or change a resource with a lock in place, then the lock will need to be removed before this can occur.

**Permissions**

Permission to set and remove locks requires access to one of the following RBAC permissions:

- `Microsoft.Authorization/*`
- `Microsoft.Authorization/locks/*`

By default, these actions are only available on the Owner and User Access Administrator built in RBAC Roles, however you can add them to custom roles as required. As mentioned, users with these roles are still subject to the locks, but obviously they can remove them if required. Creation and deletion of a lock is tracked in the Azure Activity log.

Locks can be created both at the time of creation of a resource inside an ARM template, or later using the portal or PowerShell.

The best way to ensure that locks are in place and protecting your resources is to create them at run time and configure them in your ARM templates. Locks are top level ARM resources; they do not sit underneath the resource being locked. They refer to the resource being locked, so this must exist first. 

Contoso has identified multiple resources in production that need protection from either accidental deletion or configuration changes. 

Contoso have asked if you can create a prrof of concept in Developent that will prove that you can do two thing. 
1 - Stop anybody from making changes to an existing resource if required by the business.
2 - Allow resource configuration changes but dissalow deletion of a resource if required by the business 

### Objectives
Create a resource and test out the impact of both a ReadOnly lock and a Delete lock.
- Create a ReadOnly lock
- Verify that you cannot make changes to/delete the resource
- Change the lock to Delete
- Verify that you can make changes tothe resource but canot delete the resource


## Exercise 1: Create a ReadOnly Lock

### Task 1: Adding a Lock (Portal)

1.  Open the Cloud Shell (PowerShell) and run the following commands to create a Resource Group and Storage Account.

     ```
    New-AzResourceGroup -Name AZ500LAB03 -Location 'EastUS'
     ```
    
     ```
    New-AzStorageAccount -ResourceGroupName AZ500LAB03 -Name (Get-Random -Maximum 999999999999999) -Location  EastUS -SkuName Standard_LRS -Kind StorageV2 
     ```

1.  Once the Storage account has been created close the **Cloud Shell**

1. click on the **portal menu** in the top left corner

1. Select **Resource Groups** then the **AZ500LAB03** resource group

1. Select the Storage Account. 

1. In the main blade under **Settings**, click the "Locks" icon

1.  Click **Add**

1.  Under **Lock name** type **ReadOnly Lock** 

1. Under Lock type choose **Read-only** then select **OK**

1.  The resource is now protected from acidental deleation and modification.

### Task 2: Verify that you cannot make changes to/delete the resource

1. Under **Settings** in the main stoarge account blade choose **Configuration**

1. We want to chane secure transfer to support Https for custom domain names

1. Under **Secure transfer required** click **Disabled** then **Save**

1. You should have receoved an error in the blad that says **Update failed**

1. You should also have received an error under the **bell notification icon** in the top right hand corner stating something similar to the following which tells you the spcific lock and the locks scope

**"Failed to update storage account 'xxxxxxxx'. Error: The scope 'xxxxxxxx' cannot perform write operation because following scope(s) are locked: '/subscriptions/xxxxx-xxx-xxxx-xxxx-xxxxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Please remove the lock and try again"**

1. Click the **Overview** icon. (Discard any changes from the configuration blade)

1. Click **Delete** and type in the name of the storage account to confirm then click on the **Delete**

1. You should have received an error under the **bell notification icon** in the top right hand corner stating something similar to the following which tells you the spcific lock and the locks scope

**"Failed to delete storage account 'xxxxxxx'. Error: The scope 'xxxxxxx' cannot perform delete operation because following scope(s) are locked: '/subscriptions/xxxx-xxxx-xxxx-xxxx-xxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Please remove the lock and try again."**

1. We have noew verfied that a ReadOnly lock will stop accidental deletion and modification of a resource.

### Task 3: Change the lock to Delete

1. If you are not already there go back to your Storage account then from under **settings**select the **locks** icon. 

1. Click on the **Delete** icon from the **ReadOnly Lock** you created earlier

1. Click **Add**

1.  Under **Lock name** type **Delete Lock** 

1. Under Lock type choose **Delete** then select **OK**

1.  The resource is now protected from accidental deleation and modification.


### Task 4: Verify that you can make changes to the resource but canot delete the resource

1. Under **Settings** in the main stoarge account blade choose **Configuration**

1. We want to chane secure transfer to support Https for custom domain names

1. Under **Secure transfer required** click **Disabled** then **Save**

1. You should have succesfully updated the storage account

1. Click the **Overview** icon.

1. Click **Delete** 

1. You should have received a warning stating the following 

**'xxxxxx' can't be deleted because this resource or its parent has a delete lock. Locks must be removed before this resource can be deleted. Learn more"**

1. We have now verfied that a **Delete** lock will allow configuration changes and stop accidental deletion and modification of a resource.

By using Resource Locks you can put in place an extra line of defense against accidental or malicious changing and/or deletion of your most important resources. It's not perfect, as your administrators can still remove these locks, but doing so requires a conscious effort, as the only purpose for removing a lock is to circumvent it. As these locks sit outside of RBAC you can apply them and be sure that they are impacting all your users, regardless of what roles or custom permissions you may have granted them.

### Task 3: Delete Resources

1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):
  
  1.  To remove the lock use the following 3 commands. After the 3rd command appears you may need to press **enter** .(When prompted to confirm press Y and press enter):

     ```
    $Storageaccountname = (get-azstorageaccount -ResourceGroupName AZ500LAB03).StorageAccountName
    $LockName = (Get-AzResourceLock -ResourceGroupName AZ500LAB03 -ResourceName $Storageaccountname -ResourceType Microsoft.Storage/storageAccounts).Name
    Remove-AzResourceLock -LockName $LockName -ResourceName $Storageaccountname  -ResourceGroupName AZ500LAB03 -ResourceType Microsoft.Storage/storageAccounts
     ```
1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):
    ```
    Remove-AzResourceGroup -Name "AZ500LAB03"
    ```

1.  Close the **Cloud Shell**. 

**Results**: You have now completed this lab.

