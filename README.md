# FIAP TRAVEL

###### O Objetivo deste projeto é mostrar a intgração entre o front-end e o back-end utilizando os recursos da Microsoft Azure.
<br />


### 1. Obtendo os recursos para deploy da solução
###### Você precisará dos arquivos de configuração do ambiente. 
###### Estes arquivos contém todas as informações para configuração do ambiente da solução

1. Baixando os arquivos de configuração
```
git clone https://github.com/marcocouzin/fiap_travel_k8s.git
```



<br /><br /><br /><br /><br />




### 2. CONFIGURANDO O CLUSTER E O KUBERNETES
###### A maior parte da aplicação rodará no cluster Kubernete
####  Para instalar a solução, execute os comandos abaixo:
1. Criar um cluster utilizando o Azure Cloud Shell
```
az group create --name gpakstravel --location eastus && az aks create --name akstravel --resource-group gpakstravel --node-count 2 --generate-ssh-keys
```
2. Conecte ao cluster
```
az aks get-credentials --resource-group gpakstravel --name akstravel
```




<br /><br /><br /><br /><br />





### 3. FERRAMENTA GRÁFICA DE ADMINISTRAÇÃO 
###### Uma forma fácil de visualizar o que está acontecendo no ambiente é utilizar uma ferramenta chamada Weave Scope.
###### É pré-requisito a criação do AKS
####  Para instalar a solução, execute os comandos abaixo:

1. Instalar o Weave Works
```
kubectl apply -f https://github.com/weaveworks/scope/releases/download/v1.13.2/k8s-scope.yaml && kubectl patch svc weave-scope-app -n weave -p '{"spec": {"type": "LoadBalancer"}}'
```
2. Obtenha o IP externo gerado na instalação
```
kubectl get svc -n weave
```
![img_2.png](img_2.png)
3. Abra um browser e digite o endereço IP obtido no passo anterior
```
http://<external_ip>
```
![img_5.png](img_5.png)



<br /><br /><br /><br /><br />





### 4. INSTALANDO A SOLUÇÃO DE BACK-END (BFF):
###### Está solução prove um serviço de back-end para o front-end.
####  Para instalar a solução, execute os comandos abaixo:
1. Acesse a pasta que contém os arquivos de deployment
```
cd ~/fiap_travel_k8s/deploy_fiap_bff_travel/
```
2. Criar o recurso para deploy
```
kubectl create -f deploy_fiap_bff_travel_app.yml
```
3. Criar o load balancer para expor o serviço
```
kubectl create -f deploy_fiap_bff_travel_loadbalancer.yml
```
4. Obtenha o IP externo, do balanceador, gerado na instalação
```
kubectl get service
```
![img_3.png](img_3.png)
5. Teste a solução. Abra um browser e acesse a URL conforme abaixo:
```
http://<external_ip>:8081/swagger-ui/index.html
```
![img_4.png](img_4.png)
- OU Verifique no Weave Scope

![img_6.png](img_6.png)
6. Atualize o IP externo do Back-end external no serviço utilizado para controlar as variáveis de ambiente para que o front-end possa acessar o back-end
```
https://www.mockable.io/
```



<br /><br /><br /><br /><br />





### 5. INSTALANDO A SOLUÇÃO DE FRONT-END (FED):
###### Está solução prove um serviço de front-end
####  Para instalar a solução, execute os comandos abaixo:
1. Acesse a pasta que contém os arquivos de deployment
```
cd ~/fiap_travel_k8s/deploy_fiap_fed_travel/
```
2. Criar o recurso para deploy
```
kubectl create -f deploy_fiap_fed_travel_app.yml
```
3. Criar o load balancer para expor o serviço
```
kubectl create -f deploy_fiap_fed_travel_loadbalancer.yml
```
4. Obtenha o IP externo, do balanceador, gerado na instalação
```
kubectl get service
```
--- evidence ---
5. Teste a solução. Abra um browser e acesse a URL conforme abaixo:
```
http://<external_ip>
```
![img_1.png](img_1.png)

- OU Verifique no Weave Scope

![img_7.png](img_7.png)




<br /><br /><br /><br /><br />




### 6. EXCLUINDO OS RECURSOS
####  Para excluir a solução, execute os comandos abaixo:
```
az aks delete --yes --name akstravel --resource-group gpakstravel && az group delete --yes --resource-group gpakstravel && az group delete --yes --resource-group NetworkWatcherRG
```




<br /><br /><br /><br /><br />





### 7. COMANDO ÚTEIS
- Lista os serviços
```
kubectl get service
```
- Lista os recursos de deployment
```
kubectl get deploy 
```
- Lista os recursos de ReplicaSets
```
kubectl get replicasets
```
- Lista os pods
```
kubectl get pods
```
Lists os nós
```
kubectl get nodes
```
Lista os nameSpace
```
kubectl get namespaces
```