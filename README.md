# Config Sync Test Repository

This repository contains Kubernetes configuration files managed by **Google Cloud Config Sync**. It uses the **Unstructured Source Format**, allowing you to organize your Kubernetes resources in any directory structure you prefer under the sync directory.

## Repository Structure

With the unstructured format, you have flexibility in how you organize your manifests:

```text
config-sync/
├── cluster/                # Contains cluster-scoped objects
│   └── gatekeeper-validating-webhook-configuration.yaml
└── namespaces/             # Contains namespaces and namespace-scoped objects
    └── test-configsync.yaml
```

### Why this structure?

Config Sync relies on these exact paths to determine how to apply configurations:

- **`config-sync/`**: The root directory that Config Sync is configured to read from.
- **`cluster/`**: Contains objects that are scoped to the entire cluster, such as `ValidatingWebhookConfiguration`, `ClusterRole`, and `ClusterRoleBinding`.
- **`namespaces/`**: Any resource placed here is treated as a namespace or a namespace-scoped object. Config Sync will apply these to the cluster.

Unlike the hierarchical format, the unstructured format does not require a `system/repo.yaml` file and does not rely on strict directory structures for inheritance.

## References

For more details on how unstructured format works and how to use it, please refer to the official documentation:

*   **[Unstructured format](https://cloud.google.com/anthos-config-management/docs/concepts/unstructured-repo)**: Detailed explanation of the unstructured directory layout.
*   **[Config Sync Overview](https://cloud.google.com/anthos-config-management/docs/config-sync-overview)**: General documentation for Config Sync.

## Usage

When connected to a Kubernetes cluster (GKE, AKS, EKS, etc.) with Config Sync installed in unstructured mode:
1.  Config Sync reads all YAML/JSON manifests under the `config-sync` directory.
2.  It applies the manifest in `namespaces/`, creating the `test-configsync` namespace in the cluster.
3.  It applies the manifest in `cluster/`, creating the `ValidatingWebhookConfiguration`.