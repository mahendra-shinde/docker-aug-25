# Container Instances and Images

## What is a Container Instance?
A **container instance** is a running environment created from a container image. It is an isolated process on your host machine that runs the application and its dependencies as defined in the image. Each container instance is lightweight, portable, and can be started, stopped, or deleted independently.

**Key Points:**
- Runs a single application or service.
- Isolated from other containers and the host OS.
- Created from a container image.
- Can be ephemeral (short-lived) or persistent.

## What is a Container Image?
A **container image** is a lightweight, standalone, and executable package that includes everything needed to run a piece of software: code, runtime, libraries, environment variables, and configuration files. Images are immutable and can be shared across environments.

**Key Points:**
- Blueprint for creating container instances.
- Built in layers, allowing for efficient storage and reuse.
- Can be versioned and tagged (e.g., `nginx:1.21`).

## Container Registry
A **container registry** is a centralized service for storing and distributing container images. Registries can be public (like Docker Hub) or private (like Azure Container Registry, Amazon ECR, or self-hosted solutions).

**Key Points:**
- Stores container images for sharing and deployment.
- Supports access control and image versioning.
- Enables automation in CI/CD pipelines.

## Container Repository
A **container repository** is a collection of related container images, usually for a specific application or service. Each repository can contain multiple versions (tags) of an image.

**Key Points:**
- Organizes images by project or application.
- Each repository can have multiple tags (e.g., `myapp:1.0`, `myapp:latest`).
- Found within a container registry.

---

## Example Workflow
1. **Build** a container image from your application code using a `Dockerfile`.
2. **Push** the image to a container registry (e.g., Docker Hub).
3. **Pull** the image from the registry to your host or cloud environment.
4. **Run** a container instance from the image.

---

## Visual Overview
```
[Source Code] → [Dockerfile] → [Image] → [Registry/Repository] → [Container Instance]
```

---

## Useful Commands
- `docker build -t myapp:1.0 .`  # Build an image
- `docker images`                # List images
- `docker run myapp:1.0`         # Run a container instance
- `docker push myapp:1.0`        # Push image to registry
- `docker pull myapp:1.0`        # Pull image from registry
