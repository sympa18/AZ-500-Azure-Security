---
lab:
    title: '01 - Azure Monitor'
    module: 'Module 04 - Manage security operations'
---

# Lab 01 - Azure Monitor

# Student lab manual

## Lab scenario


Azure Monitor maximizes the availability and performance of your applications and services by delivering a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. It helps you understand how your applications are performing and proactively identifies issues affecting them and the resources they depend on.

Contoso would like to see a proof of concept for their secuirity team. They would like to see Monitoring, Security Posture of the subscription and what Azure has to offer in terms of a SIEM and a SOAR. They initially want to start with monitoring capabilities of Azure.

## Objectives

Contoso would like to see the capabilities of Azure Monitor and would like the following tasks completed.
- 
+ Task 1: Deploy an Azure VM to monitor
+ Task 2: Create a workspace
+ Task 3: Enable the Log Analytics VM Extension
+ Task 4: Collect event and performance of a Windows VM
+ Task 5: View data collected

## Instructions

## Exercise 1: Collect data from an Azure virtual machine with Azure Monitor


Azure Monitor can collect data directly from your Azure virtual machines into a Log Analytics workspace for detailed analysis and correlation. Installing the Log Analytics VM extension for Windows and Linux allows Azure Monitor to collect data from your Azure VMs. This exercise shows you how to configure and collect data from your Azure Linux or Windows VMs using the VM extension with a few easy steps.  


### Task 1: Deploy an Azure VM to monitor.

1.  Open the Azure Cloud Shell and run the following two commands to create a Resource Group and Azure VM that you will use to monitor:

    ```
    New-AzResourceGroup -Name AZ500LAB131415 -Location EastUS
    ```

    ```
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location "East  US" -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress"     -OpenPorts 80,3389
    ```

1.  When prompted for credentials enter **LocalAdmin** as the User and use the password **Pa55w.rd1234**

### Task 2: Create a workspace

1.  In the Azure portal, select **All services**. In the list of resources, type **Log Analytics**. As you begin typing, the list filters based on your input. Select **Log Analytics workspaces**.


2.  Select **Add**, and then select choices for the following items:

       * Select a **Subscription** to link to by selecting from the drop-down list if the default selected is not appropriate.
       * For **Resource Group**, select **AZ500LAB131415** which is the Resource Group that contains the VM you created in Task 1.
       * Provide a unique name for the new **Log Analytics workspace**, such as *myWorkspaceDemoyourname*.  
       * Select the **EastUS** as the location. 
       * Leave the pricing Tier as **Per Gb (2018)**
  
3.  Click **Review + Create** > **Create**


### Task 3: Enable the Log Analytics VM Extension


For Windows and Linux virtual machines already deployed in Azure, you install the Log Analytics agent with the Log Analytics VM Extension. Using the extension simplifies the installation process and automatically configures the agent to send data to the Log Analytics workspace that you specify. The agent is also upgraded automatically when a newer version is released, ensuring that you have the latest features and fixes. Before proceeding, verify the VM is running otherwise the process will fail to complete successfully. 
 

1.  In the Azure portal, select **All services** found in the upper left-hand corner. In the list of resources, type **Log Analytics**. As you begin typing, the list filters based on your input. Select **Log Analytics workspaces**.

2.  In your list of Log Analytics workspaces, select **myWorkspaceDemo** created earlier.

    **Note**: The name of your workspace may be different to **myWorkspaceDemo**.


3.  On the left-hand menu, under **Workspace Data Sources**, select **Virtual machines**.  

4.  In the list of **Virtual machines**, select a virtual machine you want to install the agent on. Notice that the **Log Analytics connection status** for the VM indicates that it is **Not connected**.

5.  In the details for your virtual machine, select **Connect**. The agent is automatically installed and configured for your Log Analytics workspace. This process takes a few minutes, during which time the **Status** shows **Connecting**.

6.  After you install and connect the agent, the **Log Analytics connection status** will be updated with **This workspace**.

### Task 4: Collect event and performance of a Windows VM.


Azure Monitor can collect events from the Windows event logs or Linux Syslog and performance counters that you specify for longer term analysis and reporting, and take action when a particular condition is detected. Follow these steps to configure collection of events from the Windows system log and Linux Syslog, and several common performance counters to start with.  


1.  On the Log Analytics workspaces blade, Under **settings** select **Advanced settings**.

1.  Select **Data**, and then select **Windows Event Logs**.

1.  You add an event log by typing in the name of the log.  Type **System** and then select the plus sign **+**.

1.  In the table, check the severities **Error** and **Warning**.

1.  Select **Save** at the top of the page to save the configuration. Click **OK**

1.  Select **Windows Performance Counters** to enable collection of performance counters on a Windows computer.

1.  When you first configure Windows Performance counters for a new Log Analytics workspace, you are given the option to quickly create several common counters. They are listed with a checkbox next to each.

1. Select **Add the selected performance counters**.  They are added and preset with a ten second collection sample interval.
  
1.  Select **Save** at the top of the page to save the configuration. Click **OK**


### Task 5: View data collected


Now that you have enabled data collection, lets run a simple log search example to see some data from the target VMs.  


1.  In the selected workspace, from the left-hand pane, under **General** select **Logs**.

1.  Click **Get started**.  

1. On the Logs query page, type `Perf` in the query editor and select **Run**.


       > Note since the VM has only being ran for a few minutes you may need to wait to get some data returned.

       > Please leave the resources in this lab for the following labs on Azure Security Center and Azure Sentinel

**Results**: In this lab, you learned how to monitor resources with Azure Monitor. Do not remove the resources from this lab as they are needed for both the Azure Security Center lab and the Azure Sentinel lab.

