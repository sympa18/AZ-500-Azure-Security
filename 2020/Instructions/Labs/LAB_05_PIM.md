# Module 1: Lab 1 - Azure AD Privileged Identity Management


**Scenario**

In this lab, you'll learn how to use Azure Privileged Identity Management (PIM) to enable just-in-time administration and control the number of users who can perform privileged operations. You'll also learn about the different directory roles available as well as newer functionality that includes PIM being expanded to role assignments at the resource level. Lessons include:

- Getting Started with PIM
- PIM Security Wizard
- PIM for Directory Roles
- PIM for Role Resources

The Managing Identities course also covers Azure RBAC and Azure Active Directory. This content has been included here also to provide more context and foundation for the remainder of the course.


## Azure AD Privileged Identity Management

## Exercise 1 - Discover and Manage Azure Resources

### Task 1: Lab Setup


This lab requires creating a user that will be used for PIM.


1.  In the **Azure Portal** open the **Cloud Shell** in **PowerShell** mode. If prompted click **Create Storage**.

1. Run the following command to authenticate

    ```powershell
    Connect-AzureAD
    ```
    **Note**: If you close your Cloud Shell session you may be required to enter this command again throughout the labs.

1.  Run the following PowerShell Commands to create an AD user and password in your default domain

    ```powershell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```
    ```powershell
    $PasswordProfile.Password = "Pa55w.rd"
    ```
    ```powershell
    $domainObj = get-azureaddomain
    ```
    ```powershell
    $domain = $domainObj[0].name
    ```
    ```powershell
     New-AzureADUser -DisplayName "Isabella Simonsen" -PasswordProfile $PasswordProfile -UserPrincipalName "Isabella@$domain" -AccountEnabled $true -MailNickName "Isabella" -UsageLocation "US"
    ```
### Task 2:  Enable Azure AD Premium P2 trial and create a test user.

1.  In the Azure Portal, on the Hub menu click **Azure Active Directory**.

1.  Select **Licences** then **All Products**.

1.  Click **Try / Buy**.


1.  Click the drop down arrow and select **Activate** on the **Azure AD Premium P2** product.


**You may need to log out of the Azure portal and log in again for this to refresh**

### Task 3: Discover resources

1.  In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.


2.  Click **Azure resources**.



3.  Click **Discover resources** to launch the discovery experience.


4.  On the Discovery pane, use **Resource state filter** and **Select resource type** to filter the management groups or subscriptions you have write permission to. It's probably easiest to start with **All** initially.


5.  Add a checkmark next to your Azure subscription.
   
 
6.  Click **Manage resource** to start managing the selected resources.


7.  Click **Yes** when prompted.



    **Note**: You can only search for and select management group or subscription resources to manage using PIM. When you manage a management group or a subscription in PIM, you can also manage its child resources.


    **Note**: Once a management group or subscription is set to managed, it can't be unmanaged. This prevents another resource administrator from removing PIM settings.


## Exercise 2 - Assign Directory Roles

### Task 1:  Make a user eligible for a role


In the following task you will make  a user eligible for an Azure AD directory role.


1.  Sign in to Azure portal

1.  In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.

1.  Select **Azure AD Roles**.


1.  Click **Roles**.



1.  Click **Add member** to open Add managed members.


1.  Click **Select a role**, and click **Billing Administrator** and then click  **Select**.


1.  Click **Select a member**, select **Isabella** and then click **Select**.


1.  On the Membership settings blade, click **Save**.


1.  On the New assignment blade, click **Add**  to add the user to the role.


1. Select the **Billing Administrator** role.

1.  Review the added assignment.


1.  When the role is assigned, the user you selected will appear in the members list as **Eligible** for the role.



## Exercise 3 - Activate and Deactivate PIM Roles

### Task 1: Activate a role


When you need to take on an Azure AD directory role, you can request activation by using the **My roles** navigation option in PIM.

1.  Open an **In Private** browsing session and navigate to **`https://portal.azure.com`** and login as **Isabella** using her UPN. example Isabella@myaad.onmicrosoft.com with the password **Pa55w.rd**.  When prompted change Isabella's password.

1.  In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.


1.  Click **Azure AD roles**.



1.  Click **Quick start** and click **Activate your role**.



1.  On the Billing Administrator role, scroll to the right and click **Activate**.


1.  Click **Additional verification required.  Click to continue**. You only have to authenticate once per session. Run through the wizard to authenticate Isabella.

 
1.  Once returned to the Azure Portal, enter an activation reason and click **Activate**.


By default, roles do not require approval unless configured explicitly in settings. 

 If the role does not require approval, it is activated and added to the list of active roles. If you want to use the role right away, follow the steps in the next section.

 If the role requires approval to activate, a notification will appear in the upper right corner of your browser informing you the request is pending approval.


### Task 2: Use a role immediately after activation


When you activate a role in PIM, it can take up to 10 minutes before you can access the desired administrative portal or perform functions within a specific administrative workload. To force an update of your permissions, use the **Application access** page as described in the following steps.


1.  Click **Sign Out**.



1.  Log back in as Isabella.


### Task 3: Deactivate a role


Once a role has been activated, it automatically deactivates when its time limit (eligible duration) is reached.

If you complete your administrator tasks early, you can also deactivate a role manually in Azure AD Privileged Identity Management.



1.  Still signed in as **Isabella**, open Azure AD Privileged Identity Management.

1.  Click **Azure AD roles**.

1.  Click **My roles**.



1.  Click **Active roles** to see your list of active roles.



1.  Find the role you're done using and then click **Deactivate**.



1.  Click **Deactivate** again.



1.  Click **Yes** to confirm.


### Task 4: Cancel a pending request


If you do not require activation of a role that requires approval, you can cancel a pending request at any time.


