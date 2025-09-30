---
title: Ubuntu 24.04 Install Docker & Docker Compose
date: 2025-09-30 13:47:51
tags:
 - Docker
 - Docker-Compose
 - Ubuntu 24.04
---

I’m running an **Ubuntu 24.04 LTS (Noble)** server on an *AWS t3.large* instance, and I need Docker to manage containers for my applications. In this guide, I’ll walk through the exact steps to install Docker and Docker Compose on *Ubuntu 24.04*, configure it to start on boot, and allow it to run without sudo.

## Update System

Before installing, always update your package lists:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt full-upgrade -y
```

---

## Add Docker Repository and Prerequisites

<!-- more -->

1. Install required packages:

   ```bash
   sudo apt install -y apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
   ```

2. Add Docker’s official GPG key:

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
   sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. Add the Docker repository:

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
   https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. Update apt index again:

   ```bash
   sudo apt update
   ```

---

## Install Docker

### Install Latest Docker CE

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

✅ These packages include:

* **docker-ce** → The Docker engine (daemon).
* **docker-ce-cli** → The Docker CLI tool (`docker`).
* **containerd.io** → Container runtime used by Docker.
* **docker-compose-plugin** → Adds `docker compose` (modern Compose).

---

### (Optional) Install a Specific Version

List available versions:

```bash
apt-cache madison docker-ce
```

Install a specific version (replace version strings as needed):

```bash
sudo apt install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

---

## Enable & Start Docker

Check Docker status:

```bash
systemctl status docker
```

If not running, enable and start:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

## Run Docker Without `sudo`

To avoid typing `sudo` with every Docker command, add your user to the `docker` group:

```bash
sudo usermod -aG docker $USER
```

> ⚠️ Log out and back in (or reboot) for the group change to take effect.

Test:

```bash
docker ps -a
docker --version
```

Example:

```
Docker version 28.4.0, build d8eb465
```

---

## Install Standalone Docker Compose (Optional)

If you want the **standalone `docker-compose` binary** in addition to `docker compose`:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.28.1/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

Check version:

```bash
docker-compose --version
# Docker version 28.4.0, build d8eb465
```

