## Managing images
1. Save current image to tar file
```
sudo podman save -o example.tar imagename:v1.0
sudo podman save -o mysql.tar registry.access.redhat.com/rhscl/mysql-57-rhel7:5.7
```
2. Load an image from .tar file
```
sudo podman load -i example.tar
```

3. Commit current container to an image
```
sudo podman commit --author="Hamidulla Ziyo" container-name/id registry.example.com:5000/mysql7:5.7
```

4. Identify which files were changed, created, or deleted since the container was started
```
sudo podman diff container-name/id
sudo podman diff mysql-basic
```
5. Tag image to distinguish from other releases
```
sudo podman tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG] 

sudo podman tag mysql-custom devops/mysql:snapshot
```

6. Remove tag from an image
```
sudo podman rmi devops/mysql:snapshot
```
7. Push the local image to registry
```
sudo podman push [OPTIONS] IMAGE [DESTINATION]
sudo podman push quay.io/bitnami/nginx
```

## Building images with Podman
```
sudo podman build -t NAME:TAG DIR
```

## New app from template on OCP
