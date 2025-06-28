# OpenShift ZTP SNO Deployment

This repository contains manifests and automation for deploying **Single Node OpenShift (SNO)** clusters using **Zero Touch Provisioning (ZTP)** and **GitOps workflows** via **ArgoCD** and **Advanced Cluster Management (ACM)**. It supports both initial SNO cluster deployments and Day-2 operations like adding additional worker nodes.

---

## 🔧 Key Features

- GitOps-based SNO provisioning using ZTP
- Compatible with Red Hat ACM and ArgoCD
- Declarative provisioning using `SiteConfig` and `PolicyGenTemplate`
- Supports Day-2 operations (e.g., adding worker nodes)
- DU Profile support for RAN/Edge environments
- Works in disconnected (air-gapped) environments

---

## 📁 Repository Structure

```
.
├── clusters/ocp2/ # Site-specific manifests for the target cluster
│ ├── clusterinstance.yaml # Defines the SNO instance and overrides
│ ├── extra-manifests/ # Additional manifests to be deployed
│ ├── kustomization.yaml # Kustomize entrypoint
│ ├── ns.yaml # Namespace manifest
│ └── secrets.yaml # BMC and pull secret definitions
├── site-configs/ # SiteConfig CRs per site
├── policygentemplates/ # PolicyGenTemplate definitions
├── out/ # Generated CRs (do not manually edit)
├── ztp-site-generate/ # (Optional) Containerized tool for CR generation
```

---

## 🚀 Prerequisites

- A working hub cluster with:
  - **ACM** (v2.6 or later)
  - **ArgoCD Operator**
  - **GitOps ZTP plugins** installed (`ztp-site-generate`, etc.)
- Bare metal hosts with BMC and PXE boot support
- Internet access OR preloaded images (for disconnected installs)

---

## ⚙️ How It Works

1. **Define Cluster in Git**
   - Use `SiteConfig` and `PolicyGenTemplate` to describe cluster layout, BMC credentials, MAC addresses, and DU/worker profiles.

2. **Generate CRs**
   - Use `ztp-site-generate` or `make` commands to generate manifests under `out/`.

3. **Deploy via ArgoCD**
   - ArgoCD picks up changes from Git and applies them to the hub cluster.
   - Provisioning begins automatically using BareMetalHost and Agent CRs.

4. **Monitor Progress**
   - Use `oc get bmh`, `oc get agent`, and `oc get managedcluster` to track the provisioning status.

5. **Add Day-2 Nodes (Optional)**
   - Modify `SiteConfig` to include additional nodes.
   - Push to Git — ArgoCD will trigger provisioning and apply updated policies.

---

## 🧪 Useful Commands

```bash
# Validate SiteConfig
oc get siteconfig -n ztp

# Check provisioning status
oc get bmh -A
oc get agent -A
oc get clusterdeployment -A

# Check for preprovisioning image status
oc get ppimg -A
