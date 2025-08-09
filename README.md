# Helm Parent–Child Charts Guide

This repository demonstrates how to structure a **parent chart** with **child charts** (dependencies) in Helm, how to configure their relationship, and how to set values for child charts from the parent chart.

---

## 1. Overview

In Helm:

- **Parent Chart** → The main chart you install. It can:
  - Deploy its own Kubernetes manifests.
  - Include one or more **child charts** as dependencies.
  - Pass configuration values to its child charts.
  
- **Child Chart (Subchart)** → A separate chart that the parent references or bundles. It is a fully functional chart with its own `Chart.yaml`, templates, and `values.yaml`.

---

## 2. How Values Flow from Parent to Child

When the parent chart needs to override values for a child chart:

1. The override must be **namespaced** under the child chart’s name (from its `Chart.yaml` → `name` field).
2. If an `alias` is used in the dependency declaration, the alias becomes the prefix instead of the chart’s name.
3. Helm merges values in the following order:
   - Defaults from the child chart’s `values.yaml`
   - Overrides from the parent chart’s `values.yaml` (under the correct prefix)
   - CLI `--values` or `--set` overrides

**Example:**

**Child Chart (`child1`) Default Values**
```yaml
replicaCount: 1
image:
  repository: nginx
  tag: latest
