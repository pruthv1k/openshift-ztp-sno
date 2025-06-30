# Below steps to be done for preparing the environment for upgrading the openshift cluster (x or y stream) using ZTP - GitOps Pipeline

This repository contains manifests and automation for upgrading a **Single Node Openshift Cluster** (x or y stream) using ZTP - GitOps Pipeline

## 🚀 Prerequisites
- Openshift Cluster is already built using GitOps Pipeline and the Git Application is in sync with the Lastest HEAD revision from git.
- Have access to internet to download and mirror the relevant platform and operator related images to quay mirror registry.
- Admin access to RHACM Hub cluster to emable the pipeline for upgrade.
- Topology Aware Lifecycle Operator - Required to perform cluster upgrade.
