# Cloud-Native Perimeter

This repository manages the deployment configurations for the `cloud-native-perimeter` infrastructure using GitOps principles and ArgoCD. It utilizes an **App-of-Apps pattern** via a root application configuration to dynamically manage internal components and guestbook applications.

## 📂 Repository Structure

```text
├── apps/
│   └── guestbook/                  # Guestbook application component
│       ├── templates/
│       │   ├── deployment.yaml     # Application deployment specs
│       │   ├── ingress.yaml        # External access rules
│       │   └── service.yaml        # Internal networking
│       ├── Chart.yaml              # Helm chart definition
│       └── values.yaml             # Deployment configuration values
├── Perimeter/
│   └── manifests/
│       └── argocd-ingress.yaml     # Core routing infrastructure for ArgoCD
└── root-application.yaml           # Root ArgoCD Application (App-of-Apps)
```

## 🚀 Architecture Overview

This project uses **ArgoCD** to continuously synchronize the state of the Kubernetes cluster with this Git repository. 

*   **Root Application (`root-application.yaml`)**: Acts as the main entry point. It monitors the `apps/guestbook` path and deploys it automatically.
*   **Guestbook App**: A containerized application template configured as a Helm chart.
*   **Perimeter Manifests**: Contains network edge configurations like `argocd-ingress.yaml` to securely route traffic.

## 🛠️ Getting Started

### Prerequisites
*   A running Kubernetes cluster.
*   `kubectl` configured to access your cluster.
*   ArgoCD installed in the `argocd` namespace.

### Deployment via ArgoCD

To bootstrap the entire stack, apply the root application manifest directly to your cluster:

```bash
kubectl apply -f root-application.yaml
```

Once applied, ArgoCD will read the root configuration, point to this repository, and automatically trigger the creation and synchronization of the components defined in `apps/guestbook/`.

## ⚙️ Root Configuration Details

The root application features the following automation policies:
*   **Automated Sync**: Continuously checks the repository for changes.
*   **Prune**: Automatically removes resources from the cluster if they are deleted from Git.
*   **Self-Heal**: Automatically overwrites manual cluster overrides to maintain the Git source of truth.
