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
expose route to the service
```
oc expose svc/infodb
oc get route
```

6. Create an application based on git repo. (Source to image)
```
oc new-app --as-deployment-config  php~https://github.com/openshift/sti-ruby.git#develop --context-dir=2.0 --name=php-helloworld 
```

7. Start a new build after changing source code
```
oc start-build php-helloworld
```

8. List all local available templates/images
```
oc new-app --list
```
9. List template parametres
```
oc process --parameters mysql-persistent -n openshift
```
10. Process template and create multi-container application
```
oc process \
> -f todo-template.json -p RHT_OCP4_QUAY_USER=${RHT_OCP4_QUAY_USER} \
> | oc create -f -
```

same result can be achieved using `oc new-app` command without processing the template first. Just wait little bit after exposing the service
```
oc new-app -f todo-template.json -p RHT_OCP4_QUAY_USER=${RHT_OCP4_QUAY_USER}
```
## Troubleshooting


1.  retrieve the logs from a build configuration
```
oc logs bc/<application-name>
```
2. If a build fails, after finding and fixing the issues, run the following command to request a new build:
```
oc start-build <application-name>
```
3. Deployment logs
```
oc logs dc/<application-name>
```

4. relax the OpenShift project security with the command `oc adm policy`. This's used when container user has permission issues
```
oc adm policy add-scc-to-user anyuid -z default
```

5. automated way to remove obsolete images and other resources.
```
oc adm prune
```

6. Temporarily access some of these missing commands is mounting the host binaries folders, such as /bin, /sbin, and /lib, as volumes inside the container
```
sudo podman run -it -v /bin:/bin image /bin/bash
```
