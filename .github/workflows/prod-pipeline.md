# GitHub Actions Reusable Workflow Example

This workflow demonstrates how to use reusable workflows in GitHub Actions.

Instead of writing the same CI/CD pipeline logic in multiple repositories, GitHub Actions allows workflows to be centralized and reused across projects using the `uses` keyword.

This approach is commonly used in DevOps organizations to:

* Standardize CI/CD pipelines
* Reduce duplicated YAML code
* Enforce deployment standards
* Maintain centralized pipeline logic

The workflow is manually triggered and accepts a version input (commit SHA or release version) which is passed to a reusable production pipeline workflow.

---

---

# Important Concepts

# Reusable Workflows

```yaml id="p7v4tx"
uses:
```

allows one workflow to call another workflow.

Example:

```yaml id="u2m8qy"
uses: org/repo/.github/workflows/pipeline.yaml@main
```

This enables:

* Centralized CI/CD logic
* Shared deployment pipelines
* Standardized automation

---

# workflow_dispatch

```yaml id="m5q1vn"
workflow_dispatch
```

Allows manual workflow execution from GitHub UI.

Used for:

* Production deployments
* Release pipelines
* Controlled infrastructure changes

---

# Inputs

```yaml id="t8v3px"
inputs:
```

Allows users to provide runtime values.

Example:

```yaml id="f4m9wa"
version
```

can represent:

* Git SHA
* Docker image tag
* Application version

---

# concurrency

```yaml id="x6q2tr"
concurrency:
```

Prevents multiple workflows from running simultaneously in same group.

---

## group

```yaml id="r3v8wp"
group: roboshop-catalogue-prod
```

Creates unique execution lock.

Purpose:

* Prevent duplicate deployments
* Avoid concurrent production releases

---

## cancel-in-progress

```yaml id="k1m7qx"
cancel-in-progress: false
```

Behavior:

* Existing workflow continues running
* New workflow waits in queue

If:

```yaml id="c9v4ty"
true
```

then:

* Older workflow gets cancelled
* Latest workflow starts immediately

---

# permissions

```yaml id="n4q8vz"
permissions:
```

Defines GitHub token access level.

This improves security by following:

```text id="w7m2pa"
Least Privilege Principle
```

---

## contents: read

```yaml id="b5v1xr"
contents: read
```

Allows:

* Repository checkout
* Read repository files

---

## statuses: write

```yaml id="q8m3ty"
statuses: write
```

Allows workflow to:

* Update commit statuses
* Report pipeline results

---

## security-events: read

```yaml id="z2v7qp"
security-events: read
```

Allows access to:

* Security scan results
* Code scanning alerts

---

# Conditional Execution

```yaml id="d6m9vx"
if: github.ref == 'refs/heads/main'
```

Ensures deployment runs only for:

```text id="f1q4pn"
main branch
```

This protects production environments from accidental deployments.

---

# Reusable Workflow Syntax

```yaml id="j7v2mt"
uses: daws-86s/workflows/.github/workflows/prod-pipeline.yaml@main
```

Breakdown:

```text id="x5m8wa"
organization/repository/path@branch
```

---

# with

```yaml id="p9v1qx"
with:
```

Passes inputs to reusable workflow.

Example:

```yaml id="u4m7ty"
version: ${{ inputs.version }}
```

---

# secrets: inherit

```yaml id="g3v8px"
secrets: inherit
```

Passes all current repository secrets to reusable workflow.

Examples:

* AWS credentials
* Docker credentials
* API tokens

Without this:

* Child workflow cannot access secrets

---

# Workflow Execution Flow

```text id="m8q2vn"
User manually starts workflow
          ↓
Enter version/SHA
          ↓
Workflow validates branch
          ↓
Concurrency lock check
          ↓
Reusable workflow triggered
          ↓
Inputs passed to child workflow
          ↓
Secrets inherited
          ↓
Production pipeline executes
```

---

# Real DevOps Use Cases

## Centralized CI/CD Pipelines

Used for:

* Standard deployment pipelines
* Shared testing workflows
* Enterprise DevOps platforms

---

## Multi-Repository Deployments

One reusable workflow can serve:

```text id="r5v9wa"
catalogue
user
cart
payment
shipping
```

services.

---

## Controlled Production Deployments

Using:

```yaml id="h2m6tx"
if: github.ref == 'refs/heads/main'
```

to restrict deployments.

---

## Version-Based Deployments

Using runtime inputs:

```yaml id="v7q1py"
version
```

to deploy specific releases.

---

# Benefits of Reusable Workflows

* Reduced YAML duplication
* Centralized pipeline maintenance
* Consistent deployment standards
* Easier updates across repositories
* Better DevOps governance

---

# Why This Workflow Is Important

This workflow demonstrates advanced CI/CD design patterns using:

* GitHub Actions
* Reusable workflows
* Concurrency control
* Secure permissions
* Branch protection logic

These concepts are widely used in enterprise DevOps environments to build scalable and maintainable automation platforms.

---

# How to Run

1. Open GitHub repository
2. Go to Actions tab
3. Select `NON-PROD Pipeline`
4. Click `Run workflow`
5. Enter version/SHA
6. Start workflow
7. Observe reusable workflow execution

---
