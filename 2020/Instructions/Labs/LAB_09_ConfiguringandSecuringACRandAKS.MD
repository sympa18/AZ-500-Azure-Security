---
lab:
    title: '09 - Configuring and Securing ACR and AKS'
    module: 'Module 02 - Implement platform protection'
---

# Lab 09 - Configuring and Securing ACR and AKS

# Student lab manual

## Lab scenario

Contoso are looking at using containers as a technology. What they are unsure of is how they can store container images and how they can restrict access to those images. They would also like to look at a solution that can orchestrate many application deployments with the ability to produce both internal and external facing solutions. In addition they would like a solution that can be integrated into Azure Active Directory for access to applications.

Contoso will be deploying a proof of concept with  Azure container Registry to store their container images and they want to deply AKS as their managed orchstration service. 

## Objectives

In this lab, you will:

+ Task 1: Deploy an Azure Container Registry
+ Task 2: Build a container and push it to Azure Container Registry
+ Task 3: Create an Azure Kubernetes Service
+ Task 4: Give AKS permission to access ACR 
+ Task 5: Deploy an external facing container from ACR to AKS
+ Task 6: Verify the you can access the container externally
+ Task 7: Deploy an internal facing container from ACR to AKS
+ Task 8: Verify the you can access the container internally

## Instructions

### Exercise 1

#### Task 1: Deploy an Azure Container Registry

1. Open **Cloud Shell** and select **Bash**

1. Run the following command to create a new resource group

    ```
    az group create --name AZ500LAB09 --location eastus
    ```
1. Now we need to create a brand new Azure Container Registry. Type in the following command into Cloud Shell replacing  with a globally unique name.(Take note of this for use later in the lab)

    ```
    az acr create --resource-group AZ500LAB09 --name <ACRuniquename> --sku Basic
    ```
1. Once that has completed, in the portal navigate to **Resource Groups** > **AZ500LAB09**. You should see the new container registry that you created. (It may take a minute or two to appear. Refresh your window)

### Task 2: Build a container and push it to Azure Container Registry

1. Let's now build and deploy an image to ACR. 
1. First lets create a Dockerfile to deploy Nginx.
1. Open **Command Shell** in **Bash**

    ```
    echo FROM nginx > Dockerfile
    ```
1. Now let's build an image off that docker file and push the image to our container registry

    ```
    az acr build --image sample/nginx:v1 --registry <ACRuniquename> --file Dockerfile .
    ```
1. Once the build completes successfully navigate back to **Resource Groups** > **AZ500LAB09** > Click on your container registry
1. Under **Services** select **Repositories**. You should now see a new container image named sample/nginx.

1. Click on the image and you will see we have a **v1** tag on the image to identify the version. 
1. Click on **V1** and you will now see all of the Manifest information for that container image including the sha256 Digest. 

### Task 3: Create an Azure Kubernetes Service
1. Navigate to **Create a new Resource**, type in **Kubernetes Service** and select the drop down option.

1. Click **Create**

1. In the **Basics** tab choose the folloing options

   |Setting  |Value  |
   |---------|---------|
   |Subscription     |_your subscription_|
   |Resource group     |**Use existing**: AZ500LAB09 |
   |Kubernetes cluster name     |MyKubernetesCluster|
   |Region     |East US|
   |Kubernetes version | leave at default |
   |Node size     | leave at default |
   |Node count     |1|

1. Click **Next: Node pools**
   |Setting  |Value  |
   |---------|---------|
   |Virtual nodes     |leave at default: disabled|
   |VM scale sets     |disabled |

1. Click **Next: Authentication**

