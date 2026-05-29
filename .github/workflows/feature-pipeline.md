# GitHub Actions Feature and Development Pipeline

This workflow demonstrates how to use conditional reusable workflows in GitHub Actions for different branch-based deployment pipelines.

The workflow automatically decides which pipeline to execute based on the branch:

* Feature branches trigger the feature pipeline
* Main branch triggers the development pipeline

This is a very common CI/CD design pattern in DevOps environments where:

* Feature branches run lightweight validation/testing pipelines
* Main branch runs full deployment or integration pipelines

Instead of duplicating CI/CD logic, reusable workflows are used to centralize automation and maintain consistency across repositories.

---

  # --------------------------------------------------------
  # MAIN BRANCH DEV PIPELINE
  # --------------------------------------------------------

  dev-pipeline:

    # Execute only for main branch
    if: github.ref == 'refs/heads/main'

    # Call reusable development pipeline
    uses: daws-86s/workflows/.github/workflows/dev-nodejs-pipeline.@main

    # Inputs passed into reusable workflow
    with:

      # Project name
      project: roboshop

      # Application/service name
      component: catalogue

      # Deployment environment
      environment: dev

    # Pass repository secrets
    secrets: inherit
```

---

# Important Concepts

# Branch-Based Pipeline Execution

This workflow uses:

```
if:
```

conditions to execute different pipelines based on Git branch.

---

# Feature Branch Pipeline

```
if: github.ref != 'refs/heads/main'
```

This means:

* Any branch except `main`
* Executes feature pipeline

Examples:

```text
feature-login
feature-payment
bugfix-auth
dev
```

will trigger:

```text
feature-nodejs-pipeline.
```

---

# Main Branch Pipeline

```
if: github.ref == 'refs/heads/main'
```

This means:

* Only main branch triggers this pipeline

Main branch executes:

```text
dev-nodejs-pipeline.
```

---

# Reusable Workflows

```
uses:
```

allows one workflow to call another workflow.

Example:

```
uses: org/repo/.github/workflows/file.@main
```

Benefits:

* Centralized CI/CD logic
* Reduced  duplication
* Easier maintenance
* Standardized deployments

---

# with

```
with:
```

passes inputs into reusable workflow.

This workflow passes:

* project
* component
* environment

---

# secrets: inherit

```
secrets: inherit
```

passes all repository secrets into reusable workflow.

Examples:

* AWS credentials
* Docker credentials
* API tokens

Without this:

* Child workflow cannot access secrets

---

# permissions

```
permissions:
```

defines GitHub token access level.

This improves security by following:

```text
Least Privilege Principle
```

---

# Workflow Execution Flow

# Push to Feature Branch

```text
Developer pushes code → feature-login
               ↓
Workflow starts
               ↓
Feature pipeline condition = TRUE
               ↓
Feature reusable workflow executes
               ↓
Dev pipeline skipped
```

---

# Push to Main Branch

```text
Developer pushes code → main
               ↓
Workflow starts
               ↓
Feature pipeline skipped
               ↓
Dev pipeline condition = TRUE
               ↓
Development reusable workflow executes
```

---

# Real DevOps Use Cases

# Feature Pipelines

Used for:

* Pull request validation
* Unit testing
* Code quality checks
* Docker image builds

Feature pipelines are usually:

* Faster
* Lightweight
* Non-deployment focused

---

# Main Branch Pipelines

Used for:

* Integration testing
* Environment deployments
* Security scans
* Artifact publishing

Main branch pipelines are usually:

* More comprehensive
* Deployment-oriented

---

# Why Branch-Based Pipelines Are Important

Without branch conditions:

* Every branch may deploy applications
* Production safety decreases
* CI/CD pipelines become inefficient

Using branch-based workflows helps:

* Separate development stages
* Protect deployment environments
* Optimize CI/CD execution time
* Improve release management

---

# Benefits of Reusable Workflows

* Centralized pipeline management
* Easier updates
* Consistent DevOps standards
* Reduced duplicated  code
* Better maintainability

---

# Why This Workflow Is Important

This workflow demonstrates advanced CI/CD design patterns using:

* GitHub Actions
* Reusable workflows
* Conditional execution
* Branch-based automation
* Secure secret inheritance

These patterns are widely used in enterprise DevOps platforms for scalable CI/CD automation.

---

# How to Run

1. Push code to feature branch
2. Observe feature pipeline execution

OR

1. Push code to main branch

2. Observe development pipeline execution

3. Review workflow logs in GitHub Actions tab

---
