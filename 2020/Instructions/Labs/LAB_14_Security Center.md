---
lab:
    title: '14 - Azure Security Center'
    module: 'Module 04 - Manage security operations'
---

# Lab 14 - Azure Security Center

# Student lab manual

## Lab scenario
Azure Security Center is a unified infrastructure security management system that strengthens the security posture of your data centers, and provides advanced threat protection across your hybrid workloads in the cloud - whether they're in Azure or not - as well as on premises.

Keeping your resources safe is a joint effort between your cloud provider, Azure, and you, the customer. You have to make sure your workloads are secure as you move to the cloud, and at the same time, when you move to IaaS (infrastructure as a service) there is more customer responsibility than there was in PaaS (platform as a service), and SaaS (software as a service). Azure Security Center provides you the tools needed to harden your network, secure your services and make sure you're on top of your security posture.

Contoso would now like to look at the security posture of their proof of concept staring with the VM deployed as part of the Azure Monitor lab. They are also very interested to see how we can further protect our public facing and internal servers.

## Objectives

Contoso would like you to see the following achieved:
- Onboard Azure Security Center to the existing subscription so we can see our Security posture
- Apply endpoint protection to any hosts
- Apply Just in time VM access to any public facing hosts

+ Task 1: Onboard Security Center
+ Task 2: Review a host computer
+ Task 3: Configure endpoint security on a VM
+ Task 4: Enable Just in time VM Access

## Instructions

## Exercise 1: Onboard your Azure subscription to Security Center Standard


Azure Security Center provides unified security management and threat protection across your hybrid cloud workloads. While the Free tier offers limited security for your Azure resources only, the Standard tier extends these capabilities to on-premises and other clouds. Security Center Standard helps you find and fix security vulnerabilities, apply access and application controls to block malicious activity, detect threats using analytics and intelligence, and respond quickly when under attack. You can try Security Center Standard at no cost. To learn more, see the pricing page.

In this Exercise, you upgrade to the Standard tier for added security and install the Microsoft Monitoring Agent on your virtual machines to monitor for security vulnerabilities and threats.


### Task 1: Onboard Security Center


Security Center collects data from your Azure VMs and non-Azure computers to monitor for security vulnerabilities and threats. Data is collected using the Microsoft Monitoring Agent, which reads various security-related configurations and event logs from the machine and copies the data to your workspace for analysis. By default, Security Center will create a new workspace for you.

When automatic provisioning is enabled, Security Center installs the Microsoft Monitoring Agent on all supported Azure VMs and any new ones that are created. Automatic provisioning is strongly recommended.


To enable automatic provisioning of the Microsoft Monitoring Agent:

1.  In the Azure Portal, select **Security Center** from the Hub menu.

1.  On the **Getting started** blade click **Upgrade**.
     
1.  Under the Security Center main menu, select **Pricing & settings**.

1.  Click on the row of your subscription. Ensure **Standard** is selected and that all Resource types are **Enabled**. Click **Save** if it is not greyed out

1. Navigate back to **All Services** > **Security Center** > **Pricing and Settings** > Select **your subscription**

1.  In the **Data Collection** tab, set **Auto provisioning** to **On**. Click **Save**

1.  Under **Workspace configuration** select **Use another workspace** and choose the Log Analytics workspace the you created from Lab 13. Click **Save**, Click **Yes**

1. Click **Workflow Automation** > **+ Add Workflow automation**

1. Look through these settings. We can see that we can automate a response using Logic Apps for threat detecion alerts and for security center recommendations.

 With this new insight into your Azure VMs, Security Center can provide additional Recommendations related to system update status, OS security configurations, endpoint protection, as well as generate additional Security alerts.


### Task 2: Review a host computer

1.  In the previous lab we added a VM to a log analytics workspace which we have now configured to Security Center.

1.  Under **Resource Security Hygiene** click **Compute & apps**

1. Click on **VMs and Servers**. You should see the VM from the previous lab **myVM**.
       > Note it may take a up to 15 minutes before the vm appears

1. We can also add other VMs into Security Center using the same process.

### Task 3: Configure endpoint security on a VM

1. Before we start lets look at our Secure Score.

1. Under **Policy & Compliance** Click on **Secure Score**. Take note of your current secure score.

1.  Under **Resource Security Hygiene** click **Compute & apps**

1. Click on **Install endpoint protection solution on virtual machines**

1. **myVM** should be selected. Click on **Install on 1 VMs**

1. Select **Microsoft Antimalware** > Click **Create**

1. Accept the defaults and click **OK**. Wait for the Installation to complete. The **State** will show **In progress**. **Note** it should take only a few minutes to install the exrension. To verify you can Navigate to **Virtual Machines** click on **myVM** under **settings** > **Extensions** you should see IAASAntimalware with a ststus of Provisioning succeeded.

1. Once the Antimalware hass been provisioned successfully Security Center will rescan the VM ad we should see that issue disapear from our list or marked as green resolved and our secure score increase. (Note this may take some time to complete, you may want to wait till the end of the lab to double check)

### Task 4: Enable Just in time VM Access

1. Under **Advanced Cloud Defense** click **Just in time VM access**

1. You should find myVM under either **Recommended** or **No recommendation**. Again it may take some time for the scan to finish and the VM appear in **Recommended**. In the interest of time we will configure JIT from the VM pane.

1. Under **Virtual Machines** click on **myVM**

1. Click **Connect** > **RDP**

1. You should now notice a yellow bar at the top of the blade that says **To improve security, enable just-in-time access on this VM** click on the yellow bar.

1. Select **Enable just-in-time** and you should see that JIT has been enabled in Security Center.

1. Navigate to **Security Center** > Under **Advanced Cloud Defense** select **Just in time VM acess**

1. Select **Configured**

    > NOTE the VM may take a few minutes to appear

1. Click on the **elipsis** to the right of **myVM** and select **edit**

1. You can see that we have 3389 access for RDP, lets also provide extra ports for WinRM 5985 and 5986.

1. Click on **+ Add**

1. In port type **5985** leave the rest as default and click **OK**

1. Click on **+ Add**

1. In port type **5986** leave the rest as default and click **OK**

1. Click **save**

1. Lets now request access which will add a rule in our Network Security Group for us to be able to RDP to the VM.

1. Select the **myVM** and click **request access**

1. We can see multiple options here including choosing which ports to open the IP Range we want to allow and the Time Range.

1. Toggle 3389 to **On**. Type in **RDP access** into justification Leave the other settings at default and click **Open ports**

1. Under **Virtual Machines** click on **myVM**

1. Click **Connect** > **RDP**

1. We have already opened the ports do select **Public IP** address and click **Dowload RDP file anyway** 

1. Open the file and Connect
1.  When prompted for credentials enter **LocalAdmin** as the User and use the password **Pa55w.rd1234**

1. You should be able to connect to the VM suuccessfully.

1. Back in the Azure portal navigate to **Resource Groups** > **AZ500LAB131415** > **myNetworkSecurityGroup**.
1. Take note of the rules that have been created by Security Center.

1. Navigate to **Security Center**

1. Under **Advanced Cloud Defense** click **Just in time VM access**

1. To view all activity for this VM click on the **elipsis** to the right of **myVM** and select **View Activity Log**

**Results**: You have now completed this lab and can move onto the next lab in the series. Do not remove the resources from this lab as they are needed for the Azure Sentinel Lab.

