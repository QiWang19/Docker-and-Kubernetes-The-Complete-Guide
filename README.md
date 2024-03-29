# Docker-and-Kubernetes-The-Complete-Guide

/home/qiwan/redis-image/Dockerfile

```Dockerfile
# Use an existing docker image as base
FROM alpine
# Download and install a dependency
RUN apk add --update redis
RUN apk add --update gcc
# Tell the image what to do when it starts
# as a container
CMD ["redis-server"]
```

```
[qiwan@qiwan ~]$ mkdir redis-image
[qiwan@qiwan ~]$ cd redis-image/

[qiwan@qiwan redis-image]$ sudo docker build .
Sending build context to Docker daemon 2.048 kB
Step 1/3 : FROM alpine
 ---> 196d12cf6ab1
Step 2/3 : RUN apk add --update redis
 ---> Running in e5a11b1f262d
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/community/x86_64/APKINDEX.tar.gz
(1/1) Installing redis (4.0.11-r0)
Executing redis-4.0.11-r0.pre-install
Executing redis-4.0.11-r0.post-install
Executing busybox-1.28.4-r1.trigger
OK: 6 MiB in 14 packages
 ---> 3bb918f12019
Removing intermediate container e5a11b1f262d
Step 3/3 : CMD redis-server
 ---> Running in 3fc6d3081760
 ---> 3049f1170018
Removing intermediate container 3fc6d3081760
Successfully built 3049f1170018
[qiwan@qiwan redis-image]$ sudo docker run 304
1:C 07 Nov 06:05:02.037 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 07 Nov 06:05:02.037 # Redis version=4.0.11, bits=64, commit=bca38d14, modified=0, pid=1, just started
1:C 07 Nov 06:05:02.037 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 07 Nov 06:05:02.040 * Running mode=standalone, port=6379.
1:M 07 Nov 06:05:02.040 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 07 Nov 06:05:02.040 # Server initialized
1:M 07 Nov 06:05:02.040 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 07 Nov 06:05:02.040 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 07 Nov 06:05:02.040 * Ready to accept connections

```

tag image

```
[qiwan@qiwan redis-image]$ sudo docker build -t myreg/redis:gcc .
Sending build context to Docker daemon 2.048 kB
Step 1/4 : FROM alpine
 ---> 196d12cf6ab1
Step 2/4 : RUN apk add --update redis
 ---> Using cache
 ---> 3bb918f12019
Step 3/4 : RUN apk add --update gcc
 ---> Running in 16867b96be9c
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/community/x86_64/APKINDEX.tar.gz
(1/11) Installing binutils (2.30-r5)
(2/11) Installing gmp (6.1.2-r1)
(3/11) Installing isl (0.18-r0)
(4/11) Installing libgomp (6.4.0-r9)
(5/11) Installing libatomic (6.4.0-r9)
(6/11) Installing pkgconf (1.5.3-r0)
(7/11) Installing libgcc (6.4.0-r9)
(8/11) Installing mpfr3 (3.1.5-r1)
(9/11) Installing mpc1 (1.0.3-r1)
(10/11) Installing libstdc++ (6.4.0-r9)
(11/11) Installing gcc (6.4.0-r9)
Executing busybox-1.28.4-r1.trigger
OK: 90 MiB in 25 packages
 ---> abdc44939d26
Removing intermediate container 16867b96be9c
Step 4/4 : CMD redis-server
 ---> Running in dd8243785e2d
 ---> 10ae8e8f17ed
Removing intermediate container dd8243785e2d
Successfully built 10ae8e8f17ed

```

### Making Real Projects with Docker

base image alpine doesn't have npm installed
<https://hub.docker.com/_/node/>, find all the avaliable versions.

