# General config
### important entries for .bashrc or .zshrc                                                                                                                                    
```
ns() {                                                                         
  kubectl config set-context --current --namespace=( >/dev/null
  }
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

