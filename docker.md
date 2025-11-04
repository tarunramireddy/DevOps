# 🖥️ Difference Between Virtual Machines and Docker Containers

## 🧱 Virtual Machines (VMs)
Virtual Machines run on a hypervisor, and **each VM includes a full operating system (OS)** along with the application and all required dependencies.

Example:  
On a single physical server, you may have:
- VM 1 → Ubuntu OS  
- VM 2 → Windows OS  

Each VM has:
✅ Its own OS kernel  
✅ Its own system libraries  
✅ Full isolation  

---

## 🐳 Docker (Containers)
Docker uses **containerization**, where the **host OS kernel is shared**.  
Instead of a full OS, a container only packages:
- The application
- Required dependencies
- Minimal system libraries

Result:
⚡ Lightweight  
⚡ Fast startup  
⚠️ Less isolation compared to VMs (shared OS kernel)

---

### 🔑 Key Differences

| Feature | Virtual Machines | Docker Containers |
|---------|------------------|-------------------|
| OS | Full OS per VM | Shared host OS kernel |
| Isolation | Strong (full OS) | Process-level isolation |
| Resource Usage | Heavy | Lightweight |
| Performance | Slower (more overhead) | Faster |
| Portability | Limited (depends on OS) | Highly portable |

---

📌 Reference:  
<https://github.com/iam-veeramalla/Docker-Zero-to-Hero>

---

# 🐳 Docker Architecture – Key Concepts

## 🔹 Docker Host
- The machine (local or remote) where Docker Engine is installed
- Can be physical or virtual

## 🔹 Docker Engine Components
| Component | Description |
|-----------|-------------|
| **Docker CLI** | Interface to run Docker commands (`docker run`, `docker ps`, etc.) |
| **Docker REST API** | Allows the CLI or external tools to communicate with the Daemon |
| **Docker Daemon (`dockerd`)** | Background service that manages containers, images, volumes, and networks |

---

## 🌐 Remote Docker Access

Yes — you can run Docker commands on a remote host.

```bash
docker -H tcp://<host-ip>:<port> <command>
