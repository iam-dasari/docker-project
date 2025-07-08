
Docker Image
------------------
- It's a light-weight,standalone and executable package that includes Code+Runtime+Libraries
  +Environment variables+configuration files
- Docker Images are immutable
- Docker Images are really portable

Docker Containers
---------------------------
- Running instances of images are called containers 
- Isolation
- Ephemeral
- Portable

How Docker Works
-------------------
![alt text](image-6.png)

Docker Commands
--------------------
- docker pull <image-id>
- docker run <image-id> (Pull+Create+Start) (-d-->detached mode)
- docker ps
- docker ps -a (or --all)
- docker stop <container-id>
- docker images
- docker rmi <Image-Id> (or <image-name>) (-f --> forcebly)
- docker rm <container-id>
- docker exec -it <container-id> /bin/bash (or --bash) (-it-->interactive mode)
- docker build -t dasaridevops/name:tag .
- docker run -p targetport:containerport dasaridevops/name:tag
- docker inspect <container_id_or_name>
- docker logs <container_id_or_name>
- docker login
- docker push <image-name>

Dockerize Applications
-------------------------
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)


Troubleshoot a Docker Container That Exits Immediately?
--------------------------------------------------------------------
When a Docker container exits immediately, it’s usually because:
  - The main process (PID 1) inside the container has terminated.
  - There was a misconfiguration, crash, or incorrect command in the Dockerfile or entrypoint.
1. Check the exit code - docker ps -a
  - Exit 0: The container completed successfully but had nothing to keep it alive.
  - Non-zero: An error or crash occurred.
2. Inspect the logs - docker logs <container_id_or_name>
Look for:
  - Python exceptions
  - Missing files or misconfigured entrypoints
  - Syntax errors
  - Missing environment variables
3. Run the container interactively - docker run -it --entrypoint /bin/sh <image_name>
This allows you to:
  - Inspect the filesystem
  - Check the Python environment
  - Manually run the start command
4. Check the CMD or ENTRYPOINT
Common issues:
  - No long-running process (e.g., Flask app isn’t started).
  - Syntax errors in CMD or ENTRYPOINT.
5. Check for required environment variables or files
If your app depends on:
  - .env files
  - Mounted volumes
  - Configuration values
  - Make sure they are present or injected properly.
6. Use docker inspect - docker inspect <container_id_or_name>
Look for:
  - Command used to start the container
  - Environment variables
  - Mounts and volumes
7. Add a sleep or tail command (for debugging only)
  CMD ["sleep", "3600"]
  CMD ["tail", "-f", "/dev/null"]

Common Causes
----------------------
![alt text](image-2.png)
>
 Docker Best Practices
 ---------------------------
 - Keep the image size low
 - Use official images
 - Use multi-stage builds
 - Don't run containers with root access
 - Keep the environment from code
 - Persist the logs
 - Maintain own network instead of default
 - Create Docker volumes
 - Optimize layers

 Docker Architecture
 ----------------------
 ![alt text](image.png)

 Docker Layers
 -----------------
 - Once docker image is created we cannot modify the image because docker images are immutable
 - Docker follows layering approach

 - FROM almalinux:8
 1st Instruction - It pulls the image and creates container out of this image. Then layer1 is created.
 Inside layer1 container, docker is running 2nd instruction.
 - RUN yum install nginx -y
 Now layer2 image is created
 layer2 container is created by docker engine
 In Layer2 container, layer3 instruction will run
 Now layer 3 image is created and that is final
 RUN echo "Hello">/opt/hello.html
 - These layers will be cached
 - If the instructions are more, build time is more and when docker push layers will be pushed to the container registry and memory is high

 ![alt text](image-1.png)

 How to optimize layers
 --------------------------
 - Club multiple instructions as single instruction
   Example: RUN yum install nginx -y && RUN echo "Hello">/opt/hello.html
 - Keep the frequently changing instructions at end so that previous layers will not get affected   
   and your build process is fast

Disadvantages of Docker
------------------------
 - What happens if docker server is crashed
    Application goes down
    You loose data also, docker volumes are inside the server
 - What happens if you suddenly get more traffic
    We have to run multiple containers
 - How to balance the lod if you have multiple cart containers
 - No self healing
 - No auto-scaling
 - What about configs and secrets