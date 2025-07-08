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
 - RUN echo "Hello">/opt/hello.html

 These layers will be cached
 If the instructions are more, build time is more and when docker push layers will be pushed to the container registry and memory is high

 ![alt text](image-1.png)

 How to optimize layers
 --------------------------
 - Club multiple instructions as single instruction
   Example: RUN yum install nginx -y && RUN echo "Hello">/opt/hello.html
 - Keep the frequently changing instructions at end so that previous layers will not get affected   
   so that your build process is fast

Disadvantages of Docker
------------------------
 - What happens if docker server is crashed
    Application goes down
    You loose data also, docker volumes are inside the server
 - What happens if you suddenly get more traffic
    We have to run multiple containers
 - How to balance the lod if you have multiple cart containers
 - No self healing
 - What about configs and secrets