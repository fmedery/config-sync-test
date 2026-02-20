# Config Sync Test Repository

This repository contains Kubernetes configuration files managed by **Google Cloud Config Sync**. It is structured to use the **Hierarchical Source Format**, a specific directory layout that Config Sync uses to apply inheritance and structure to your Kubernetes resources.

## Repository Structure

This repository follows the strict directory structure required by Config Sync's hierarchical mode:

```text
config-sync/
├── cluster/                # Contains cluster-scoped objects
│   └── gatekeeper-validating-webhook-configuration.yaml
├── namespaces/             # Contains namespaces and namespace-scoped objects
│   └── test-configsync.yaml
└── system/                 # System configurations for Config Sync
    └── repo.yaml           # REQUIRED: Marks this as a hierarchical repo
```

### Why this structure?

Config Sync relies on these exact paths to determine how to apply configurations:

- **`config-sync/`**: The root directory that Config Sync is configured to read from.
- **`cluster/`**: Contains objects that are scoped to the entire cluster, such as `ValidatingWebhookConfiguration`, `ClusterRole`, and `ClusterRoleBinding`.
- **`namespaces/`**: Any resource placed here is treated as a namespace or a namespace-scoped object. Config Sync handles **inheritance** here; configs placed in parent directories are automatically applied to child namespaces.
- **`system/repo.yaml`**: This file is **mandatory** for the hierarchical format. It contains versioning information and explicitly tells Config Sync that this repository uses the hierarchical structure (as opposed to unstructured). Without this file, the hierarchical features (like abstract namespaces and automatic inheritance) would not work.

## References

For more details on how this structure works and how to use it, please refer to the official documentation:

*   **[Structure of the Hierarchical Repo](https://cloud.google.com/anthos-config-management/docs/concepts/hierarchical-repo)**: Detailed explanation of the directory layout and rules.
*   **[Config Sync Overview](https://cloud.google.com/anthos-config-management/docs/config-sync-overview)**: General documentation for Config Sync.

## Usage

When connected to a Kubernetes cluster (GKE, AKS, EKS, etc.) with Config Sync installed:
1.  Config Sync reads the `config-sync` directory.
2.  It detects the `system/repo.yaml` and enables hierarchical mode.
3.  It applies the manifest in `namespaces/`, creating the `test-configsync` namespace in the cluster.