1. Leave the settings at default but notice that here we are creating a cluster infrastructure service principal which is used by the Kubernetes cluster to manage cloud resources attached to the cluster.(For further information refer to the following link https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal)
1. We are also enabling Role based access control for Kubernetes RBAC (For further information refer to the following link https://docs.microsoft.com/en-us/azure/aks/azure-ad-integration)

1. Click **Next: Networking**

   |Setting  |Value  |
   |---------|---------|
   |Network configuration     |Advanced|

1. **Leave all other settings at default**. Note that we can also set AKS to use a private cluster which gives the API server a provate IP to ensure network traffic between your API server and your node pools remains on the private network only. For this lab though we want to leave that setting at Disabled. (For more information refer to https://docs.microsoft.com/en-us/azure/aks/private-clusters)

1. Click **Next: Monitoring**
   |Setting  |Value  |
   |---------|---------|
   |Container monitoring     |Disabled|


1. Click **Review + Create**

1. Click **Create**
    > Note: It may take up to 15 minutes for AKS to deploy

1. Once the deployment is complete navigate to **Resource Groups**. Notice that you will have a new resource group that holds components of the AKS Nodes named similar to this format dependig on the location **MC_AZ500LAB09_MyKubernetesCluster_eastus**. You will also find your AKS Cluster in the AZ500LAB09 along with the new VNET.

1. Lets now connect to our Kubernetes cluster. Open **Cloud Shell** and select ****. Type in the following command

    ```
    az aks get-credentials --resource-group AZ500LAB09 --name MyKubernetesCluster
    ```
1. now type in the following command to ensure you can see your nodes.

    ```
    kubectl get nodes
    ```

### Task 4: Give AKS permission to access ACR
1. We now need to give AKS permission to access our container registry. Run the following command to do that. (Note this may take between 10 and 15 minutes to complete. You will also notice the the AKS cluster is in an updating state during the update)
    ```
    az aks update -n MyKubernetesCluster -g AZ500LAB09 --attach-acr <ACRuniquename> 
    ```
    > Note this may take up to 10 minutes

### Task 5: Deploy an external facing container from ACR to AKS

1. From **Cloud Shell** select **Bash** if you are not still in it

1. type in the following command to download our Manifest files

    ```
    wget https://raw.githubusercontent.com/sympa18/AZ-500-Azure-Security/master/2020/Allfiles/Labs/LAB_09/nginxexternal.yaml && wget https://raw.githubusercontent.com/sympa18/AZ-500-Azure-Security/master/2020/Allfiles/Labs/LAB_09/nginxinternal.yaml
    ```
1. Type the following to open the nginxexternal.yaml 

    ```
    code ./nginxexternal.yaml
    ```

1. Click on the **Open Editor** the icon with the curly brackets.

1. From **files** scroll down until you see the file you just uploaded **nginxexternal.yaml** and click on it.

1. You will now see the YAML file to your right.
1. On **line 21** replace **ACRUniquename** with your ACR name

    image: **ACRUniquename**.azurecr.io/sample/nginx:v1
1. Click on the **elipsis** > **Save**
1. Click on the **elipsis** > **Close editor**
1. Type in the following in Cloud Shell
    ```
    kubectl apply -f nginxexternal.yaml
    ```

1. You should have seen an output such as below stating that the deployment and the service have been created.

    ```
    deployment.apps/nginxexternal created
    service/nginxexternal created
    ```

### Task 6: Verify the you can access the container externally

1. Type in the following command to get the public IP that has been assigned to the pod(container)

    ```
    kubectl get service nginxexternal
    ```

1. You should see something similar to this output which shows in my case that my website will be at the public IP 52.191.80.141

    ```
    NAME            TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)        AGE
    nginxexternal   LoadBalancer   10.0.60.56   52.191.80.141   80:32591/TCP   2m9s
    ```
1. Navigate to the Public IP you had in your own output and paste it into a browser to confirm you can access the NGINX we server

### Task 7: Deploy an internal facing container from ACR to AKS

1. From **Cloud Shell** select **Bash** if you are not still in it

1. Type the following to open the nginxexternal.yaml 

    ```
    code ./nginxinternal.yaml
    ```
1. You will now see the YAML file to your right.
1. On **line 21** replace **ACRUniquename** with your ACR name

    image: **ACRUniquename**.azurecr.io/sample/nginx:v1
1. Click on the **elipsis** > **Save**
1. Click on the **elipsis** > **Close editor**
1. Type in the following in Cloud Shell
    ```
    kubectl apply -f nginxinternal.yaml
    ```

1. You should have seen an output such as below stating that the deployment and the service have been created.

    ```
    deployment.apps/nginxinternal created
    service/nginxinternal created
    ```


### Task 8: Verify the you can access the container internally

1. Type in the following command to get the public IP that has been assigned to the pod(container)

    ```
    kubectl get service nginxinternal

1. You should see something similar to this output which shows in my case that my website will be at the private IP 10.240.0.35. Take note of your private IP for use later in this lab.

    > When you view the service details, the IP address of the internal load balancer is shown in the EXTERNAL-IP column. In this context, External is in relation to the external interface of the load balancer, not that it receives a public, external IP address.

    ```
    NAME            TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
    nginxinternal   LoadBalancer   10.0.78.106   10.240.0.35   80:31834/TCP   41s
    ```
1. To be able to access this we need to deploy a server within the same VNET. 

1. Navigate to **Resource Groups** > **AZ500LAB09** Select **AZ500LAB09-vnet**

1. Under **Settings** Select **Subnets**. Click **+ Subnet**

1. For **Name** Type in **JumpBox** and leave the default address range of  **10.0.0.0/24**. 

1. Click **OK**

1. Once the subnet has been created navigate to **Create a resource**, under **Popular** Click  **Windows Server 2016 Datacenter**.

1. Choose the following settings

   |Setting  |Value  |
   |---------|---------|
   |Subscription     |_your subscription_|
   |Resource group     |**Use existing**: AZ500LAB09 |
   |Virtual Machine name     |AKSJumpBox|
   |Region     |East US|
   |Administrator account | student |
   |Password     | Pa55w.rd1234 |
   |Size       | DS3_v2 |
   |Public inbound ports     |Allow selected ports: RDP(3389)|

1. Click **Next: Disks**
1. Click **Next: Networking**
1. Choose the following settings

   |Setting  |Value  |
   |---------|---------|
   |Virtual network     |AZ500LAB09-vnet|
   |subnet     |JumpBox (10.0.0.0/24|
   |Virtual Machine name     |AKSJumpBox|
1. Leave everything else as default.
1. Click **Next: Management**
1. Set **Boot Diagnostics** to **Off**
1. Click **Review + Create** then **Create**
1. Once the VM has deployed click **Resource Groups** > **AZ500LAB09**.

1. You will notice that the AKSJumpBox and its resources now reside in the Resource group. You will also notice that we have Network security groups that we can control network flow within the Virtual network to the AKS Cluster.
1. Click on the VM **AKSJumpBox**
1. Click on the **Connect** button then **RDP**
1. Ensure **Public IP Address** is selected, then click Doanload RDP File to connect to the VM via RDP.
1. Open the file and when prompted click **Connect** 

1. When prompted put in the following.

   |Setting  |Value  |
   |---------|---------|
   |Username  | student |
   |Password  | Pa55w.rd1234 |

1. Click **Yes** when prompted
1. Start Internet Explorer and type in the private IP you noted before. 

1. You should be connectd to the NGINX website through the private IP.

### Remove resources.

1. Open **Cloud Shell** in **PowerShell**

1.  Remove the resource group by running the following command (When prompted to confirm press Y and press enter):(Use the Resource Group names that we created in your lab)
    ```
    Remove-AzResourceGroup -Name "AZ500LAB09"
    Remove-AzResourceGroup -Name "MC_AZ500LAB09_MyKubernetesCluster_eastus"
    ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Result**: In this exercise, you removed the resources used in this lab.