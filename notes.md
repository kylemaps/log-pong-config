# Log-Pong Application - Kubernetes Configuration

This repository serves as the single source of truth for the Kubernetes deployment of the **Log-Pong** application stack. It follows the GitOps methodology, and its state is managed by ArgoCD.

---

## Overview

This is a **Configuration Repository**. It contains only the Kubernetes manifests (`.yaml` files) required to run the entire application. It does **not** contain any application source code.

The application stack consists of two microservices:
1.  **`pingpong-app`**: A backend service that provides a counter via an API.
2.  **`log-output-app`**: A frontend service that consumes data from the `pingpong-app` and other sources.

The source code for these applications can be found in their respective repositories:
-   `pingpong-code`
-   `log-output-code`

## How It Works

1.  **ArgoCD Watches This Repo:** An ArgoCD instance is configured to continuously monitor the `main` branch of this repository.
2.  **CI Pipelines Push Updates:** When a new version of either the `pingpong-app` or `log-output-app` is built, the CI pipeline in its respective application repository will automatically commit a change to the manifests in this repository (e.g., updating a Docker image tag).
3.  **Automatic Sync:** ArgoCD detects the new commit, compares the desired state (from this repo) with the live state (in the Kubernetes cluster), and automatically syncs the changes, triggering a deployment of the new application version.

## Manual Changes

This repository is primarily intended to be updated by automated CI processes. Manual changes should be made with caution and typically only for permanent configuration adjustments (e.g., changing a resource limit, modifying a ConfigMap).

