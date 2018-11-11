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

Propagate changes in source to the containers (do not rebuild images every time)  

use volume -- ise reference instead of copy


