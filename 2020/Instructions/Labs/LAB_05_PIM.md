# Module 1: Lab 1 - Azure AD Privileged Identity Management


**Scenario**

In this lab, you'll learn how to use Azure Privileged Identity Management (PIM) to enable just-in-time administration and control the number of users who can perform privileged operations. You'll also learn about the different directory roles available as well as newer functionality that includes PIM being expanded to role assignments at the resource level. Lessons include:

- Getting Started with PIM
- PIM Security Wizard
- PIM for Directory Roles
- PIM for Role Resources

The Managing Identities course also covers Azure RBAC and Azure Active Directory. This content has been included here also to provide more context and foundation for the remainder of the course.


## Azure AD Privileged Identity Management

## Prerequisites

Ensure you have completed Lab 04: MFA, Conditional Acccess and AAD Identity Protection before proceeding with this lab as we need both the new AAD directory AdatumLab500-04 and the three users aaduser 1 2 and 3 for this lab.

## Exercise 1 - Assign Directory Roles

### Task 1:  Make a user eligible for a role


In the following task you will make  a user eligible for an Azure AD directory role.

1.  Sign in to Azure portal

1. In the Azure portal, set the **Directory + subscription** filter to **AdatumLab500-04** (the newly created Azure AD tenant from Lab 04.)

     > **Note**: The **Directory + subscription** filter is located to the right of the Cloud Shell icon in the toolbar of the Azure portal

1.  In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.

1.  Under **Manage** Select **Azure AD Roles**.


1.  Click **Roles**.



1.  Click **Add assignments** to open Add managed members.


1.  Click **Select a role**, and click **Billing Administrator**.


1.  Under **Select member(s)**, select **No member selected** and then click **aaduser2**, then **Select**.

1. Click **Next**

1. Untick **Permamnently eligible** and click **Assign**
 
1.  On the New assignment blade, click **Add**  to add the user to the role.


1. Select the **Billing Administrator** role.

1.  When the role is assigned, the user you selected will appear in the members list as **Eligible** for the role.

### Task 2:  Make a user eligible for a role with approval

 In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.

1.  Under **Manage** Select **Azure AD Roles**.


1.  Click **Settings**.

1. Select **Global Reader** > **Edit**

1. Notice all of the potential settings when activating a role including requiring MFA. 

1. Tick **Require approval to activate** 

1. Click **Select approvers** select **aaduser3** then click **select**

1. Click **Next:Assignment**

1. Notice all of the options as part of an assigment including the ability to permanently assign a role and the expiry period. Untick **Allow permanent eligible assignment**. Leave the rest as default and click **Next:Notification**

1. Leave this as default and click **Update**

### Task 3 Add a role with Approval required

1.  Under **Manage** Select **Azure AD Roles**.

1. Under **Manage** click **Roles**

1. Click **Add assignments** 


1.  Click **Select a role**, and click **Global Reader**.


1.  Under **Select member(s)**, select **No member selected** and then click **aaduser2**, then **Select**.

1. Click **Next**

1. Untick **Permamnently eligible** and click **Assign**
 
### Task 4 Allow Permanent assignment of a role

In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.

1.  Under **Manage** Select **Azure AD Roles** > **Assign Eligibility**.

1. Click **Add assignments** 

1.  Click **Select a role**, and click **Security Administrator**.


1.  Under **Select member(s)**, select **No member selected** and then click **aaduser2**, then **Select**.

1. Click **Next**

1. Leave **Permamnently eligible** ticked and click **Assign**


## Exercise 3 - Activate and Deactivate PIM Roles

### Task 1: Activate a role


When you need to take on an Azure AD directory role, you can request activation by using the **My roles** navigation option in PIM.

1.  Open an **In Private** browsing session and navigate to **`https://portal.azure.com`** and login as **aaduser2** using their UPN. example aaduser2@adatumlab50004.onmicrosoft.com

1.  In the Azure Portal, click **All services** and search for and select **Azure AD Privileged Identity Management**.

1. Under **tasks** click **My roles**

1. You should see three roles as **Eligible roles** for **aaduser2**. **Global Reader**, **Billing Administrator** and **Security Administrator**. Notice that **Security Administrator** can be assigned as a **Permanenet** role

1.  On the **Billing Administrator** role, to the right  click **Activate**.


1.  Click **Additional verification required.  Click to continue**. You only have to authenticate once per session. Run through the wizard to authenticate **aaduser2**.

 
1.  Once returned to the Azure Portal, enter an activation reason and click **Activate**.


