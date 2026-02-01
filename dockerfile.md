# 📦 Dockerfile – All Instructions Explained (Standard Format)

This document provides a **clean, consistently formatted, interview-ready explanation** of **all major Dockerfile instructions**, along with **what they do, when to use them, and why they exist**.

---

## 📌 What is a Dockerfile?

A **Dockerfile** is a declarative text file that contains a set of instructions used by Docker to build an image. Each instruction creates a **layer**, and these layers together form a Docker image.

---

## 📄 Sample Dockerfile (All Instructions)

```dockerfile
FROM ubuntu:22.04

LABEL maintainer="hemanathan@example.com"
LABEL description="Dockerfile with all major instructions"

MAINTAINER Hemanathan

ARG APP_VERSION=1.0

ENV APP_ENV=production \
    APP_VERSION=${APP_VERSION}

SHELL ["/bin/bash", "-c"]

WORKDIR /app

RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*

COPY app.sh /app/app.sh

ADD sample.tar.gz /app/

VOLUME ["/app/data"]

EXPOSE 8080

RUN useradd -m appuser
USER appuser

STOPSIGNAL SIGTERM

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:8080 || exit 1

ENTRYPOINT ["/bin/bash"]
CMD ["app.sh"]

ONBUILD COPY . /app/src
```

---

## 🔹 FROM

**Purpose:**
Creates a new build stage from a base image.

**When to use:**
Always — it is the foundation of every Docker image.

**Why it exists:**
Provides OS libraries and runtime dependencies.

---

## 🔹 LABEL

**Purpose:**
Adds metadata to a Docker image.

**When to use:**
For documentation, ownership, and CI/CD visibility.

**Why it exists:**
Helps identify, manage, and organize images.

---

## 🔹 MAINTAINER (Deprecated)

**Purpose:**
Specifies the author of the image.

**When to use:**
Not recommended in new images (use LABEL instead).

**Why it exists:**
Legacy method for ownership information.

---

## 🔹 ARG

**Purpose:**
Defines variables available only at build time.

**When to use:**
For versioning and build-time configuration.

**Why it exists:**
Allows parameterized and reusable Dockerfiles.

---

## 🔹 ENV

**Purpose:**
Sets environment variables inside the container.

**When to use:**
For runtime configuration values.

**Why it exists:**
Separates configuration from application code.

---

## 🔹 SHELL

**Purpose:**
Sets the default shell used for RUN instructions.

**When to use:**
When shell features differ from default `/bin/sh`.

**Why it exists:**
Provides flexibility for complex shell commands.

---

## 🔹 WORKDIR

**Purpose:**
Sets the working directory for subsequent instructions.

**When to use:**
Always — best practice.

**Why it exists:**
Improves readability and avoids repeated `cd` commands.

---

## 🔹 RUN

**Purpose:**
Executes commands during image build.

**When to use:**
To install packages or configure the image.

**Why it exists:**
Prepares the filesystem of the image.

---

## 🔹 COPY

**Purpose:**
Copies files and directories from host to image.

**When to use:**
For application source code and configs.

**Why it exists:**
Safely transfers local files into the image.

---

## 🔹 ADD

**Purpose:**
Copies files and can extract archives or fetch URLs.

**When to use:**
Only when extraction or remote download is needed.

**Why it exists:**
Provides extended file-handling capabilities.

---

## 🔹 VOLUME

**Purpose:**
Creates a mount point for persistent storage.

**When to use:**
When data must survive container restarts.

**Why it exists:**
Separates data from container lifecycle.

---

## 🔹 EXPOSE

**Purpose:**
Documents which ports the container listens on.

**When to use:**
For network-based applications.

**Why it exists:**
Improves communication with orchestration tools.

---

## 🔹 USER

**Purpose:**
Specifies the user to run the container.

**When to use:**
Always in production.

**Why it exists:**
Improves container security.

---

## 🔹 STOPSIGNAL

**Purpose:**
Defines the system signal sent to stop the container.

**When to use:**
For graceful shutdown handling.

**Why it exists:**
Prevents data corruption during container stop.

---

## 🔹 HEALTHCHECK

**Purpose:**
Checks whether a container is healthy.

**When to use:**
Production and microservice environments.

**Why it exists:**
Allows Docker/Kubernetes to manage failures.

---

## 🔹 ENTRYPOINT

**Purpose:**
Defines the main executable for the container.

**When to use:**
When the container should behave like a fixed command.

**Why it exists:**
Ensures consistent container behavior.

---

## 🔹 CMD

**Purpose:**
Provides default arguments or command.

**When to use:**
To define a default, overridable command.

**Why it exists:**
Adds flexibility to container execution.

---

## 🔹 ONBUILD

**Purpose:**
Defines instructions executed when the image is used as a base image.

**When to use:**
For base images or frameworks.

**Why it exists:**
Automates behavior in child images.

---

## ✅ Interview Summary

> A Dockerfile is a declarative blueprint used to build Docker images through layered instructions that define the environment, dependencies, configuration, and runtime behavior of a container.

---

## ⭐ Best Practices

* Prefer COPY over ADD
* Use non-root users
* Minimize image layers
* Use slim base images
* Use multi-stage builds in production
