Difference between VMs and Docker:
Virtual Machines (VMs):
VMs run on a hypervisor and each VM includes a full operating system (OS), along with the application and all its dependencies. For example, on a single physical server, you can have multiple VMs — one running Ubuntu, another running Windows, etc. Each VM is completely isolated and includes its own OS kernel, system libraries, and configuration.

Docker (Containers):
Docker uses containerization, which shares the host OS kernel. Instead of running a full OS for each application, containers include only the application and its dependencies (also some system dependencies). This makes them lightweight and fast to start. Docker containers are isolated at the process level, but they are not as strongly isolated as VMs since they share the host kernel.

Key Differences:
	• Isolation: VMs offer stronger isolation because each has its own OS. Containers are isolated but share the host OS kernel.
	• Overhead: VMs are heavier because of full OS replication. Containers are lighter and more efficient.
	• Performance: Containers typically perform better due to lower overhead.
	• Portability: Docker containers are more portable across environments, as long as the host OS supports Docker.
	


https://github.com/iam-veeramalla/Docker-Zero-to-Hero


🐳 Docker Architecture – Key Concepts
🔹 Docker Host
	• Machine (local or remote) where Docker Engine is installed.
	• Can be physical or virtual.
🔹 Docker Engine Components
	1. Docker CLI – Interface to run Docker commands like docker run, docker ps, etc.
	2. Docker REST API – Used by CLI (or tools) to interact with Docker Daemon.
	3. Docker Daemon (dockerd) – Core process that manages containers, images, volumes, and networks.

🌐 Remote Docker Access
✅ You can run Docker CLI from a remote machine:
	• Example: Laptop CLI connects to Desktop Docker Daemon via TCP/IP.
	• CLI uses REST API to talk to remote Daemon.
✅ Syntax for remote Docker CLI:

docker -H tcp://<host-ip>:<port> <command>
	• Example: docker -H tcp://192.168.1.100:2375 ps


🐳 PID Namespaces in Containers – Summary Notes
🔹 What is a Namespace?
	• A namespace in Linux isolates system resources for processes.
	• Docker uses several namespaces for isolation:
		○ PID – Process IDs
		○ NET – Network
		○ IPC – Inter-process communication
		○ UTS – Hostname/domainname
		○ MNT – Mount points (file system)
		○ USER – User and group IDs

🔹 PID Namespace (Process ID Isolation)
	• Each container gets its own PID namespace.
	• Inside the container:
		○ The first process (e.g., Nginx) is assigned PID 1.
		○ Other processes in the container are numbered accordingly (2, 3…).
	• From the host OS:
		○ These are just regular processes (e.g., PID 5001, 5002).
		○ The host can see all processes inside containers.
	• From inside the container:
		○ Only processes within that namespace are visible.
		○ It thinks it's a separate OS with its own process tree.

🔹 Example
Host OS View	Container View
PID 1 – init/systemd	PID 1 – nginx
PID 2 – sshd	PID 2 – bash
PID 5001 – nginx (container)	


🔹 Key Benefits
	• Isolation: Containers don’t see or interfere with host/other containers.
	• Security: Processes in a container can’t signal or inspect host processes.
	• Flexibility: Containers can manage their own process trees independently.


🔧 Docker Container Resource Limiting – Notes
	• Default Behavior:
		○ Containers can use unrestricted CPU and memory of the host machine.
		○ Without limits, a container can consume most of the system resources (even 90%+).
	• Why Limit Resources?
		○ Prevent one container from starving others.
		○ Improve system stability and performance.
		○ Useful in production/multi-container environments.
	• Limit CPU Usage:
docker run --cpus=0.5 ubuntu
		○ Limits the container to use 50% of a single CPU core.
	• Limit Memory Usage:
docker run --memory=100m ubuntu
		○ Restricts the container to 100 MB RAM.
	• ✅ These flags help in managing resource allocation efficiently.

<img width="992" height="3885" alt="image" src="https://github.com/user-attachments/assets/09223aaf-cc62-43a3-9847-183774989138" />
