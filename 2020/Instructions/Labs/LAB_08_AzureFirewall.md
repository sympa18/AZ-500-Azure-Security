---
lab:
    title: '08 - Azure Firewall'
    module: 'Module 02 - Implement platform protection'
---

# Lab 08 - Azure Firewall


## Lab Scenario

Controlling outbound network access is an important part of an overall network security plan. For example, you may want to limit access to web sites, or the outbound IP addresses and ports that can be accessed.

One way you can control outbound network access from an Azure subnet is with Azure Firewall. With Azure Firewall, you can configure:

* Application rules that define fully qualified domain names (FQDNs) that can be accessed from a subnet.
* Network rules that define source address, protocol, destination port, and destination address.

Network traffic is subjected to the configured firewall rules when you route your network traffic to the firewall as the subnet default gateway.

## Objectives

In this lab, you will:

+ Task 1: Deploy a lab environment
+ Task 2: Deploy an Azure Firewall
+ Task 3: Configure Azure Firewall
+ Task 4: Tesy Azure Firewall

## Instructions

## Exercise 1: Deploy an Azure Firewall

### Task 1: Lab Setup


1. Navigate to **Create a resource** > type in **template** and select **Template deployment (deploy using custom templates)** > Select **Create**

1. Click on **Build your own template in the editor** > **Load File** and select the folllowing file **2020\Allfiles\Labs\LAB_08\template.json** > Click **Open** > **Save**

1.  Click **Create new** under the Resource Group and use the resource group name of **AZ500LAB08**  

1.  Select the loction of **East US**  

1.  Leave all the other fields as the pre-populated defaults

1.  Select the I agree.... check box and click **Purchase** and wait for the deployment to complete.

    This lab setup template will set up the following resources for the lab

 |Name     |Type     | Location|
 |---------|---------|---------|
azureFirewalls-ip|	Public IP address|	East US	
Firewall-route |	Route table|	East US	
Srv-Jump|	Virtual machine|	East US	
Srv-Jump_OsDisk|	Disk|	East US	
srv-jump121	|Network interface|	East US	
Srv-Jump-nsg|	Network security group|	East US	
Srv-Jump-PIP|	Public IP address|	East US	
Srv-Work|	Virtual machine|	East US	
Srv-Work_OsDisk_1 |	Disk|	East US	
srv-work267|	Network interface|	East US	
Srv-Work-nsg|	Network security group|	East US	
Test-FW-VN|	Virtual network|	East US


### Task 2: Deploy the firewall


In this task you will deploy the Azure firewall into the VNet.


1.  In the Azure portal, click **All services** and search for and select **Firewalls**.


1.  On the **Firewalls** blade click **Create firewall**. 

1.  On the **Create a Firewall** blade, use the following table to configure the firewall:

   |Setting  |Value  |
   |---------|---------|
   |Subscription     |_your subscription_|
   |Resource group     |**Use existing**: AZ500LAB08 |
   |Name     |Test-FW01|
   |Location     |East US|
   |Choose a virtual network     |**Use existing**: Test-FW-VN|
   |Public IP address     |**Add new**. **TEST-FW-PIP** The Public IP address must be the Standard SKU type.|

1.  Click **Review + create**.
1.  Review the summary, and then click **Create** to create the firewall.

    > This will take a few minutes to deploy.

1.  After the deployment completes, go to the **AZ500LAB08** resource group, and click the **Test-FW01** firewall.

1.  Make a note of the **Private IP** address. You'll use it later when you create the default route.

### Task 3: Create a default route

For the **Workload-SN** subnet, configure the outbound default route to go through the firewall.


1.  From the Azure portal home page, click **All services**.
1.  Under **Networking**, click **Route tables**.
1.  Click **Add**.
1.  For **Name**, type **Firewall-route**.
1.  For **Subscription**, select your subscription.
1.  For **Resource group**, select **Use existing**, and select **AZ500LAB08**.
1.  For **Location**, select **East US**.
1.  Click **Create**.
1.  Click **Refresh**,(This may take a minute or two to appear) and then click the **Firewall-route** route table.
1.  Click **Subnets** > **Associate**.
1.  Click **Virtual network** > **Test-FW-VN**.
1.  For **Subnet**, click **Workload-SN**. Make sure that you select only the **Workload-SN** subnet for this route, otherwise your firewall won't work correctly.

