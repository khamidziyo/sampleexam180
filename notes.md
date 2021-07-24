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

## Creating application in different ways
1. Creating  an application based on an image, mysql, from Docker Hub, with the label set to db=mysql
```
oc new-app mysql --as-deployment-config \
> MYSQL_USER=user MYSQL_PASSWORD=pass MYSQL_DATABASE=testdb -l db=mysql
```
2. Creating an application based on an image from a private Docker image registry:
```
oc new-app --docker-image=myregistry.com/mycompany/myapp --name=myapp --as-deployment-config
```
3. Creating an application based on source code stored in a Git repository:
```
oc new-app https://github.com/openshift/ruby-hello-world --name=ruby-hello --as-deployment-config
```
4. Desribe information about template
```
oc describe template postgresql-ephemeral -n openshift
```

5. Create application from temmplate
```
oc new-app --template=postgresql-ephemeral -p POSTGRESQL_DATABASE=acmedb |
> -p POSTGRESQL_PASSWORD=redhat -p POSTGRESQL_USER=acmeadmin -p DATABASE_SERVICE_NAME=infodb -l app=info -l db=postgres
```


