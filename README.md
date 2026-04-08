# 🛡️ OpenShift OPA Gatekeeper: Cluster Admin Limits

[![OpenShift](https://img.shields.io/badge/OpenShift-4.x-red?logo=redhat)]
[![OPA Gatekeeper](https://img.shields.io/badge/OPA-Gatekeeper-blue?logo=openpolicyagent)]

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

## ⚙️ Deployment Steps & Configuration Files
1) Create the sync file to load the existing roles assigned.
Run the following command:
```text
oc apply -f sync/sync-config.yaml
```
**⏳ Wait a moment: Give Gatekeeper a few seconds to populate its cache before proceeding.**

2) Create the constraint template based on your needed users/groups, and it will create a CRD on your cluster to validate the incoming request.
Run the following commands:
```text
oc apply -f constraint-templates/template-users.yaml
oc apply -f constraint-templates/template-groups.yaml
```

3) Create the number of max users/groups need to be assigned.
Run the following commands:
```text
oc apply -f 3-constraints/constraint-users.yaml
oc apply -f 3-constraints/constraint-groups.yaml
```

## ⚠️ Important Note
OpenShift has built-in system users (like system:admin) that are technically cluster-admin. It will be counted in the user calculation by OPA Gatekeeper. Ensure you account for these default accounts when determining your maximum limit threshold.
