	1. docker pull
		○ This command downloads a Docker image from a registry (like Docker Hub) to your local machine.
		○ Example: docker pull ubuntu
		○ It only pulls the image; it does not run a container.

	2. docker run
		○ This command does two things:
			i. If the image is not present locally, it automatically pulls the image from Docker Hub.
			ii. It then creates and starts a container from that image.
		○ Example: docker run ubuntu
		○ If the image was already pulled earlier (using docker pull), it skips downloading and directly starts the container.
		
	3. docker run <image>:tag   (Docker Run with Image Tag (Version))
		a. docker run ubuntu      # pulls 'ubuntu:latest'
		b. docker run ubuntu:20.04
			§ ubuntu is the image name.
			§ 20.04 is the tag (version).
			§ This ensures you're running that specific version of Ubuntu.
			
	4. docker ps
		○ Lists all currently running containers.
		○ Displays container ID, image name, status, ports, and other runtime details.
		○ Example: docker ps
		
	5. docker ps -a
		○ Lists all containers, including those that are:
			§ Running
			§ Stopped
			§ Exited
			§ Created (but never started)
		○ Helpful for inspecting historical containers or debugging.
		○ Example: docker ps -a
		
	6. docker inspect
	📌 Purpose:
		Used to get detailed info (in JSON) about Docker containers, images, volumes, and networks.
		
		a. Syntax : docker inspect <object_name_or_id>
		✅ Examples:
			• Inspect a container: docker inspect mycontainer
			• Inspect an image: docker inspect ubuntu:20.04
			• Inspect a volume: docker inspect mysql_data
		
		🎯 Key Info You Get:
			• IP address
			• Mount points (volumes)
			• Environment variables
			• Entrypoint & CMD
			• Port mappings
			• Volume paths (e.g., /var/lib/docker/volumes/<name>/_data)
		
	7. docker stop
		• Gracefully stops a running container.
		• The container is not deleted; it just changes to a "stopped" state.
		• Can be restarted later using docker start.
		• Example: docker stop <container_id or name>
		
	8. docker rm
		• Removes (deletes) a container permanently from the system.
		• It only works on stopped containers. Running containers must be stopped first using docker stop.
		• Example: docker rm <container_id or name>
		
	Tip: To stop and remove a container in one line:
			i. docker rm -f <container_id or name>
				(The -f flag forces stop + remove.)
				
	9. docker images
		• Displays a list of all Docker images that are stored locally on the system.
		• Shows details such as:
			§ Repository (image name)
			§ Tag (version)
			§ Image ID
			§ Size
			§ Created date
		• Example: docker images

	8. docker rmi
		• Removes (deletes) one or more Docker images from the local system.
		• You must not have any containers (running or stopped) using the image, or it will throw an error (unless forced).
		• Example: docker rmi <image_id or repository:tag>

	Tip: Use -f to forcefully remove an image even if it’s being used:
		docker rmi -f <image_id>
		
🏷️ Assigning a Name to a Docker Container
By default, Docker assigns a random name to containers (like crazy_einstein). To give your container a custom, meaningful name, use the --name flag.

Example: docker run --name myubuntu -it ubuntu
	• --name myubuntu: Assigns the name myubuntu to the container.
	• -it: Runs the container in interactive mode with a TTY.
	• ubuntu: The base image being used.


Why docker run ubuntu Exits Immediately
When you run:
docker run ubuntu
	• Docker starts a new container from the ubuntu image.
	• Since no command is provided, Docker uses the image's default command, which is usually: /bin/bash
	• However, without an interactive terminal (-it), /bin/bash exits immediately because it's not attached to a user session or running any input/output.
As a result:
	• The container starts,
	• Executes the default command (/bin/bash),
	• Then exits immediately because there’s nothing to do — no task, no process running.

✅ How to Keep It Running Interactively

To keep the container running with a shell: docker run -it ubuntu
	• -i: interactive (keeps STDIN open)
	• -t: allocates a pseudo-TTY (terminal)
	This way, you get an interactive Ubuntu shell inside the container.
	