```bash
$ cd simpleweb/
[qiwan@qiwan simpleweb]$ sudo docker build .
Sending build context to Docker daemon 4.096 kB
Step 1/3 : FROM node:alpine
Trying to pull repository docker.io/library/node ...
sha256:324ccac1d7c4ddf5eb9f9ed5274c37c90965605b5eb68df0a67c6266837bfb79: Pulling from docker.io/library/node
4fe2ade4980c: Already exists
a3f62ee5351e: Pull complete
b8ee302f1a47: Pull complete
Digest: sha256:324ccac1d7c4ddf5eb9f9ed5274c37c90965605b5eb68df0a67c6266837bfb79
Status: Downloaded newer image for docker.io/node:alpine
 ---> 4b3c025f5508
Step 2/3 : RUN npm install
 ---> Running in e9f2450c6376
npm WARN saveError ENOENT: no such file or directory, open '/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/package.json'
npm WARN !invalid#2 No description
npm WARN !invalid#2 No repository field.
npm WARN !invalid#2 No README data
npm WARN !invalid#2 No license field.

up to date in 0.78s
found 0 vulnerabilities

 ---> a7ca6f1c9cf2
Removing intermediate container e9f2450c6376
Step 3/3 : CMD npm start
 ---> Running in a65c2c6c7d08
 ---> ef56fadad43b
Removing intermediate container a65c2c6c7d08
Successfully built ef56fadad43b
```

Deal with error `no package.json`
`package.json` should be avaliable before `npm install`
`copy ./ ./` instruction to copy local machine file relative to build context

notice and WARN! are fine, no issue

```shell
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN !invalid#2 No description
npm WARN !invalid#2 No repository field.
npm WARN !invalid#2 No license field.
```
tag image

```
[qiwan@qiwan simpleweb]$ sudo docker build -t qiwan/simpleweb .
Sending build context to Docker daemon 4.096 kB
Step 1/4 : FROM node:alpine
 ---> 4b3c025f5508
Step 2/4 : COPY ./ ./
 ---> Using cache
 ---> 250bb635e983
Step 3/4 : RUN npm install
 ---> Using cache
 ---> 139ace947c37
Step 4/4 : CMD npm start
 ---> Using cache
 ---> b70d40aab01c
Successfully built b70d40aab01c
```

start this new image and got the node server running

container port mapping

```
[qiwan@qiwan simpleweb]$ sudo docker run -p 8080:8080 qiwan/simpleweb

> @ start /
> node index.js

Listening on port 8080
```

Go to localhost:8080

specify working directory in Dockerfile.
`WORKDIR /usr/app` move all the files under simpleweb in host to /usr/app in container.

Docker compose
project `visit`
Used to start up multiple Docker containers at the same time. and connect them together.
![](/img/docker_compose_file.png)
create the docker compose yml file. will automatically create networking between the two containers.

```bash
[qiwan@qiwan visits]$ sudo docker-compose up
# will set up network first
Creating network "visits_default" with the default driver

# two services have been created
Creating visits_redis-server_1 ... done
Creating visits_node-app_1     ... done

# output of redis server and node server
redis-server_1  | 1:M 08 Nov 2018 21:55:45.237 * DB loaded from disk: 0.000 seconds
redis-server_1  | 1:M 08 Nov 2018 21:55:45.237 * Ready to accept connections
node-app_1      |
node-app_1      | > @ start /app
node-app_1      | > node index.js
node-app_1      |
node-app_1      | Listening on port 8081
```

Then go to localhost:4001

![](/img/visit_app.png)

stop docker compose containers `$ sudo docker-compose down`

```shell
[qiwan@qiwan visits]$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS     PORTS                    NAMES
6fe2f41cf5e3        redis               "docker-entrypoint..."   11 minutes ago      Up 9 seconds     6379/tcp                 visits_redis-server_1
2bd77084cfc8        visits_node-app     "npm start"              11 minutes ago      Up 9 seconds     0.0.0.0:4001->8081/tcp   visits_node-app_1
[qiwan@qiwan visits]$ sudo docker-compose down
Stopping visits_redis-server_1 ... done
Stopping visits_node-app_1     ... done
Removing visits_redis-server_1 ... done
Removing visits_node-app_1     ... done
Removing network visits_default
```

