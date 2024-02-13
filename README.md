# Stress testing

## Install K3d

## Install Helm

## Install Kube prom stack
- `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

Find the Grafana pod and do a port forward to access it. The default login is `admin`/`prom-operator`

## Install Chaos Mesh
-  `helm repo add chaos-mesh https://charts.chaos-mesh.org`
- `helm search repo chaos-mesh`
- `kubectl create ns chaos-mesh`
- `helm install chaos-mesh chaos-mesh/chaos-mesh -n=chaos-mesh --set chaosDaemon.runtime=containerd --set chaosDaemon.socketPath=/run/k3s/containerd/containerd.sock --version 2.6.3`

Make sure the installation was ok
- `kubectl get pods --namespace chaos-mesh -l app.kubernetes.io/instance=chaos-mesh`

## Access the Chaos mesh dashboard
- Setup port forwarding for the pod called something like `chaos-dashboard-...`
- Access the dashboard on `localhost:<port>`
- Create credentials by:
  - `kubectl apply -f rbac.yaml`
  - `kubectl create token account-cluster-manager-qxsch`
- Paste the token in the dashboard

## Deploy example pods
- `kubectl apply -f deployment.yaml`

## Apply the stress test
There is a builder in the dashboard that can be used, or just apply the following yaml to stress test the CPU of all pods in the `default` namespace.
- `kubectl apply -f cpu-stress.yaml`
