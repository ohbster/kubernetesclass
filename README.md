# kubernetesclass
Thursday Night Fun



Universal Pattern (important mental model)

Every managed Kubernetes service does the same thing:
Use cloud CLI authentication (AWS or GCP)
Fetch cluster endpoint + certs
Write/update ~/.kube/config
kubectl now talks to the cluster using cloud IAM

AWS (EKS)
Prerequisites (already assumed, but say it once)
aws configure

aws eks update-kubeconfig \
  --region <region> \
  --name <cluster-name>

What this does

Pulls the EKS cluster endpoint
Adds a context to ~/.kube/config
Configures AWS IAM auth via aws-iam-authenticator
Does not create users or permissions (IAM already did that)

Verify immediately (discipline)
kubectl get nodes

If this works:
authentication ✔
network ✔
RBAC ✔

GCP (GKE)
Prerequisites

gcloud auth login
gcloud config set project <project-id>

Core command

gcloud container clusters get-credentials <cluster-name> \
  --region <region>


What this does
Fetches cluster endpoint + certs
Writes kubeconfig entry
Uses Google IAM + OAuth behind the scenes
No static credentials stored (token-based)

Verify
kubectl get nodes



| Step         | AWS EKS             | GCP GKE             |
| ------------ | ------------------- | ------------------- |
| Auth source  | IAM                 | Google IAM          |
| CLI tool     | `aws`               | `gcloud`            |
| Kubeconfig   | `~/.kube/config`    | `~/.kube/config`    |
| Verification | `kubectl get nodes` | `kubectl get nodes` |