1.  **Open Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Click **My requests**.

1.  For the role that you want to cancel, click the **Cancel** button.

**Note**: The cancel button in this task is greyed out as the request was approved.

When you click Cancel, the request will be cancelled. To activate the role again, you will have to submit a new request for activation.


## Exercise 4 - Directory Roles (General)

### Task 1: Start an access review for Azure AD directory roles in PIM


Role assignments become "stale" when users have privileged access that they don't need anymore. In order to reduce the risk associated with these stale role assignments, privileged role administrators or global administrators should regularly create access reviews to ask admins to review the roles that users have been given. This task covers the steps for starting an access review in Azure AD Privileged Identity Management (PIM).


1.  Return back to the browser that is logged in as your Global Admin Account.

1.  From the PIM application main page click **Azure AD Roles** under the **Manage** section click **Access reviews** and click > **New**.


1.  Enter the following details and click **Start**:

      - Review name:  **Global Admin Review**
      - Start Date:  **Today's Date** 
      - Frequency: **One time**
      - End Date:  **End of next month**
      - Review role membership:  **Global Administrator**
      - Reviewers:  **Select your account**
 
 

 
1.  Once the review has completed and has a status of Active, click on the **Global Admin Review**.

1.  Select **Results** and see the outcome of **Not reviewed**.



### Task 2: Approve or deny access


When you approve or deny access, you're just telling the reviewer whether you still use this role or not. Choose Approve if you want to stay in the role, or Deny if you don't need the access anymore. Your status won't change right away, until the reviewer applies the results. Follow these steps to find and complete the access review:


1.  In the PIM application, select **Review access**. 

2.  Select the **Global Admin Review**.



3.  Unless you created the review, you appear as the only user in the review. Select the check mark next to your name.



5.  Close the **Review Azure AD roles** blade.

### Task 3: Complete an access review for Azure AD directory roles in PIM


Privileged role administrators can review privileged access once an access review has been started. Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users to review their access. If a user did not get an email, you can send them the instructions in how to perform an access review.

After the access review period is over, or all the users have finished their self-review, follow the steps in this task  to manage the review and see the results.



1.  Go to the Azure portal and select the **Azure AD Privileged Identity Management**.

1.  Select **Azure AD Roles**.

2.  Select the **Access reviews**.


3.  Select the Global Admin Review.


1.  Review the blade.



### Task 4: Configure security alerts for Azure AD directory roles in PIM


You can customize some of the security alerts in PIM to work with your environment and security goals. Follow these steps to open the security alert settings:



1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Click **Settings** and then **Alerts**.


1.  Click an alert name to see the settings for the preconfigured alerts.


## Exercise 5 - PIM Resource Workflows

### Task 1:  Configure the Global Administrator role to require approval.

1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Click **Settings** 

1.  Click **Roles** and select **Global Administrator**.

1.  Scroll down and select **Require Approval** and  select your account as the approver then click **Update**.



### Task 2: Enable Isabella for Global Administrator privileges.

1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Click the **Quick Start** and select **Assign eligibility**.

     
1.  Select **Global Administrator** and click **Edit**.


1.  Select **+ Add Member** and select **Isabella**.


1.  Open an in Private Browsing session and login to portal.azure.com as Isabella.

1.  Open **Azure AD Privileged Identity Management**.

1.  Select **My Roles**.

     
1.  **Activate** the Global Administrator Role.

     
1.  Verify Isabella's identity using the wizard.

     
1.  Once you are returned to the **Activate - Global Administrator** blade, enter the justification **I need to carry out some administrative tasks** and click **Activate**.

     

### Task 3: Approve or deny requests for Azure resource roles in PIM


With Azure AD Privileged Identity Management (PIM), you can configure roles to require approval for activation, and choose one or multiple users or groups as delegated approvers. Follow the steps in this article to approve or deny requests for Azure resource roles.


#### View pending requests


As a delegated approver, you'll receive an email notification when an Azure resource role request is pending your approval. You can view these pending requests in PIM.


1.  Switch back to the browser you are signed in with your Global Administrative account.

1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Approve requests**.

    
    **Note**: You may need to refresh your browser to see the request.

1.  Click the request from Isabella and enter the justification **Granted for this task** and click **Approve**.

   


1.  Switch back to the In Private Browsing session where Isabella is signed in and click My Roles and click the Active roles tab.  Note the status.

   
## Exercise 6 - View audit history for Azure AD roles in PIM


You can use the Azure Active Directory (Azure AD) Privileged Identity Management (PIM) audit history to see all the role assignments and activations within the past 30 days for all privileged roles. If you want to see the full audit history of activity in your directory, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory security and activity reports](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/overview-reports).


### Task 1: View audit history


Follow these steps to view the audit history for Azure AD roles.


1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Click **Resource audit**.

    Depending on your audit history, a column chart is displayed along with the total activations, max activations per day, and average activations per day.

    At the bottom of the page, a table is displayed with information about each action in the available audit history. The columns have the following meanings:

    | Column | Description |
    | --- | --- |
    | Time | When the action occurred. |
    | Requestor | User who requested the role activation or change. If the value is **Azure System**, check the Azure audit history for more information. |
    | Action | Actions taken by the requestor. Actions can include Assign, Unassign, Activate, Deactivate, or AddedOutsidePIM. |
    | Member | User who is activating or assigned to a role. |
    | Role | Role assigned or activated by the user. |
    | Reasoning | Text that was entered into the reason field during activation. |
    | Expiration | When an activated role expires. Applies only to eligible role assignments. |

1.  To sort the audit history, click the **Time**, **Action**, and **Role** buttons.

### Task 2: Filter audit history

1.  At the top of the audit history page, use the filter options to filter the results.

**Results**: You have now completed this lab.



