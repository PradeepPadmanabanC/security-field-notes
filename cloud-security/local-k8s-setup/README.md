# Local K3d setup 

## 1. Pre-requiste : Verify Docker 

check if Docker is running on the machine 
```
docker --version
```

## 2. Install K3d and Kubectl 

```
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

k3d version
```

## 3. Create a Demo-Ready Cluster

```
k3d cluster create interview-demo \
  --servers 1 \
  --agents 2 \
  --port "8080:80@loadbalancer"

```


### note: to delete cluster 

```
k3d cluster delete interview-demo
```

```
# Check if cluster is deleted 
k3d cluster list
```