# 📦 Dockerfile – Complete Guide (With Examples & Explanations)

This document explains **Dockerfile instructions**, **why they are used**, and **when to use them**, with a **complete sample Dockerfile**. This is interview-ready and DevOps‑friendly.

---

## 1️⃣ What is a Dockerfile?

A **Dockerfile** is a text file that contains a set of instructions used to **build a Docker image**. Each instruction creates a **layer**, and these layers together form a Docker image.

> 📌 Dockerfile is declarative – you describe *what you want*, Docker figures out *how to build it*.

---

## 2️⃣ Complete Sample Dockerfile (Using **ALL** Dockerfile Instructions)

Below is a **single sample Dockerfile** that demonstrates **every Dockerfile instruction you listed**. Some instructions (like `ONBUILD`) are situational and explained inline.

```dockerfile
# ==============================
# Base Image
# ==============================
FROM ubuntu:22.04

# ==============================
# Metadata (Modern replacement for MAINTAINER)
# ==============================
LABEL maintainer="hemanathan@example.com"
LABEL description="Dockerfile demonstrating all major instructions"

# (Deprecated but still asked in interviews)
MAINTAINER Hemanathan

# ==============================
# Build-time variable
# ==============================
ARG APP_VERSION=1.0

# ==============================
# Environment variables (runtime)
# ==============================
ENV APP_ENV=production \
    APP_VERSION=${APP_VERSION}

# ==============================
# Default shell
# ==============================
SHELL ["/bin/bash", "-c"]

# ==============================
# Set working directory
# ==============================
WORKDIR /app

# ==============================
# Install packages
# ==============================
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*

# ==============================
# Copy files
# ==============================
COPY app.sh /app/app.sh

# ADD (can extract archives or fetch URLs)
ADD sample.tar.gz /app/

# ==============================
# Create volume
# ==============================
VOLUME ["/app/data"]

# ==============================
# Expose port
# ==============================
EXPOSE 8080

# ==============================
# Create non-root user
# ==============================
RUN useradd -m appuser
USER appuser

# ==============================
# Stop signal
# ==============================
STOPSIGNAL SIGTERM

# ==============================
# Health check
# ==============================
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:8080 || exit 1

# ==============================
# Entrypoint & CMD
# ==============================
ENTRYPOINT ["/bin/bash"]
CMD ["app.sh"]

# ==============================
# ONBUILD (used by child images)
# ==============================
ONBUILD COPY . /app/src
```

---

dockerfile

# Base Image

FROM python:3.11-slim

# Metadata

LABEL maintainer="[hemanathan@example.com](mailto:hemanathan@example.com)"
LABEL application="sample-python-app"

# Environment Variables

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Working Directory

WORKDIR /app

# Copy dependency file first

COPY requirements.txt .

# Install dependencies

RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code

COPY . .

# Expose application port

EXPOSE 5000

# Create non-root user for security

RUN useradd -m appuser
USER appuser

# Health check

HEALTHCHECK --interval=30s --timeout=10s --retries=3 
CMD curl -f [http://localhost:5000/](http://localhost:5000/) || exit 1

# Default command

CMD ["python", "app.py"]

````

---

## 3️⃣ Dockerfile Instructions – Detailed Explanation

---

### 🔹 FROM

```dockerfile
FROM python:3.11-slim
````

**Purpose:**
Defines the **base image**.

**Why we use it:**

* Provides OS + runtime
* All images must start from a base

**When to use:**

* Always the **first instruction** (except `ARG`)

---

### 🔹 LABEL

```dockerfile
LABEL maintainer="hemanathan@example.com"
```

**Purpose:**
Adds metadata to the image.

**Why:**

* Ownership tracking
* Image documentation
* CI/CD visibility

**When to use:**

* Production & enterprise images

---

### 🔹 ENV

```dockerfile
ENV APP_ENV=production
```

**Purpose:**
Sets environment variables inside the container.

**Why:**

* Configuration without code changes
* Environment-specific values

**When to use:**

* App configs, paths, flags

---

### 🔹 WORKDIR

```dockerfile
WORKDIR /app
```

**Purpose:**
Sets the working directory.

**Why:**

* Avoids `cd` usage
* Cleaner Dockerfiles

**When to use:**

* Always (best practice)

---

### 🔹 COPY

```dockerfile
COPY . .
```

**Purpose:**
Copies files from host to container.

**Why:**

* Needed to bring application code

**When to use:**

* App source, configs, scripts

> ✅ Prefer `COPY` over `ADD`

---

### 🔹 RUN

```dockerfile
RUN apt-get update && apt-get install -y curl
```

**Purpose:**
Executes commands at **build time**.

**Why:**

* Install dependencies
* Configure OS

**When to use:**

* Anything that must exist in image

> ⚠ Each RUN creates a new layer

---

### 🔹 EXPOSE

```dockerfile
EXPOSE 5000
```

**Purpose:**
Documents application listening port.

**Why:**

* Helps developers & orchestration tools

**When to use:**

* Network-based applications

> ❗ Does NOT publish the port

---

### 🔹 USER

```dockerfile
USER appuser
```

**Purpose:**
Runs container as non-root user.

**Why:**

* Security best practice
* Prevents privilege escalation

**When to use:**

* Always in production

---

### 🔹 HEALTHCHECK

```dockerfile
HEALTHCHECK CMD curl -f http://localhost:5000/ || exit 1
```

**Purpose:**
Checks container health.

**Why:**

* Enables auto-restart
* Improves reliability

**When to use:**

* Microservices, production apps

---

### 🔹 CMD

```dockerfile
CMD ["python", "app.py"]
```

**Purpose:**
Defines default container command.

**Why:**

* Starts application

**When to use:**

* Exactly once per Dockerfile

---

## 4️⃣ CMD vs ENTRYPOINT (Interview Favorite)

| CMD               | ENTRYPOINT       |
| ----------------- | ---------------- |
| Default command   | Fixed command    |
| Can be overridden | Hard to override |
| Flexible          | Strict           |

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

## 5️⃣ Other Important Instructions

### 🔹 ARG

```dockerfile
ARG VERSION=1.0
```

* Used at build time only
* Not available at runtime

---

### 🔹 VOLUME

```dockerfile
VOLUME /data
```

* Persistent storage
* Data survives container restart

---

### 🔹 ADD (Use Carefully)

```dockerfile
ADD app.tar.gz /app
```

* Extracts archives
* Can download URLs

> ⚠ Prefer COPY unless ADD is required

---

## 6️⃣ Interview One‑Line Summary

> "A Dockerfile is a declarative file that defines how to build a Docker image using layered instructions such as FROM, RUN, COPY, CMD, and ENTRYPOINT."

---

## ✅ Best Practices

* Use minimal base images (`slim`, `alpine`)
* Use `.dockerignore`
* Run containers as non-root
* Use multi-stage builds
* Keep layers small

---

🎯 **This Markdown file is ready for:**

* GitHub README
* DevOps interview prep
* Notes & revision
* Project documentation
