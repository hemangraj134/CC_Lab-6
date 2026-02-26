# Automated CI/CD Deployment with Jenkins & NGINX Load Balancing

## Overview
This repository contains the infrastructure and pipeline code for automating the deployment of containerized backend applications. It demonstrates the use of **Jenkins Pipeline as Code** to build, deploy, and manage multiple Docker containers, while utilizing **NGINX** to distribute incoming traffic through various load-balancing algorithms.

## Tech Stack
* **CI/CD:** Jenkins (Declarative Pipeline)
* **Containerization:** Docker (Custom Network & Containers)
* **Reverse Proxy / Load Balancer:** NGINX
* **Version Control:** Git & GitHub
* **Backend:** C++ (Dockerized Application)

## Architecture
1. **Source Code Management:** Jenkins continuously polls this GitHub repository for changes.
2. **Build Stage:** Jenkins triggers a `docker build` to create the backend image from the application source code.
3. **Deployment Stage:** Jenkins spins up multiple isolated backend containers (`backend1`, `backend2`) within a custom Docker network (`app-network`).
4. **Load Balancing:** An NGINX container is deployed with a volume-mounted configuration file (`nginx/default.conf`) to proxy incoming HTTP requests on port 80 and distribute them to the backend instances.

## Load Balancing Strategies
This project implements and tests different NGINX routing algorithms:
* **Round-Robin (Default):** Distributes client requests sequentially across available servers to ensure an even load.
* **Least Connections (`least_conn;`):** Routes new requests to the backend server with the fewest active connections, preventing overload on busy nodes.
* **IP Hash (`ip_hash;`):** Ensures requests from the exact same client IP address are consistently routed to the same backend server, enabling basic session persistence.

## Pipeline Stages (`Jenkinsfile`)
* **Build Backend Image:** Cleans up legacy images and compiles the new backend Docker image.
* **Deploy Backend Containers:** Establishes the Docker bridge network and deploys the backend containers.
* **Deploy NGINX Load Balancer:** Deploys the NGINX reverse proxy, linking the local configuration file directly into the container using a read-only bind mount.
