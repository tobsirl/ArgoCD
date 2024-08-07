# ArgoCD

## ArgoCD Introduction

### What is ArgoCD?

ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.

### ArgoCD Features

1. **Automated Deployment:** Sync applications with desired state defined in Git repository.
2. **Easy Rollback:** Quick recovery from undesired changes or failed deployments.
3. **Multi-Cluster Support:** Manage deployments across multiple Kubernetes clusters.
4. **Visualizations:** Rich UI for visualizaion and management of applications.

### What is GitOps?

GitOps is a way to do Kubernetes cluster management and application delivery. It works by using Git as a single source of truth for declarative infrastructure and applications. With Git at the center of your delivery pipelines, developers can make pull requests to accelerate and simplify application deployments and operations tasks to Kubernetes.

### GitOps Workflow

- **Push-based Model:**
- Changes are pushd to the Git repository, triggering the desired changes in the infrastructure or application.
- The push-based approach simplifies automation and ensures a consistent state across environments.

- **Pull and Reconciliation**
- Automation tools continuously pull from the Git repository and reconcile the system to match the declared state.
- Automatic synchromization reduces manual interventions and minimizes configuration drift.

### Core Principles

- Declarative Configuration
- Version controlled
- Reconciliation

### Benefits

- Consistency
- Traceability
- Collaboration

### Understanding Applications in ArgoCD

ArgoCD Applications are declarative definitions that describe the desired state of applications in a Git repository, including the source of the application, path to the Kubernetes manifests, destination cluster, and namespace.

- **Source Repository:** Where your Kubernetes manifests are stored.
- **Path:** The directory within the repository where the manifests are located.
- **Destination Cluster:** The Kubernetes cluster where the application will be deployed.
- **Destination Namespace:** The Kubernetes namespace in the cluster where resources will be created.

### ArgoCD Application Specification

An ArgoCD Application Specification defines the deployment and management parameters for an application in a Kubernetes cluster. The specification includes several key elements:

#### Metadata

Includes the application's name, namespace, and labels, along with finalizers to control deletion behavior.

```yaml
metadata:
  name: guestbook
  namespace: argocd
  labels:
    app.kubernetes.io/name: guestbook
  finalizers:
    - resources-finalizer.argocd.argoproj.io
```

#### Source

Defines the location and specifics of the manifests, such as a Git repository.

```yaml
source:
  repoURL: https://github.com/argoproj/argocd-example-apps.git
  targetRevision: HEAD
  path: guestbook
```

#### Destination

Specifies where the application should be deployed within the cluster.

```yaml
destination:
  server: https://kubernetes.default.svc
  namespace: guestbook
```

#### Sync Policy

Configures synchronization behavior, including options for pruning and self-healing.

```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
```

#### Ignore Differences

Lists resource fields that should be ignored during synchronization.

```yaml
ignoreDifferences:
  - group: apps
    kind: Deployment
    jsonPointers:
      - /spec/replicas
```

#### Tool-specific Configuration

Additional configurations for tools like Helm or Kustomize.

```yaml
helm:
  parameters:
    - name: image.tag
      value: v1.0.1
kustomize:
  namePrefix: prod-
```

#### Health Checks

Defines custom health checks for resources to monitor the application's state.

Each section can be expanded with details specific to your deployment and applied to ArgoCD with `kubectl apply -f application.yaml`. Monitoring can be done via ArgoCD CLI or UI.
