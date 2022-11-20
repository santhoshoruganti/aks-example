Pre-requistes:

Azure CLI is installed on your local machine.
Account setup in Azure cloud.


Step 1

Make sure you are login to Azure portal first.

az login

enter your Microsoft credentials.


Step 2 - create a resource group first.
az group create --name myResourceGroup --location southcentralus

Step 3 Create AKS cluster with 2 worker nodes

az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 2 --enable-addons monitoring --generate-ssh-keys

Display Details of Cluster

az aks show --name myAKSCluster --resource-group myResourceGroup  # The above command will display Cluster details.


Step 4 - Create Azure Container Registry

Run the below command to create your own private container registry using Azure Container Registry (ACR). Make sure below red marked registry name is unique.

az acr create --resource-group myResourceGroup --name <<Addyourregistrtname>> --sku Standard --location southcentralus
  
  
Connect to the cluster

 az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
  
  
  
To verify the connection to your cluster, use the kubectl get command to return a list of the cluster nodes.

kubectl get nodes
  
  # List all deployments in a specific namespace

kubectl get deployments --all-namespaces=true
  
  
  
  For Deploying Docker images from ACR into AKS Cluster 

When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), an authentication mechanism needs to be established. 
You can set up the AKS to ACR integration in a few simple commands with the Azure CLI or Azure PowerShell. This integration assigns the AcrPull role to the managed identity associated to the AKS Cluster.

  For Deploying Docker images from ACR into AKS Cluster 

az aks update -n myAKSCluster -g myResourceGroup --attach-acr myacrrepo4321
  
  
  
  
  
  
  
  
  
  
  
  Finaly 
  
Deploy Nginx App
kubectl create -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/controllers/nginx-deployment.yaml
  
  
  Once the deployment is created, use kubectl to check on the deployments by running this command: 

kubectl get deployments
  
  
  
  
  
  
  Notes to delete
  
  Clean up the cluster

To avoid Azure charges, you should clean up unneeded resources. When the cluster is no longer needed, use the az group delete command to remove the resource group, container service, and all related resources. 

az group delete --name myResourceGroup --yes --no-wait
  
