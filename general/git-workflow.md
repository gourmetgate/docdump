# Git Flow Workflow Documentation

## Overview

**Git Flow** is a branching model for Git that defines a structured workflow for managing features, releases, and hotfixes. It is especially suited for projects with scheduled releases and multiple developers.

The workflow is based on long-lived branches and supporting short-lived branches, each with a specific purpose.

## Branch Types

### 1. Main Branches

These branches always exist in the repository.

#### `main

* Represents **production-ready code**
* Every commit on `main` is a stable release
* Tagged with version numbers (e.g., `v1.0.0`)

#### `develop`

* Represents the **latest development state**
* Contains completed features that will go into the next release
* All feature branches are merged into `develop`

### 2. Supporting Branches

These branches are temporary and deleted after use.

#### `feature/*`

* Used to develop new features
* Created from: `develop`
* Merged into: `develop`
* Naming convention: `feature/{username}/{issue_id}_feature_name`

**Example**

```bash
git checkout develop
git checkout -b feature/kga/1_login_page
```

#### `release/*`

* Used to prepare a new production release
* Created from: `develop`
* Merged into: `main` and `develop`
* Naming convention: `release/version-number`

**Purpose**

* Version number updates
* Documentation updates

**Example**

```bash
git checkout develop
git checkout -b release/1.2.0
```

#### `hotfix/*`

* Used to quickly fix critical bugs in production
* Created from: `main`
* Merged into: `main` and `develop`
* Naming convention: `hotfix/{username}/{issue_id}_issue_description`

**Example**

```bash
git checkout main
git checkout -b hotfix/kga/1_fix-payment-bug
```

---

## Workflow Steps

### 1. Feature Development

1. Create a feature branch from `develop`
2. Implement the feature
3. Merge the feature branch back into `develop`
4. Delete the feature branch

### 2. Release Process

1. Create a release branch from `develop`
2. Perform final testing and fixes
3. Merge the release branch into `main`
4. Tag the release version
5. Merge the release branch back into `develop`
6. Delete the release branch

```bash
git checkout main
git merge release/1.2.0
git tag v1.2.0
```

### 3. Hotfix Process

1. Create a hotfix branch from `main`
2. Apply the fix
3. Merge the hotfix into `main`
4. Tag the new version
5. Merge the hotfix into `develop`
6. Delete the hotfix branch

---

## Versioning

Git Flow commonly follows **Semantic Versioning**:

```
MAJOR.MINOR.PATCH
```

* **MAJOR**: Breaking changes
* **MINOR**: New features
* **PATCH**: Bug fixes

## Advantages of Git Flow

* Clear separation between development and production
* Well-defined release process
* Suitable for large teams and long-term projects
* Easy rollback and hotfix handling




## Continuous Deployment (CD) Flow for `develop`

### Overview

The **Continuous Deployment (CD)** process is triggered by commits merged into the `develop` branch. This process ensures that every integrated change is automatically versioned and deployed to the **test environment**, allowing early validation and fast feedback.

---

### Trigger: Commit or Merge into `develop`

* A **feature branch** or **hotfix back-merge** is merged into `develop`
* The merge must pass:

    * Code review
    * Automated checks (unit tests, jasmin tests)

Once the commit is pushed to `develop`, the CD pipeline is automatically triggered.

---

### Version Bump Strategy

* Each successful merge into `develop` triggers an **automatic version bump**
* The version is considered a **pre-release or snapshot version**
* with the format: '1.1.1-1'

**Rules**

* No manual version changes on `develop`
* Version bump is handled by the CI/CD pipeline

### Deployment to Test System

If the build and tests succeed, the application is automatically deployed to the **test environment**.

**Test Environment Characteristics**

* Mirrors production configuration as closely as possible
* Used by QA, product owners, and developers
* Allows validation of new features and bug fixes

**Deployment Actions**

* Deploy version built from `develop`
* Update environment variables and configuration

### Summary Flow

```
Commit / Merge
      ↓
   develop
      ↓
Version Bump
      ↓
 Build & Test
      ↓
Deploy to Test System
```
