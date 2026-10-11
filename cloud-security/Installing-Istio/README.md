# Installing Istio 

## 1. Install istio in tools

- As Istio will create a new dir, install in tools dir 

```
# Navigate to your tools directory
cd ~/tools

# Download and extract Istio here
curl -L https://istio.io/downloadIstio | sh -

```

## 2. Add Path if not already present 


```
# Add to bashrc (only if not already there)
grep -q 'tools/istio' ~/.bashrc || echo 'export PATH=$HOME/tools/istio-1.31.1/bin:$PATH' >> ~/.bashrc

# Reload bashrc
source ~/.bashrc
```

## 3. Verify installation 

```
# Confirm the correct binary is being used
which istioctl
# Expected: /home/padmanaban/tools/istio-1.31.1/bin/istioctl

# Verify version
istioctl version --remote=false
# Expected: client version: 1.31.1
```

## 4. Deploy cluster 

```
k3d cluster create interview-demo \
  --servers 1 \
  --agents 2 \
  --port "80:80@loadbalancer" \
  --port "443:443@loadbalancer" \
  --k3s-arg "--disable=traefik@server:0"
```

## 5. Install istio in the cluster

```
istioctl install --set profile=demo -y
```

Verify pods are running

```
kubectl get pods -n istio-system
```

## 6. Enable sidecar injection 

```
kubectl label namespace default istio-injection=enabled
```

## 7. Ingress gateway has an External IP

```
kubectl get svc istio-ingressgateway -n istio-system
```

## 8. Deploy Kiali Dashboard

```
kubectl apply -f addon-yamls/prometheus.yaml

kubectl apply -f addon-yamls/kiali.yaml 

# Wait for pods to be ready
kubectl get pods -n istio-system -w
```

## 9. Open kiali dashboard 

```
istioctl dashboard kiali
```

## . check demo with book info APP

```
# Deploy the app
kubectl apply -f bookinfo-yamls/bookinfo.yaml

# Deploy the gateway configuration
kubectl apply -f  bookinfo-yamls/bookinfo-gateway.yaml

# Verify no configuration issues
istioctl analyze
```