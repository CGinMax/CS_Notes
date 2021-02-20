Docker
========================
`docker pull image-name[:version]` : pull the image repository.version can be latest mean new publish
`docker images` : check your docker images

`docker run image/container`:
1. Client input `docker run` command
2. Docker daemon not found the image in local
3. Download image from **Docker Hub**
4. Save the image to local
5. Docker daemon start the container.

`run option` :
+ **-i** :keep container open
+ **-t** :create terminal and connect to container standard output

`docker start container-name/container-id` : start the created container

`docker restart container-name/container-id` : restart the ran container

`docker stop container-name/container-id` : stop the running container

`docker rm container-name/container-id` : remove the container instance

`docker image rm image-name` : remove the image instance

`docker build -t repo-name/image-name:TAG .` : build the docker image from Dockerfile

`docker commit -a "author" -m "commit" <container-name/container-id> <hub-user>/<repo-name>[:<TAG>]` : like git commit, commit your container changes.