restart policy (use always most)
- "no"
- always
- failure
- on-failure
- unless-stopped
After adding the policy, run `$ sudo docker-compose up --build`

`$ sudo docker-compose ps` will look for yml file first, so must be run under that dir
```shell
[qiwan@qiwan visits]$ sudo docker-compose ps
  Name       Command    State    Ports
----------------------------------------
visits_no   npm start   Up      0.0.0.0:
de-app_1                        4001->80
                                81/tcp
visits_re   docker-en   Up      6379/tcp
dis-        trypoint.
server_1    sh redis
```

### Creating a Production-Grade Workflow

```
$ sudo npm install -g create-react-app
$ create-react-appfrontend
```

![](/img/npm_flow.png)

command to intereact with project

`Dockerfile.dev` for build locally, `Dockerfile` for production

Customize the name of dockerfile
```
$ sudo docker build -f Dockerfile.dev .
Removing intermediate container 213e107275b0
Successfully built 9c0fbec3c770
```

Build dependencies

`Sending build context to Docker daemon 236.2 MB` --- no need the  node_modules folder in local

```
$ sudo docker run -p 3000:3000 9c0
```

Propagate changes in source(source code) to the containers (do not rebuild images every time)
use volume -- use reference instead of copy
![](/img/volume_syntax.png)

`-v /app/node_modules` is a place holder
```
[qiwan@qiwan frontend]$ sudo docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app e43dd
```

Error: EACCES: permission denied, scandir '/app' in fedora

<https://www.udemy.com/docker-and-kubernetes-the-complete-guide/learn/v4/questions/5293976>
<https://stackoverflow.com/questions/44139279/docker-mounting-volume-with-permission-denied/44142648#44142648>
Error because SELinux

Use docker compose to simplify the command
- create docker compose file
- use Dockerfile.dev file to build images
  `context`: where the files for the images pulled from
  `dockerfile: Dockerfile.dev`
- run   `$ sudo docker-compose up`
- do not need the `COPY . .` in the Dockerfile

### Executing Tests
```
$ sudo docker build -f Dockerfile.dev .
```

```
$ sudo docker run 14c npm run test

# or
# get the access to the container input
$ sudo docker run -it 14c npm run test
```

Living update tests
- start container by dokcecompose and then change the test
- add tests service in docker-compose.yml, and run `$ sudo docker-compose up --build`, change test, it will automatically update.

Our terminal -> stdin of each process, but the stdout?
After `$ sudo docker-compose up --build`, find the id of running container with command `npm run start`

```
# get access to stdin of the primary process,
# not the one that runs the test suite
$ sudo docker attach 3dc
```
in a new terminal

```
$ sudo docker exec -it 3dc sh
/app # ps
PID   USER     TIME  COMMAND
    1 root      0:00 npm
   24 root      0:00 node /app/node_modules/.bin/react-scripts start
   31 root      0:07 node /app/node_modules/react-scripts/scripts/start.js
   71 root      0:00 sh
   78 root      0:00 ps
/app #
```

### npm build make production version Need for Nginx
Production Server - nginx
multi-step build process
![](/img/nginx.png)
Create Dockerfile /app/build
 - build phase

 ```docker
 FROM node:alpine as builder
 ```
 - run phase

 ```docker
 FROM nginx
 ```

 ```
 $ sudo docker build .

 Step 6/8 : RUN npm run build
 ---> Running in fcddb04f4cc9

 Step 7/8 : FROM nginx
 ---> dbfc48660aeb

 Successfully built cf7d2dd86c07

 $ sudo docker run -p 8080:80 cf7
 ```

 go to <http://localhost:8080/>

 ### Deploy the production  Github, Travis CI and AWS

 ![](/img/deploy.png)
 set up github
 set Travis CI - Travis CI yml file configuration  `.travis.yml`
 ![](/img/travis_flow.png)
 ![](/img/travis_docker.png)

