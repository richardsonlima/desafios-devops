# Challenger 02: Kubernetes

## Step 1. Install `kubernetes-docker-in-docker-cluster`. 
So it should be an intereting tool to delivery a k8s cluster locally 

```console

# Linux  
$ curl -O https://storage.googleapis.com/kubernetes-release/release/v1.9.9/bin/linux/amd64/kubectl
$ mv kubectl /usr/local/bin/

# macOS
$ brew install kubernetes-cli
$kubectl version


$ wget https://raw.githubusercontent.com/richardsonlima/kubernetes-docker-in-docker-cluster/master/kubernetes-docker-in-docker-cluster-v1.9.sh
$ chmod +x kubernetes-docker-in-docker-cluster-v1.9.sh

$ # start the cluster
$ ./kubernetes-docker-in-docker-cluster-v1.9.sh up

$ # also you can start the cluster with 3 nodes
$ NUM_NODES=3 ./kubernetes-docker-in-docker-cluster-v1.9.sh up

$ ./kubernetes-docker-in-docker-cluster-v1.9.sh initial-config 

$ kubectl --kubeconfig ~/.kube/config get pods --all-namespaces

# Creating a namespace
$ kubectl --kubeconfig ~/.kube/config create namespace `desafio-devops`

$ kubectl --kubeconfig ~/.kube/config get nodes
NAME          STATUS    ROLES     AGE       VERSION
kube-master   Ready     master    57m       v1.9.9
kube-node-1   Ready     <none>    56m       v1.9.9
kube-node-2   Ready     <none>    56m       v1.9.9
kube-node-3   Ready     <none>    56m       v1.9.9

$ # k8s dashboard available at http://localhost:[aleatory-port]/api/v1/namespaces/kube-system/services/kubernetes-dashboard:/proxy

$ # restart the cluster, this should happen much quicker than initial startup
$ ./kubernetes-docker-in-docker-cluster-v1.9.sh up

$ # stop the cluster
$ ./kubernetes-docker-in-docker-cluster-v1.9.sh down

$ # remove containers and volumes
$ ./kubernetes-docker-in-docker-cluster-v1.9.sh clean
```

Terminal session recorded::
[![asciicast](https://asciinema.org/a/195341.png)](https://asciinema.org/a/195341?autoplay=1) 

## Step 2. Install `helm`  
```console
$ curl -fsSL https://raw.githubusercontent.com/fishworks/gofish/master/scripts/install.sh | bash
$ gofish init
$ gofish install helm
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm init
```

## Step 3. Install `jenkins` locally 

On macOSx
```console
brew install jenkins
```
Using Docker

```console
docker run \
  -u root \
  --rm \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
```


 ```console
  $ kubectl port-forward svc/guestbook 8090:3000
  Forwarding from 127.0.0.1:8090 -> 3000
  Forwarding from [::1]:8090 -> 3000
```














# Desafio 02: Kubernetes

## Motivação

Kubernetes atualmente é a principal ferramenta de orquestração e _deployment_ de _containers_ utilizado no mundo, práticamente tornando-se um padrão para abstração de recursos de infraestrutura. 

Na IDWall todos nossos serviços são containerizados e distribuídos em _clusters_ para cada ambiente, sendo assim é importante que as aplicações sejam adaptáveis para cada ambiente e haja controle via código dos recursos kubernetes através de seus manifestos. 

## Objetivo
Dentro deste repositório existe um subdiretório **app** e um **Dockerfile** que constrói essa imagem, seu objetivo é:

- Construir a imagem docker da aplicação
- Criar os manifestos de recursos kubernetes para rodar a aplicação (_deployments, services, ingresses, configmap_ e qualquer outro que você considere necessário)
- Criar um _script_ para a execução do _deploy_ em uma única execução.
- A aplicação deve ter seu _deploy_ realizado com uma única linha de comando em um cluster kubernetes **local**
- Todos os _pods_ devem estar rodando
- A aplicação deve responder à uma URL específica configurada no _ingress_


## Extras 
- Utilizar Helm [HELM](https://helm.sh)
- Divisão de recursos por _namespaces_
- Utilização de _health check_ na aplicação
- Fazer com que a aplicação exiba seu nome ao invés de **"Olá, candidato!"**

## Notas

* Pode se utilizar o [Minikube](https://github.com/kubernetes/minikube) ou [Docker for Mac/Windows](https://docs.docker.com/docker-for-mac/) para execução do desafio e realização de testes.

* A aplicação sobe por _default_ utilizando a porta **3000** e utiliza uma variável de ambiente **$NAME**

* Não é necessário realizar o _upload_ da imagem Docker para um registro público, você pode construir a imagem localmente e utilizá-la diretamente.