🕒 Keeping a Container Running: sleep
To run a container temporarily (e.g., for 10 seconds), you can use:
docker run ubuntu sleep 10
	• This starts an Ubuntu container and runs the sleep 10 command.
	• The container stays alive for 10 seconds, then exits automatically.
📝 Why this is useful:
	• Great for testing or debugging short-lived containers.

🔧 Running Commands in a Running Container: docker exec
To execute a command inside a running container, use:

docker exec <container_name_or_id> <command>
Example: docker exec myubuntu cat /etc/hosts

		Tip: For an interactive shell in a running container:
		docker exec -it myubuntu bash
		
Command	Behavior
docker run -d	Detached mode (runs in background)
docker run -it	Interactive mode (attached terminal)
docker attach <name>	Attach terminal to running container
CTRL + P and CTRL+ Q	Detach from container without stopping
docker run -a	Attach specific streams (advanced use)

-------------------------------------------------------------------------------
🌐 Port Mapping in Docker
When running a containerized app (like a web server), it exposes ports inside the container, but these aren't accessible outside by default. To make them available on your host machine, you use port mapping with the -p flag.

Syntax : docker run -p <host_port>:<container_port> <image>
	• host_port – Port on your local machine (outside the container).
	• container_port – Port inside the container that the app is listening on.
	
Example: docker run -p 8080:80 nginx
	• Runs an nginx container (which listens on port 80 inside).
	• Maps it to port 8080 on your local machine.
	• So you can access the app at:
👉 http://localhost:8080
	
In the command: docker run -p 80 nginx 
Docker implicitly assumes: docker run -p 80:80 nginx

-----------------------------------------------------------------------------------------------------------------
💾 Docker Volume Mapping with -v
📌 Syntax:

docker run -v <host_directory>:<container_directory> <image>
	• host_directory: Path on your local machine.
	• container_directory: Path inside the container where it will read/write files.

Example: docker run -v /backup/docker:/var/lib/mysql mysql

What this does:
	• Mounts your local folder /backup/docker to /var/lib/mysql inside the container.
	• MySQL stores its database files in /var/lib/mysql.
	• So now all data written by MySQL will persist to your local directory at /backup/docker.
	
🔄 Why Volume Mapping Is Useful:
	• Data persistence: Container data won’t be lost when the container stops or is deleted.
	• Backups: Easy to back up data by accessing the host directory.
	• Sharing: Share files between host and container.

📝 Tip:
You can mount read-only volumes like this:
docker run -v /backup/docker:/var/lib/mysql:ro mysql
(:ro = read-only access for the container)

---------------------------------------------------------------------------------------------------------------

🔄 Docker Storage: Bind Mounts vs Named Volumes
Docker provides two main ways to persist and share data between the host and containers:

🧷 1. Bind Mounts
✅ Syntax:
docker run -v /host/path:/container/path <image>

📌 Key Features:
	• You specify the exact path on the host.
	• Can be used to share config files, logs, or app source code.
	• Great for development environments where you want real-time file changes.
	
🔎 Example:
docker run -v /home/user/mysql_data:/var/lib/mysql mysql
	• All MySQL data is stored directly on your host in /home/user/mysql_data.

📦 2. Named Volumes
✅ Syntax:
docker volume create myvolume
docker run -v myvolume:/container/path <image>

📌 Key Features:
	• Docker manages the storage location (usually under /var/lib/docker/volumes/).
	• Ideal for production, where you don't need to know or manage exact paths.
	• Easier to backup, migrate, and manage via Docker CLI.
	• Automatically created if you run:
docker run -v myvolume:/data <image>
	
🔎 Example:
docker run -v mysql_data:/var/lib/mysql mysql
	• mysql_data is a named volume managed by Docker.




<img width="907" height="6485" alt="image" src="https://github.com/user-attachments/assets/4ebcd932-2466-4386-b40d-05dd5f4760fc" />