AWS elastic beanstalk
 - create new application, web server environment
 - choose `docker` platform
 - use sample application

 ![](/img/aws_loadbalancer.png)
  beanstalk scale things
 Travis configurationt to deploy to AWS

.travis.yml

```yml
deploy:
  provider: elasticbeanstalk
  region: "us-east-2" # inside URL
  app: "docker-react" # app name
  env: "DockerReact-env"
  bucket_name: # aws use this s3 bucket to put the app zip file
  bucket_path: "docker-react"
  on:
    branch: master
```

set iam keys
go to travis-settings, set keys to environment variables.
Add `EXPOSE 80` in Dockerfile

### Build a Multi-Container Application

![](/img/single_container.png)

problems:
1. only ngnix server
2. img build both travis CI & push to AWS beanstalk

![](/img/app_flow.png)

- make dev Dockerfile for each, `React App`, `Express Server`, `Worker`
- Add Postgres as server

Nginx path routhing -- create a new nginx image(container)
Add `default.conf` file. Rout requests from pages to the different servers.

Add `Dockerfile.dev` for building new image

![](/img/multictrsetup_complexapp.png)

set up .travis.yml file, run tests, production version, push images to docker hub

config Amazon EB with multi spread Dockerfiles
  container documentation, Dockerrun.aws.json
![](/img/dockerfile_spread.png)

setup environment on ElasticBeanstalk

RDS Database creation- Database to use containers-Postgres, Redis provided by aws

Create VPC security groups

Set environment variables

![](/img/dbms_aws.png)

IAM keys for deployment, set it in travisCI

Add script in .travis.yml

### Kubernetes
![](/img/k8s_flow.png)

![](/img/minikube_local.png)

docker compose vs kubernetes compose

![](/img/docker_dif_k8s_compose.png)

![](/img/complexapp_travis_flow.png)

In local machine uses kubectl to communicate with master

#### start minikube

```
minikube start --vm-driver=kvm
```

#### Get the multi-client image running on our local Kubernates Cluster running as a container

Different object type in kubernetes

```yaml
<!-- Different api version has different object we can create -->
apiVersion: v1
<!-- the kind of object we want to create: Pod, Service, StatefulSet, ReplicaController. etc.-->
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
  spec:
    containers:
      - name: client
        image: qiwanredhat/multi-client
        ports:
          - containerPort: 3000
```
![](/img/minikube_node.png)
each node has a kube-proxy can get access to the node

Nodeport Service: only for development phase
Port: other Pod needs multi-client to get access to current pod
targetPort: access the current container(node VS. pod)
nodePort: type in brower

![](/img/cmd_configfile.png)
command to run the config file
```
simplel8s]$ kubectl apply -f client-pod.yaml
pod/client-pod created
```

```
simplel8s]$ kubectl apply -f client-node-port.yaml
service/client-node-port created
```

check status of created pod
```
[qiwan@qiwan simplel8s]$ kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
client-pod   1/1     Running   0          5m10s
```

get services

```
[qiwan@qiwan simplel8s]$ kubectl get services
NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
client-node-port   NodePort    10.102.185.195   <none>        3050:31515/TCP   3m23s
kubernetes         ClusterIP   10.96.0.1        <none>        443/TCP          14h
```

node is not accessed using `localhost`, need to ask minikube for its own ip address

```
[qiwan@qiwan simplel8s]$ minikube ip
192.168.42.19
```
goto `192.168.42.19:31515` in browser

Different object type in kubernetes

- make sure the image is on Dockerhub


##### create deployment object

deployment works with service object

