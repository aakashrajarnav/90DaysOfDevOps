## Kubernetes Quick Commands Cheat Sheet

| Topic | Command | Explanation |
|------|--------|------------|
| Cluster Info | kubectl cluster-info | Displays Kubernetes control plane and service endpoints |
| Nodes | kubectl get nodes | Lists all nodes in the cluster |
| Node Debug | kubectl describe node | Provides detailed node diagnostics |
| Node Details | kubectl describe node `<node-name>` | Shows detailed information about a specific node |
| Namespaces | kubectl get namespaces | Lists all namespaces in the cluster |
| Pods (All) | kubectl get pods -A | Lists all pods across all namespaces |
| Pods (Detailed) | kubectl get pods -o wide | Shows pods with node and IP details |
| Pods (Namespace) | kubectl get pods -n kube-system | Lists pods in kube-system namespace |
| Services | kubectl get svc | Lists all services in the cluster |
| Events | kubectl get events | Shows recent cluster events |
| Components | kubectl get componentstatuses | Displays status of control plane components |
| Context (Current) | kubectl config current-context | Shows current cluster context |
| Contexts List | kubectl config get-contexts | Lists all available kubeconfig contexts |
| Kubeconfig View | kubectl config view | Displays full kubeconfig configuration |
| Cluster Delete (Kind) | kind delete cluster --name devops-cluster | Deletes KIND cluster |
| Cluster Create (Kind) | kind create cluster --name devops-cluster | Creates KIND cluster |
| Cluster Delete (Minikube) | minikube delete | Deletes Minikube cluster |
| Cluster Create (Minikube) | minikube start | Starts Minikube cluster |