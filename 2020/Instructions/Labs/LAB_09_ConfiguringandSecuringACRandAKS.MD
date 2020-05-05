---
lab:
    title: '09 - Configuring and Securing ACR and AKS'
    module: 'Module 02 - Implement platform protection'
---

# Lab 09 - Configuring and Securing ACR and AKS

# Student lab manual

## Lab scenario

blah .
blah 
blah

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

    ```bash
    az group create --name myResourceGroup --location eastus
    ```
1. Now we need to create a brand new Azure Container Registry. Type in the following command into Cloud Shell replacing  with a globally unique name.(Take note of this for use later in the lab)

    ```bash
    az acr create --resource-group myResourceGroup --name <ACRuniquename> --sku Basic
    ```
1. Once that has completed, in the portal navigate to **Resource Groups** > **myResourceGroup**. You should see the new container registry that you created. (It may take a minute or two to appear. Refresh your window)

### Task 2: Build a container and push it to Azure Container Registry

1. Let's now build and deploy an image to ACR. 
1. First lets create a Dockerfile to deploy Nginx.

    ```
    echo FROM nginx > Dockerfile
    ```
1. Now let's build an image off that docker file and push the image to our container registry

    ```
    az acr build --image sample/nginx:v1 --registry <ACRuniquename> --file Dockerfile .
    ```
1. Once the build completes successfully navigate back to **Resource Groups** > **myResourceGroup** > Click on your container registry
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
   |Resource group     |**Use existing**: myResourceGroup |
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

1. Once the deployment is complete navigate to **Resource Groups**. Notice that you will have a new resource group that holds components of the AKS Nodes named similar to this format dependig on the location **MC_myResourceGroup_MyKubernetesCluster_eastus**. You will also find your AKS Cluster in the myResourceGroup along with the new VNET.

1. Lets now connect to our Kubernetes cluster. Open **Cloud Shell** and select **Bash**. Type in the following command

    ```bash
    az aks get-credentials --resource-group myResourceGroup --name MyKubernetesCluster
    ```
1. now type in the following command to ensure you can see your nodes.

    ```bash
    kubectl get nodes
    ```

### Task 4: Give AKS permission to access ACR
1. We now need to give AKS permission to access our container registry. Run the following command to do that. (Note this may take between 10 and 15 minutes to complete. You will also notice the the AKS cluster is in an updating state during the update)
    ```bash
    az aks update -n MyKubernetesCluster -g myResourceGroup --attach-acr <ACRuniquename> 
    ```
    > Note this may take up to 10 minutes

### Task 5: Deploy an external facing container from ACR to AKS

1. From **Cloud Shell** select **Bash** if you are not still in it

1. Click on the **upoad/download files** icon which is to the right of the cog wheel. Select Upload and select the file in the following location **2020\Allfiles\Labs\LAB_09\nginxexternal.yaml**, Click **Open**

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

1. Click on the **upoad/download files** icon which is to the right of the cog wheel. Select Upload and select the file in the following location **2020\Allfiles\Labs\LAB_09\nginxinternal.yaml**, Click **Open**

1. Click on the **Open Editor** the icon with the curly brackets.

1. From **files** scroll down until you see the file you just uploaded **nginxinternal.yaml** and click on it.

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

1. You should see something similar to this output which shows in my case that my website will be at the private IP 10.240.0.35

    ```
    NAME            TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
    nginxinternal   LoadBalancer   10.0.78.106   10.240.0.35   80:31834/TCP   41s
    ```
1. To be able to access this we need to deploy a server within the same VNET. 