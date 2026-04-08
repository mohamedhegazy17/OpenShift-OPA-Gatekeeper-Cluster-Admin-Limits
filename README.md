# 🛡️ OpenShift OPA Gatekeeper: Cluster Admin Limits

[![OpenShift](https://img.shields.io/badge/OpenShift-4.x-red?logo=redhat)](https://www.openshift.com/)
[![OPA Gatekeeper](https://img.shields.io/badge/OPA-Gatekeeper-blue?logo=openpolicyagent)](https://openpolicyagent.github.io/gatekeeper/website/docs/)

This repository contains OpenShift-specific OPA Gatekeeper policies designed to strictly control and limit the number of `cluster-admin` assignments in your cluster. 

By utilizing Gatekeeper's data replication (sync) feature, these policies dynamically count existing `cluster-admin` bindings and prevent new ones from being created if they exceed your organization's defined threshold.

## 🚀 Overview

This repository provides two distinct policies:
1. **User Limit:** Restricts the maximum number of individual `User` subjects bound to `cluster-admin`.
2. **Group Limit:** Restricts the maximum number of `Group` subjects bound to `cluster-admin`.

## 📋 Prerequisites

Before deploying these policies, ensure you have:
* An active **OpenShift cluster** (v4.x) with cluster-admin privileges.
* The **OPA Gatekeeper Operator** installed from the OperatorHub.
* A running Gatekeeper instance (the `Gatekeeper` Custom Resource must be deployed so the webhook pods are running in the `openshift-gatekeeper-system` namespace).

---

## 📂 Suggested Repository Structure

To keep things organized, we recommend saving your YAML files in the following structure:

```text
├── 1-sync/
│   └── sync-config.yaml          # Enables data replication for RoleBindings
├── 2-templates/
│   ├── template-users.yaml       # Rego logic for counting users
│   └── template-groups.yaml      # Rego logic for counting groups
└── 3-constraints/
    ├── constraint-users.yaml     # Enforces the User limit (e.g., 10)
    └── constraint-groups.yaml    # Enforces the Group limit (e.g., 3)
```
## ⚙️ Deployment Steps & Configuration Files
Step One
Create the sync file to load the existing roles assigned. Save the following as 1-sync/sync-config.yaml:
