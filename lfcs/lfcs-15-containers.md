## Containers and Linux

From linux point of view, a container is an isolated process running with restricted resources. The Kernel provides namespaces and cgroups as supporting features. A chroot fake directory is ussed to access all the dependencies for the process. The container images is an archive of files which is provided in an standarized format (a tarball). The standarization is provided by the Open Container Innitiative (OCI) based on Docker.

Before Docker, native Linux containers (LXC) were used, later Docker was adopted along with Docker images, Docker registry, and Docker hub (a container registry for images). The Docker image format introduced a solution to build container images using common images as parent images which only add modifications in newer layers.

Docker started as open source, but has being movinf to closed source model. Red Hat provided podman as an alternative that is open source. Both Podman and Docker are OCI compliant.

Podman doesn't require a daemon or elevated priviledges, it allow rootless containers and it is open source. It is also fully compatible with Docker images.

## Container Images

The container start an application, and container images contains all dependencies required to start that application. Existen container images can be used to run containers. Images are provided trhough public and private image registries such as Docker Hub.

While runing containeers, the required images are fetched automatically and stored locally.

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `podman search nginx`                       | Searches for the `nginx` image in container registries.                                           |
| `podman pull docker.io/library/nginx`       | Pulls the `nginx` image from the Docker Hub registry.                                             |
| `podman images`                             | Lists all container images stored locally.                                                        |

## Running containers

All arguments should be provided before the container image name, as everything after the image name is considered a command for the container.

| Command                                    | Explanation                                                                                       |
|--------------------------------------------|---------------------------------------------------------------------------------------------------|
| `podman run -d --name=myweb nginx`         | Runs an `nginx` container in detached mode with the name `myweb`.                                 |
| `podman run -it busybox sh`                | Runs a `busybox` container interactively with a shell session (`sh`).                             |
| `podman ps`                                | Lists all running containers.                                                                     |
| `podman ps -a`                             | Lists all containers, including those that are stopped.                                           |
| `podman logs -l`                           | Displays the logs of the last created container.                                                  |
| `podman logs mydb`                         | Displays the logs of the container named `mydb`.                                                  |
| `podman exec -it myweb sh`                 | Starts an interactive shell session (`sh`) inside the running `myweb` container.                  |
| `podman inspect myweb \| less`              | Displays detailed information about the `myweb` container, with output paginated by `less`.       |
| `podman stop myweb`                        | Stops the running `myweb` container.                                                              |
| `podman rm myweb`                          | Removes the `myweb` container.                                                                    |

## Using storage, variables and ports

Container storage is ephemeral, meaning that a container creates a directory where all data is stored, and once container is terminated, the directory is deleted with all the content. You can provide presistent storage by bind-mount a directory on the host inside the container.

Applications in containers are accessed using port forwarding. The container application port is exposed in the host and the user access the host port. It guarantees that the container doesn't even need its own IP address. Priviledged host ports can only be used by a root container.

To use specific parameters in a container, you can use variables. You can pass environtment variables with `--env KEY=VALUE`.

| Command                                                          | Explanation                                                                                         |
|------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `podman run -v /home/student/files:/files:Z -it busybox sh`      | Runs a `busybox` container interactively with a shell session (`sh`), mounting `/home/student/files` to `/files` in the container with SELinux context (`:Z`). |
| `podman run -p 8080:80 -d nginx`                                 | Runs an `nginx` container in detached mode, mapping port 8080 on the host to port 80 in the container.  |
| `podman run --env key=value busybox env`                         | Runs a `busybox` container and sets an environment variable `key` to `value`, then executes the `env` command inside the container to display the environment variables. |
