# Kubernetes 

# General intruduction
## Some basis information about Kubernetes
- Kubernetes is among the largest open-source projects in the world
- It has a huge ecosystem of tools and applications.
- Kubernetes (Greek: helmsman (Steuerman)), often you will find the term k8s
- k8s was invented by Google and released in 2014 as open-source
- Kubernetes is a “Container Orchestration platform”
- maintained by the Cloud Native Computing Foundation (CNCF) a subsidiary of the Linux Foundation

## What preconditions you need to work with Kubernetes ?
- Solid understanding of Linux commands is a big plus (Recommendation: run k8s on Linux)
- you need to have a solid understanding of container technologies
(Docker, Dockerfiles, Container-Repository)
- You should have a basic understanding of networks, DNS, routing and load balancing
- Understand YAML files

## Which Kubernetes Version should I use ?
For professional usage you likely use one of the widely used managed service from the hyperscalers (AWS, Azure, Google Cloud platform)  
- Google Kubernetes Engine (GKE, 
- Azure Kubernetes Service (AKS),
- Amazon Elastic Kubernetes Service (EKS)
  
For “private” usage (to learn and play), there are bare-metal-environments like:  
- k3s
- microk8s
- minicube
  
or you can use an online playground:
- Killercoda Interactive Environments )

## Best practices
- use namespaces
- use liveness probes and readiness probes
- use resource limits and resource requests
- use deployments
- use role-based access control (RBAC)
- use a managed Kubernetes service
- keep your containers small
- label your objects

## Links
### Kubernetes as managed services
https://aws.amazon.com/de/eks/                                     EKS in the AWS Cloud
  
https://azure.microsoft.com/en-us/products/kubernetes-service/     Azure k8s service


### Kubernetes Distributions (for testing and learning)
https://k3s.io/                                                    K3S  
https://minikube.sigs.k8s.io/docs/                                 Minicube  
https://microk8s.io/                                               MicroK8S  
https://kind.sigs.k8s.io/                                          Kind / k8s in Docker  
https://killercoda.com/                                            k8s online playground  


### Documentation
https://kubernetes.io/docs/home/                                   General Documentation  
https://gateway-api.sigs.k8s.io/                                   k8s Gateway API  
https://learnk8s.io/troubleshooting-deployments                    Troubledhooting the visual way  


### Kubernetes Management tools Tools
https://k8slens.dev/                                               Lens, a Kubernetes IDE  
https://k9scli.io/                                                 k8s CLI  


### Kubernetes Ecosystem
https://artifacthub.io/                                            Helm Charts (Package manager for helmfiles)  
https://operatorhub.io/                                            k8s Operators  
https://www.kubeflow.org/                                          Machine learning on k8s  
https://krew.sigs.k8s.io/                                          Plugin manager (ns, tree, neat, ...)  
https://github.com/stackrox/kube-linter                            Checks YAML files for best practices  
https://argoproj.github.io/argo-workflows/                         Run workflows in k8s  
https://velero.io/                                                 Backup klusters and resources  


### Other stuff
Service Mesh (Istio, Linkerd)  
Open Policy Framework  
Krew NodeShell  
CEPH, Longhorn, openEbs, clusterFs (the easiest) Shared Filesystems  


# General config
### important entries for .bashrc or .zshrc                                                                                                                                    
```
# set the namespace
ns() {                                                                         
  kubectl config set-context --current --namespace=$1 >/dev/null
}

# kubectl as shortcut
alias k='kubectl "--context=${KUBECTL_CONTEXT:-$(kubectl config current-context)}"'
```

# Nodes (no, nodes)
```
k get no                                                    # get list of all nodes
```

# Namespaces (ns, namespaces)
```
k get ns                                                    # get list of all namespaces
k get namespaces                                            # get list of all namespaces
k create ns test01                                          # create namespace test01
k delete ns test01                                          # delete namespace test01
k config view                                               # show the current namespace
k config set-context --current --namespace=test01           # set namespace to test01
```

# PODS (pods)
```
k get pods                                                    # list all pods
k get pods -o wide                                            # list all pods detailed
k get pods -o json                                            # list all pods as json
k get pods -o yaml                                            # list all pods as yaml
k get pods --show-labels                                      # list all pods and their labels
k label pods whoami run2=whoami2                              # add the label run2=whoami2 to pod whoami
k label pods whoami run2-                                     # remove the label run2 from pod whoami
k run whoami --image bee42/whoami:2.2.0 --expose --port 80    # expose pod (start pod)
k delete pod whoami                                           # delete the pod whoami
```

### write to a manifest named whoami-pod.yaml
```
k run whoami2 --image bee42/whoami:2.2.0 --labels=run=whoami --dry-run=client -o yaml >whoami-pod.yaml

k apply -f whoami-pod.yaml                                    # start a pod from a manifest file
```

# Service (svc, services)
service types:
clusterip    = service is only reachable from within the cluster  
nodeport     = expose service on each node at a static ip (the node port)  
loadbalancer = exposes the service externaly  

```
k get svc                                                     # get list of all services
k get services                                                # get list of all services
k get svc my-svc -o jsonpath='{$.spec.ports[0].nodePort}'     # get the port of an service
k describe svc my-svc                                         # describes a service
k label pod whoami --overwrite app=node-svc                   # add the pod whoami to the service my-svc
```

### create svc (clusterip) name it my-svc and write it to file
```
k create svc clusterip my-svc --clusterip="None"  --tcp=80:80 -o yaml --dry-run=client > srv.yaml
```

### create svc (clusterip) name it clusterip-svc and attach it to the pod whoami
```
k create svc clusterip clusterip-svc --tcp=80:80
k label pod whoami --overwrite app=clusterip-svc
```

### create svc (nodeport) name it nodeport-svc and attach it to the pod whoami
```
k create svc clusterip nodeport-svc --tcp=80:80
k label pod whoami --overwrite app=nodeport-svc
```

### create svc (loadbalancer) and name it lb-scv and attach it to the pod whoami
```
k create service loadbalancer lb-svc --tcp=80:80
k label pod whoami --overwrite app=lb-svc
```


# Endpoints (ep, endpoint)
```
k get endpoint                                                # list all endpoints
```

# Deployments

# create a deployment and expose it with a service
create a deployment file as yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 2
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
```
create the deployment from the file
```
k apply -f deployment.yaml
```
expose the deployment as service
```
k expose deployment/my-nginx
```
scale the deployment to three pods
```
k scale --replicas=3 deployments/my-nginx
```
inspect the service
```
k describe svc my-nginx
```


### 

# General things
### access a service inside the kluster
```
k run shell --tty -i --image alpine -- /bin/sh
wget -O - -q http://servicename
exit
```

### reenter into the shell service
```
kubectl attach -ti shell
```

### get cluster info
```
kubectl cluster-info
```

### create deployment
```
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

### get deployments
```
kubectl get deployments
```

### show the kubectl version
```
kubectl version --output=yaml
```

