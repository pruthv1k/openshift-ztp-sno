# ClusterInstance Change Policy

This document outlines the **permissible changes** to the `ClusterInstance` Custom Resource (CR) at various stages of the OpenShift cluster lifecycle managed via Zero Touch Provisioning (ZTP) and GitOps.

---

## 1. 🟢 Initial Installation

When deploying a cluster for the first time:

✅ **Permitted**:
- Any field under `spec` can be set.

❌ **Restricted**:
- `spec.reinstall` — should **not** be configured during the initial installation.

---

## 2. 🟡 During Cluster Installation (Provisioning In Progress)

❌ **No changes are allowed** to the `ClusterInstance` CR while the cluster is in the provisioning phase.

---

## 3. 🔁 Reinstallations (After Initial Provisioning)

Once a cluster has been provisioned, reinstallation is allowed with strict limitations.

### ✅ Permitted Changes:
- `spec.reinstall`: The entire object can be modified.
- `spec.nodes[*]`: Only the following sub-fields:
  - `bootMACAddress`
  - `nodeNetwork.interfaces.macAddress`
  - `rootDeviceHints`

### ❌ Not Permitted:
- No updates to `spec.reinstall` **while** a reinstallation is in progress.

---

## 4. 🔄 Day-N (Post-Provisioning Updates)

After the cluster is installed, only specific Day-N changes are allowed.

### ✅ Permitted Top-Level Changes:
- `spec.extraAnnotations`
- `spec.extraLabels`
- `spec.suppressedManifests`
- `spec.pruneManifests`

### ✅ Permitted Node-Level Changes (`spec.nodes[*]`):
- **Worker nodes** can be added (scale-out) or removed (scale-in).
- For existing worker nodes, only the following sub-fields may be updated:
  - `extraAnnotations`
  - `extraLabels`
  - `suppressedManifests`
  - `pruneManifests`

---

> ⚠️ **Note**: Any modification outside of the allowed fields listed above may result in errors, failed deployments, or unsupported configurations.

---

## 📘 Reference

Ensure your GitOps workflows, pipelines, and team members are aligned with this policy when managing `ClusterInstance` objects.

