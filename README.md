# FIAP TRAVEL

### CREATING THE AZURE ENVIRONMENT:
```
    1. Create a cluster using the Azure Cloud Shell
    az group create --name gpakstravel --location eastus && az aks create --name akstravel --resource-group gpakstravel --node-count 2 --generate-ssh-keys 

    2. Connect to the cluster
    az aks get-credentials --resource-group gpakstravel --name akstravel
    
    3. Run the Weave Works graphic administration tool
    kubectl apply -f https://github.com/weaveworks/scope/releases/download/v1.13.2/k8s-scope.yaml && kubectl patch svc weave-scope-app -n weave -p '{"spec": {"type": "LoadBalancer"}}'
    
    ! To check the installation and get the external IP, run: kubectl get svc -n weave
```

### SETTING UP THE APPLICATION ENVIRONMENT:

1. Download the set up files
```
    git clone https://github.com/marcocouzin/fiap_travel_k8s.git
```

###### and then, run:

```
    cd fiap_travel_k8s
```


2. Create a deployment resource
3. Create a load balancer to expose the service
4. To destroy the environment, run:

```
    1. Download the set up files
    git clone https://github.com/marcocouzin/fiap_travel_k8s.git
    and then, run: cd fiap_travel_k8s
    
    2. Create a deployment resource
    kubectl create -f deploy_fiap_travel.yml 
    
    3. Create a load balancer to expose the service
    kubectl create -f svc_page_loadbalancer.yml
    
    ! To check the service, run: kubectl get service
    ! Helpful commands:
        kubectl get deploy
        kubectl get replicasets
        kubectl get pods
        kubectl get svc
    
    4. To destroy the environment, run:
    az aks delete --yes --name akstravel --resource-group gpakstravel && az group delete --yes --resource-group gpakstravel
```