1.  Click **OK**.
1.  Click **Routes** > **Add**.
1.  For **Route name**, type **FW-DG**.
1.  For **Address prefix**, type **0.0.0.0/0**
1.  For **Next hop type**, select **Virtual appliance**.

        Azure Firewall is actually a managed service, but virtual appliance works in this situation.

1.  For **Next hop address**, type the private IP address for the firewall that you noted previously.
1.  Click **OK**.

### Task 4: Configure an application rule


In this task you will create an application rule that allows outbound access to `www.bing.com`.


1.  Open the **AZ500LAB08** resource group and click the **Test-FW01** firewall.

1.  On the **Test-FW01** page, under **Settings** section, click **Rules**.
1.  Click the **Application rule collection** tab.
1.  Click **Add application rule collection**.
1.  For **Name**, type **App-Coll01**.
1.  For **Priority**, type **200**.
1.  For **Action**, select **Allow**.
1.  Under **Rules**, **Target FQDNs**, for **Name**, type **AllowGH**.
1.  For **Source Addresses**, type **10.0.2.0/24**.
1.  For **Protocol:port**, type **http, https**.
1.  For **Target FQDNS**, type **www.bing.com**
1.  Click **Add**.

 Azure Firewall includes a built-in rule collection for infrastructure FQDNs that are allowed by default. These FQDNs are specific for the platform and can't be used for other purposes. 

### Task 5: Configure a network rule


In this task you will create a network rule that allows outbound access to two IP addresses on port 53 (DNS).


1.  Click the **Network rule collection** tab.
1.  Click **Add network rule collection**.
1.  For **Name**, type **Net-Coll01**.
1.  For **Priority**, type **200**.
1.  For **Action**, select **Allow**.

1.  Under **Rules** in the **IP Addresses** section, for **Name**, type **AllowDNS**.
1.  For **Protocol**, select **UDP**.
1.  For **Source Addresses**, type **10.0.2.0/24**.
1.  For Destination address, type **209.244.0.3,209.244.0.4**
1.  For **Destination Ports**, type **53**.
1.  Click **Add**.

### Task 6: Change the primary and secondary DNS address for the **Srv-Work** network interface


For testing purposes in this tutorial, you configure the primary and secondary DNS addresses. This isn't a general Azure Firewall requirement.


1.  From the Azure portal, open the **AZ500LAB08** resource group.

1.  Click the network interface for the **Srv-Work** virtual machine.

1.  Under **Settings**, click **Networking**.

1.  Select the NIC

1.  Under **DNS servers**, click **Custom**.

1.  Type **209.244.0.3** in the **Add DNS server** text box, and **209.244.0.4** in the next text box.

1.  Click **Save**. Wait for the network interface update to complete.

1.  Restart the **Srv-Work** virtual machine.

### Task 7: Test the firewall


In this task you will test the firewall to confirm that it works as expected.


1.  From the Azure portal, review the network settings for the **Srv-Work** virtual machine and note the private IP address.

1.  Connect to the **Srv-Jump** virtual machine using RDP using the following credentials. 

    -	**Username**: localadmin
    -	**Password**: Pa55w.rd1234

1. From ***Srv-Jump** open a remote desktop connection (From the command prompt type **mstsc** and press enter) to the **Srv-Work** private IP address using the following credentials.

    -	**Username**: localadmin
    -	**Password**: Pa55w.rd1234

1.  Open Internet Explorer and browse to **`https://www.bing.com`**

1.  Click **OK** > **Close** on the security alerts.

   You should see the bing home page.

1.  Browse to **`http://www.microsoft.com/`**

       - You should be blocked by the firewall.
       - So now you've verified that the firewall rules are working:
          - You can browse to the one allowed FQDN, but not to any others.
          - You can resolve DNS names using the configured external DNS server.


### Task x: Remove resources.

1. Open Cloud Shell in Powershell

1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter)

    ```
    Remove-AzResourceGroup -Name "AZ500LAB08"
    ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Result**: In this exercise, you removed the resources used in this lab.

**Results**: You have now completed this lab.


