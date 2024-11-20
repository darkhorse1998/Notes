# Facts

1. `prune` would help delete unused docker resources like `docker container prune` would remove all stopped containers, `dokcer volume prune` would remove unused volumes, `dokcer network prune` would remove unused netowrks. `docker system prune` would delete all unused resources (not associated with any running container)
2. Docker volumes can be attached to one or more containers.
3. `docker run --cpu 2 --memory 1g ...` would limit the resources used by the container.
4. Docker multistage build helps in optimizing the image as only the dependencies in the last base image used is present in the image created. We can use a lightweight image as the last image to bring down the size of the final image while using bigger images before to do the heavy lifting like building and creating artifacts.
5. Docker network:
    * bridge: The default network driver. If you don't specify a driver, this is the type of network you are creating. Bridge networks are commonly used when your application runs in a container that needs to communicate with other containers on the same host. Containers are able to connect with each other through the private IP address but not through container names. Moreover, we would need port mapping because the host is isolated and we would need to map the ports so that the particular container application is accessible from the host.
    * host: Remove network isolation between the container and the Docker host, and use the host's networking directly.
    * overlay: Overlay networks connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes. This strategy removes the need to do OS-level routing.
    * ipvlan: IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration.
    * macvlan: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host's network stack.
    * none: Completely isolate a container from the host and other containers. none is not available for Swarm services.
6. CMD vs ENTRYPOINT: \
CMD: Sets the default command or parameters that will be executed when the container starts. However, these defaults can be overridden by specifying a command at the end of the docker run command. \
ENTRYPOINT: Configures the container to run as an executable, setting a fixed command that will be executed when the container starts. The command specified by ENTRYPOINT cannot be overridden by default. However, any parameters specified in the docker run command will be appended to the ENTRYPOINT command.
7. COPY vs ADD:
COPY: It can only copy files and directories from the local host system (the build context) into the Docker image. Cannot handle URLs. \
ADD: In addition to copying local files and directories, ADD can handle remote URLs and automatically extract archives (such as tar files).
8. In particular, the instructions RUN, COPY, ADD mostly contribute to the size of the final Docker image and always create layers. Some instructions, for example the ones that starts with LABEL, ENTRYPOINT, CMD directives, don't modify the filesystem since they just add metadata or configurations to the image, so they don't add any layers.