```
<!-- cleaning -->
[qiwan@qiwan simplek8s]$ kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
client-pod   1/1     Running   26         18d
[qiwan@qiwan simplek8s]$ kubectl delete -f client-pod.yaml
pod "client-pod" deleted
[qiwan@qiwan simplek8s]$ kubectl get pods
No resources found.

<!-- create -->
[qiwan@qiwan simplek8s]$ kubectl apply -f client-deployment.yaml
deployment.apps/client-deployment created
[qiwan@qiwan simplek8s]$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
client-deployment-bd7b67d96-ksqtz   1/1     Running   0          6s

[qiwan@qiwan simplek8s]$ kubectl get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
client-deployment   1/1     1            1           2m17s
```

- get ip, go to `http://192.168.42.19:31515/`

```
[qiwan@qiwan simplek8s]$ minikube ip
There is a newer version of minikube available (v0.35.0).  Download it here:
https://github.com/kubernetes/minikube/releases/tag/v0.35.0

To disable this notification, run the following:
minikube config set WantUpdateNotification false
192.168.42.19
```

every pod has own ip internel 172.17.0.2

```
[qiwan@qiwan simplek8s]$ kubectl get pods -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
client-deployment-bd7b67d96-ksqtz   1/1     Running   0          20m   172.17.0.2   minikube   <none>           <none>
```

#### Update deployment images

- tag the image with a version number
- run a 'kubectl' command forcing the deployment to use the new image version
`kubectl set image deployment/client-deployment client=qiwanredhat/multi-client:v5`

#### let docker cli display host/containers inside virtual machine

eval $(minikube docker-env)

#### Architechture of fabbnaci app

![](/img/app_k8s_archi.png)

IngressService
ClusterIPService

#### k8s Path to Production

![](/img/app_k8s_product.png)

#### complex app recreate the deployiment with kubernetes
1. delete files do not needed with k8s

  ```
  .travis.yml
  docker-compose.yml
  Dockerrun.aws.json
  //foldre
  /nginx
  ```
1. set up config file (under /comples/k8s dir)

    clusterIP, NodePort - accessible by other parts in the project
    ![](/img/ip_port.png)
    Ingress - accessible by users

2. Test locally on minikube
3. Create a Github/Travis flow to build images and deploy
4. Deploy app to a cloud provider

can combine different single config files into one file. (not recommanded in this course)

#### The need for volumes with databases

Postgres writes data to file system. If the pod crashed the data disapeared.

**avoid different pods (2 replics pods of postgres database related with the same volume) getting access to same database volume**

volume (in container termnology) : mechanism allows containers to access a filesystem outside its own filesystem.

volume (in kubernetes): an object type, allow container to save data in pod level. Different from Docker voluem. if the pod crashed, the volume can disappeared.

it's different from what we want **Persistent Volume Claim**, **Persistent Volume**

Persistent Volume Claim: advertisement of the volume choices.
Persistent Volume: create the volume on the fly.

#### persistent volume claim

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
  <!-- can be used by only one node -->
    - ReadWriteOnce
  resources:
    requests:
    <!-- 2Gi space -->
      storage: 2Gi
```

for app on my computetr `kubectl get storageclass` hostpath: host machine hardrive, its default
for app on a cloud provider: different options: Google cloud persistent disk, Azure File..., can be automatically created. also can be customized.

#### reconfig the postgres deployment

#### ser environement variables

multi-worker: redis
multi-servier: redis, postgres

#### set postgres password

need new type of object `Secrets`. Use a imperative command to create secrect objcet.
```
kubectl create secret generic[secret type, tls(related with http, or docker registry)] <secret name> --from-literal [add the secret info in this command] PGPASSWORD=password123
```

config postgres-deployment, worker-deployment

#### load balancer

load balancer only provides access to one set of pods

but we need to expose both client and server pods

#### Ingress service

wire up ingress service

will use ingress-nginx, will not use kubernetes-ingress

set up of ingress-nginx changes depending on your environment (local, GC, AWS, Azure)

config routing rules for nginx