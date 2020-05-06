---
lab:
    title: '0x - xxx'
    module: 'Module 0x - xxx'
---

# Lab 0x - xxx

# Student lab manual

## Lab scenario

blah 
blah 
blah

## Objectives

In this lab, you will:

+ Task 1: xxx
+ Task 2: xxx
+ Task 3: xxx
+ Task 4: xxx 

## Instructions

### Exercise 1

#### Task 1: xxx






# Module 3: Classify a SQL Database 

## Exercise 1: Classify your SQL Database

### Task 1: Lab Setup

1.  Click **Create a resource** and type in **template** and choose **Template deployment (deploy using custom templates)** > Click **Create**.

1. Select **Build your own template in the editor** > Click **Load File**

1. Select the following file **2020\Allfiles\Labs\LAB_11\azuredeploy.json** > Click **Open** > Click **Save**
1. Click **Edit parameters**  > Click **Load File**

1. Select the following file **2020\Allfiles\Labs\LAB_11\azuredeploy.parameters.json** > Click **Open** > Click **Save**



1.  Under **Resource group** click create new and use the default name "**AZ500LAB11**" > Location **East US** or a location near you.

1.  You can use the default populated **SQL server name** with a **unique** string added to make a **globaly unique** name

1. Select **I agree to the terms and conditions stated above** > Click **Purchase**
warning
**Note**: You must wait for the SQL database with the test data to deploy

1.  Select **Resource Groups** > **AZ500LAB11**

1.  Click **az500labserver(yourname)**

1.  Under **Security** click **Advanced Data Security**.

1.  Select **ON** under **Advanced Data Security**

1. Click **Advanced Threat Protection Types**. Read through the threat types. Click **OK**

1.  **Click Save**

1.  Return to the **AZ500LAB11** resource group

1.  **Select** the **SQL databse AZ500LabDb (az500labserver/AZ500LabDb)**

1.  Under **Security** Click **Advanced Data Security** 

1.  Click the bar at the top **Complete Advanced Data Security setup by selecting a storage account for Vulnerability Assesment**

1.  Make sure the option for **Advanced Data Security** is set to **On** 

1.  Click **storage account**

1.  Click **create new**

1.  For the name use **mod3lab11yourname** replacing **yourname** with your name to make it unique but memorable


1.  Click **OK**

1.  **Select** the **save** option

 > If you receive an error about the storage container path. The storage account may not have completely deployed as of yet. Wait a few minutes and simply reselect the same storage account. Then click Save again.

1.  **Return** to the previous **Advanced Data Security** pane

1.  **Select** the **Data discovery** and **classification** card.

## Exercise 2: Begin Classification

1.  To begin classifying your data, select the **Classification tab** at the top of the window.

1.  The classification engine scans your database for columns containing potentially sensitive data and provides a list of recommended column classifications.

1.  To view and apply classification recommendations:

    - View the list. To view the list of recommended column classifications, select the **recommendations** panel at the top of the window. (**Blue Bar**)   

1.  Click **Select all** in the top left to select all reccomendations

1.  Click **Accept Selected Reccomendations**

1.  To complete your classification and persistently label (**tag**) the database columns with the new classification metadata, select **Save**. 

1.  Upon returning to the **Advanced Data Security** pane you will be able to see in the overview pane for Data classification an overview of the data that was classified.


**Results**: You have now classified information in a SQL database on Azure for GPDR and data protection compliance



### Exercise 3 : Enable auditing on your database

1.  Select your resource group **AZ500LAB11** 

2.  **Select** the SQL server **az500labserver(yourname)**

3.  **Under Security**, select **Auditing**

4.  **Switch Auditing** to **ON**.

5.  Select **storage** as the location to send the audit logs to

6.  **Click configure**

7.  Select **Your Subscription**

8.  Click **storage account** > **Create New**.

9.  Name the storage account **mod3lab11audityourname** ensuring you replace **yourname** with a unique name using lowercase letters

10. **Click OK**.

11. Change the retention days to **5** and click **OK** 

12. Click **Save** to save the **auditing configuration**

## Exercise 2: Review audit logs

### Task 1: Review audit logs on the SQL DB.

1.  To review audit logs for a database return to the resource group created in the lab setup

1.  Click **AZ500LabDb (az500labserver2/AZ500LabDb)** to select your test database

1.  **Under Security**, select **Auditing**
  
    **Note**: The Auditing looks off here but it is set on the underlying server level so it is turned on for this database


1.  Click **View Audit Logs**.

  **Note**: Here you will review the output of the audit logs of the database including any attempted SQL injections. Since this is a test database created recently, there will be minimal audits if any in the log at the current time.

  If server auditing is enabled, the database-configured audit will exist side-by-side with the server audit.
Notice that you can select for audit logs to be written to an Azure storage account, to a Log Analytics workspace for consumption by Azure Monitor logs, or to Event Hub for consumption using an event hub. You can configure any combination of these options, and audit logs will be written to each.



**Results**: You have now setup up auditing on a SQL database and reviewed where to view the auditing output

### Remove resources.

1. Open Cloud Shell in Powershell

1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):(Use the Resource Group names that werecreated in your lab)
    ```
    Remove-AzResourceGroup -Name "AZ500LAB11"
    ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Result**: In this exercise, you removed the resources used in this lab.
