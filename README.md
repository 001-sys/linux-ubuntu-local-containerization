# Automated Multi-Tier MongoDB Infrastructure

A production-ready, containerized database infrastructure stack built using Docker Compose. This project separates the core database backend from its administrative web UI using isolated bridge networking and persistent local storage volumes.



## 🚀 Key Features

* **Infrastructure as Code (IaC):** Entire multi-service environment deployed deterministically via a single configuration file.
* **Data Persistence:** Integrated Docker Named Volumes to guarantee complete data retention across container lifecycles.
* **Self-Healing Automation:** Includes a custom Bash script to handle safe, zero-downtime infrastructure cycles.

---

## 🛠️ The Architecture

The architecture consists of two distinct services decoupled on an isolated virtual network:
1.  **Backend:** MongoDB (`mongo:4.4`) acting as the persistent state engine.
2.  **Frontend:** Mongo Express (`latest`) acting as an isolated administrative web dashboard mapped to host port `8081`.

---

## 🔍 DevOps Troubleshooting Spotlight: The AVX Hurdle

### **The Challenge (Exit Code 139)**
During the initial deployment phase using the `latest` MongoDB image, the database container experienced an immediate runtime crash yielding an **Exit Code 139 (Segmentation Fault / SIGSEGV)**. 

### **The Root Cause Analysis**
Investigation of the hypervisor logs revealed that modern MongoDB engines (v7.0+) enforce a strict CPU requirement for **AVX (Advanced Vector Extensions)** instructions. Because this environment is hosted inside a virtualized guest OS (VirtualBox), the hypervisor was failing to pass down host AVX capabilities to the container runtime.

### **The Resolution**
The infrastructure configuration was updated to pin the database image version to **MongoDB 4.4**. This version represents the latest stable release branch that operates entirely independent of the AVX hardware instruction set, successfully stabilizing the stack without compromising database security or administrative functionality.

---

## 📦 Deployment Instructions

To spin up this infrastructure locally, ensure you have Docker and Docker Compose installed, then execute:

```bash
docker compose up -d
