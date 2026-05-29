# GitHub Actions Non-Production Reusable Pipeline

This workflow demonstrates how to trigger a reusable non-production deployment pipeline using GitHub Actions.

The workflow supports:

* Manual execution
* Runtime user inputs
* Environment selection
* Reusable workflows
* Concurrency control
* Secure permissions management

This type of workflow is commonly used in DevOps environments to deploy applications into:

* QA
* UAT
* SIT

environments using centralized CI/CD pipelines.

Instead of duplicating deployment logic across repositories, the actual deployment process is stored inside a reusable workflow and invoked using the `uses` keyword.

---
```

---

# Important Concepts

# workflow_dispatch

workflow_dispatch
```

Allows manual workflow execution from GitHub UI.

Useful for:

* Controlled deployments
* Release management
* Environment-specific deployments

---

# Runtime Inputs

inputs:
```

Collects user-provided values during workflow execution.

This workflow accepts:

* `version`
* `environment`

---

# version Input

version:
```

Represents:

* Git commit SHA
* Docker image tag
* Application version

Example:

```text id="m6v1qy"
a7b12cd
v1.2.0
```

---

# environment Input

environment:
```

Allows users to select deployment environment.

Options:

```text id="d5q7pn"
qa
uat
sit
```

Displayed as dropdown menu in GitHub UI.

---

# type: choice

type: choice
```

Creates selectable dropdown input.

Benefits:

* Prevents invalid values
* Standardizes deployment environments
* Improves deployment safety

---

# default

default: uat
```

Automatically pre-selects:

uat
```

when workflow starts.

---

# concurrency

concurrency:
```

Prevents multiple deployments from running simultaneously in same group.

---

## group
group: roboshop-catalogue-nonprod
```

Creates deployment execution lock.

Purpose:

* Prevent overlapping deployments
* Avoid deployment conflicts

---

## cancel-in-progress

cancel-in-progress: false
```

Behavior:

* Existing deployment continues
* New deployment waits in queue

If:

true
```

then:

* Older deployment gets cancelled
* Latest deployment starts immediately

---

# permissions

permissions:
```

Controls GitHub token access.

Follows:

Least Privilege Principle
```

for better security.

---

# Conditional Execution

if: github.ref == 'refs/heads/main'
```

Ensures workflow runs only for:

main branch
```

Prevents accidental deployments from feature branches.

---

# Reusable Workflows

uses:
```

Calls workflow from another repository.

Example:

uses: org/repo/.github/workflows/file.yaml@main
```

Benefits:

* Centralized CI/CD logic
* Reduced YAML duplication
* Easier maintenance

---

# Passing Inputs to Child Workflow

with:
```

Sends runtime values into reusable workflow.

Example:

environment: ${{ inputs.environment }}
```

---

# secrets: inherit

secrets: inherit
```

Passes all repository secrets into reusable workflow.

Examples:

* AWS credentials
* Docker credentials
* API tokens

Without this:

* Child workflow cannot access secrets

---

# Workflow Execution Flow

User manually starts workflow
          ↓
Enter version/SHA
          ↓
Select environment (qa/uat/sit)
          ↓
Branch validation
          ↓
Concurrency lock check
          ↓
Reusable workflow triggered
          ↓
Inputs passed to child workflow
          ↓
Secrets inherited
          ↓
Deployment pipeline executes

---

# Real DevOps Use Cases

## Environment-Based Deployments

Deploy applications to:

* QA
* UAT
* SIT

using same centralized pipeline.

---

## Release Deployments

Deploy specific:

* Git SHA
* Docker image tag
* Application version

through runtime input selection.

---

## Enterprise CI/CD Standardization

Reusable workflows help:

* Standardize deployments
* Enforce deployment policies
* Reduce duplicated YAML files

---

## Safe Deployment Management

Using:

* Branch restrictions
* Concurrency control
* Controlled environment selection

to improve deployment safety.

---

# Benefits of This Design

* Centralized CI/CD management
* Reusable deployment logic
* Environment standardization
* Reduced YAML duplication
* Better security
* Easier maintenance

---

# Why This Workflow Is Important

This workflow demonstrates advanced DevOps CI/CD patterns using:

* GitHub Actions
* Reusable workflows
* Runtime environment selection
* Deployment concurrency management
* Secure secret inheritance

These patterns are widely used in enterprise-grade deployment platforms.

---

# How to Run

1. Open GitHub repository
2. Go to Actions tab
3. Select `NON-PROD Pipeline`
4. Click `Run workflow`
5. Enter version/SHA
6. Select environment:

   * qa
   * uat
   * sit
7. Start workflow
8. Observe reusable deployment execution

---
