---
title: Ubuntu 22.04 Install Docker & Docker Compose
date: 2025-09-25 11:15:35
tags:
 - Docker
 - Docker-Compose
 - Ubuntu 22.04
---

# Install Docker & Docker Compose on Ubuntu 22.04

## Update System

Open the terminal and run:

```bash
sudo apt update
sudo apt upgrade
sudo apt full-upgrade
```
---

## Add Docker Repository and Prerequisites

1. Install required packages and HTTPS support:

   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
   ```

2. Add Docker’s official GPG key:

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. Add Docker APT repository:

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

<!-- more -->
4. Update apt package index:

   ```bash
   sudo apt update
   ```

---

## Install Docker

### Install Latest Docker CE

```bash
sudo apt install docker-ce

# Additional
sudo apt install docker-ce-cli containerd.io docker-compose-plugin
```

Question: Why do I need to install the following packages: docker-ce-cli, containerd.io, docker-compose-plugin?

Answer: you’re actually installing **different components of Docker**, not just “Docker itself.”

#### 🔹 1. `docker-ce`

* **Docker Community Edition (CE)** – the core Docker engine (daemon).
* This is what actually runs containers in the background.

---

#### 🔹 2. `docker-ce-cli`

* The **command-line client** (`docker` command).
* Without this, you wouldn’t be able to run commands like:

  ```bash
  docker ps
  docker run
  docker build
  ```
* It talks to the Docker daemon (`dockerd`) and controls containers.

---

#### 🔹 3. `containerd.io`

* A **container runtime** that Docker uses under the hood.
* Think of it as the low-level tool that manages container execution, snapshots, images, etc.
* Docker relies on it for actually starting/stopping containers.

---

#### 🔹 4. `docker-compose-plugin`

* Provides the new **`docker compose`** command (note: **without the dash**).
* Example:

  ```bash
  docker compose up -d
  ```
* This replaces the old standalone `docker-compose` binary.
* It’s maintained directly by Docker, so it integrates better with the Docker CLI.

---

✅ **In short:**

* `docker-ce` = Docker engine (daemon)
* `docker-ce-cli` = CLI tool (`docker`)
* `containerd.io` = container runtime (engine under the engine)
* `docker-compose-plugin` = modern Compose integration

---

👉 You technically **could skip** some of these, but:

* Without `docker-ce-cli` → you can’t run Docker commands.
* Without `containerd.io` → the engine can’t launch containers.
* Without `docker-compose-plugin` → you won’t have Compose (unless you install the old binary).

### Install a Specific Version

To install a specific version, first list available versions:

```bash
apt-cache madison docker-ce
```

Example output:

```
docker-ce | 5:20.10.17~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
...
```

Choose a version, for example `5:20.10.16`:

```bash
sudo apt install docker-ce=5:20.10.16~3-0~ubuntu-jammy docker-ce-cli=5:20.10.16~3-0~ubuntu-jammy containerd.io
```

---

## Check Docker Status

```bash
systemctl status docker.service
```

Example output (key part):

```
docker.service - Docker Application Container Engine
Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
Active: active (running) since ...
...
```

If not running, start Docker:

```bash
sudo systemctl start docker
```

Enable auto-start at boot:

```bash
sudo systemctl enable docker
```

---

## Install Docker Compose (Standalone)

1. Download the binary (example: v2.18.1):

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. Apply execution permission:

   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```


## Run Docker Without `sudo`

By default, Docker commands require `sudo`. To allow your user to run Docker commands **without sudo**, add your user to the `docker` group:

```bash
sudo usermod -aG docker $USER
```

* Replace `$USER` with your actual username if needed, for example:

```bash
sudo usermod -aG docker user
```

* After running this command, **log out and log back in**, or restart the terminal/CLI to apply the changes.