By default, roles do not require approval unless configured explicitly in settings. 

 If the role does not require approval, it is activated and added to the list of active roles. If you want to use the role right away, follow the steps in the next section.

 If the role requires approval to activate, a notification will appear in the upper right corner of your browser informing you the request is pending approval.


### Task 2: Use a role immediately after activation


When you activate a role in PIM, it can take up to 10 minutes before you can access the desired administrative portal or perform functions within a specific administrative workload. 

1.  Click **Sign Out**.

1.  Log back in as **aaduser2**.

1. Navigate back to **Azure AD Privileged idnetity management** under **tasks** click **My Roles**

1. Select **Active roles**. You should see that **aaduser2** will have an Activated state for the **Billing Administrator** role. 

### Task 3: Deactivate a role


Once a role has been activated, it automatically deactivates when its time limit under **End time**(eligible duration) is reached.

If you complete your administrator tasks early, you can also deactivate a role manually in Azure AD Privileged Identity Management.

1.  From the Billing Administrator role click **Deactivate**.


1.  Click **Deactivate** again.


### Task 4: Cancel a pending request


If you do not require activation of a role that requires approval, you can cancel a pending request at any time.

1. Sign out and back in with your own account and ensure you are in the **ADATUMLAB500-04** directory

1.  **Open Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1. Click **Activate your role** 

1. From **Global Reader** Click **Activate**  

1. Type in a reason then click **Activate**. You should see that your request is awaiting approval

1.  Click **Pending requests**.

1.  You should see your request in Pending Requests. To remove the request tothe right click **Cancel**.

### Task 5 - Approve a pending request

Lets run through the same process except in this case we will approve the pending request

1.  **Open Azure AD Privileged Identity Management** as **aaduser2**.

1.  Click **Azure AD roles**.

1. Click **Activate your role** 

1. From **Global Reader** Click **Activate**  

1. Type in a reason then click **Activate**. You should see that your request is awaiting approval

1.  Lets also Activate our Permanent role. On **Security Administrator** click **Activate** 

1. Type in a reason then click **Activate**. The auto approval process should complete.

1. Select **Active roles**(You may need to sign out and back in). You should now have Security Administrator as a permanent Role and a pending request for the Global Reader role.

1. Click **Pending requests**.

1.  You should see your request in Pending Requests. 


### Task 6 - Approve Pending request

1.  Sign into the Azure portal as **aaduser3**.(Note you may need to reset the users password. Take note of the password for later use)

1.  **Open Azure AD Privileged Identity Management** 

1.  Click **Azure AD roles**.

1. Under **Tasks** Click **Approve Requests**

1. Under **Requests for role activations** click on the Global Reader role requests from user **aaduser2**.
1. Type in a justification and click **Approve**

1.  Sign into the Azure portal as **aaduser2**

1.  **Open Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1. Click **My roles** > **Active roles**. You should now have Global reader Activated.

## Exercise 4 - Create an Access Review

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
 
 
1.  Once the review has completed and has a status of Active, click on the **Global Admin Review**.(You may need to wait a couple of minutes and refresh the browser)

1.  Select **Results** and see the outcome of **Not reviewed**.



### Task 2: Approve or deny access


When you approve or deny access, you're just telling the reviewer whether you still use this role or not. Choose Approve if you want to stay in the role, or Deny if you don't need the access anymore. Your status won't change right away, until the reviewer applies the results. Follow these steps to find and complete the access review:


1.  In the PIM application,under **Start** select **Review access**. 

1.  Select the **Global Admin Review**.


1.  Unless you created the review, you appear as the only user in the review. Select the check mark next to your name. 

1. Type in a reason for approval then click **Approve**

1. **aaduser1** may still have access from previous labs. If so click on **aaduser1** then type in a reason and select **Deny**.

1.  Close the **Review Azure AD roles** blade.


### Task 3: Configure security alerts for Azure AD directory roles in PIM


You can customize some of the security alerts in PIM to work with your environment and security goals. Follow these steps to open the security alert settings:



1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Under Manage select **Alerts** then **Setting**.


1.  Click an alert name to see the settings for the preconfigured alerts.


 
## Exercise 6 - View audit history for Azure AD roles in PIM


You can use the Azure Active Directory (Azure AD) Privileged Identity Management (PIM) audit history to see all the role assignments and activations within the past 30 days for all privileged roles. If you want to see the full audit history of activity in your directory, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory security and activity reports](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/overview-reports).


### Task 1: View audit history


Follow these steps to view the audit history for Azure AD roles.


1.  Open **Azure AD Privileged Identity Management**.

1.  Click **Azure AD roles**.

1.  Under **Activity** Click **Resource audit**.

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


### Add Lab Cleanup