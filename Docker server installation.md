Trilium can be run as docker image. This is useful for server deployments. 

Docker images are published on docker hub: https://hub.docker.com/r/zadam/trilium/

## Pull image

~~~~
docker pull zadam/trilium:[VERSION]
~~~~

Replace [VERSION] for actual latest version. It's not recommended to use "latest" tag.

## Run image

~~~~
sudo docker run -t -i -p 8080:8080 -v ~/trilium-data:/home/[myuser]/trilium-data zadam/trilium:latest
~~~~