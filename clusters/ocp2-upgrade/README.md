# Below steps to be done for preparing the environment for upgrading the openshift cluster (x or y stream) using ZTP - GitOps Pipeline

This repository contains manifests and automation for upgrading a **Single Node Openshift Cluster** (x or y stream) using ZTP - GitOps Pipeline

## 🚀 Prerequisites
- Openshift Cluster is already built using GitOps Pipeline and the Git Application is in sync with the Lastest HEAD revision from git.
- Have access to internet to download and mirror the relevant platform and operator related images to quay mirror registry.
- Admin access to RHACM Hub cluster to emable the pipeline for upgrade.
- Topology Aware Lifecycle Operator is installed on Hub cluster - Required to perform cluster upgrade.

## 😵‍💫 Patching the GitOps Operator to enable PolicyGenerator Plugin
https://docs.redhat.com/en/documentation/red_hat_advanced_cluster_management_for_kubernetes/2.13/html/gitops/gitops-overview#integrate-pol-gen-ocp-gitops
```
apiVersion: argoproj.io/v1beta1
kind: ArgoCD
metadata:
  name: openshift-gitops
  namespace: openshift-gitops
spec:
  kustomizeBuildOptions: --enable-alpha-plugins
  repo:
    env:
    - name: KUSTOMIZE_PLUGIN_HOME
      value: /etc/kustomize/plugin
    initContainers:
    - args:
      - -c
      - cp /policy-generator/PolicyGenerator-not-fips-compliant /policy-generator-tmp/PolicyGenerator
      command:
      - /bin/bash
      image: registry.redhat.io/rhacm2/multicluster-operators-subscription-rhel9:v<version>
      name: policy-generator-install
      volumeMounts:
      - mountPath: /policy-generator-tmp
        name: policy-generator
    volumeMounts:
    - mountPath: /etc/kustomize/plugin/policy.open-cluster-management.io/v1/policygenerator
      name: policy-generator
    volumes:
    - emptyDir: {}
      name: policy-generator
```
## Create a seperate ArgoCD app to target to upgrade the cluster

## 
