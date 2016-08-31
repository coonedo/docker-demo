Docker-concepts README
======================


Created by: Donal Cooney

Note: 	This demo follows the very basic introduction to the docker (See 
	README.txt) created purely as a tutorial for myself. Absolutely 
	no gaurantees whatsoever wrt these instructions and/or software.


Docker-concepts Summary:
------------------------

Starting point: You have followed the Docker-demo (README.txt in this repo), & 
you use that docker image as the basis here. basically explore a few of the 
docker commands & concepts (Docker swarm & kubernetes covered later / elsewhere)



1.	Update to the latest docker
2.	Create a docker group, add your user (avoid needing sudo)
3.	Start a container from the image we already have 
4. 	Copy a file to the running container
5.	Commit modified container to a new image
6.	Docker volumes (Persistent container data)

Instructions:
-------------

-	UPDATE TO THE LATEST DOCKER

	Follow the instructions at: 
	https://docs.docker.com/engine/installation/linux/ubuntulinux/


	In summary:

        - Update package information, ensure that APT works with the https
        method, and that CA certificates are installed.

        $ sudo apt-get update

        $ sudo apt-get install apt-transport-https ca-certificates



        - Add the new GPG key.

        $ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80
        --recv-keys 58118E89F3A912897C070ADBF76221572C52609D




        - Open the /etc/apt/sources.list.d/docker.list file or create
        Add: (For Ubuntu Trusty 14.04 (LTS) )
        deb https://apt.dockerproject.org/repo ubuntu-trusty main



        - Update your APT package index.
        $ sudo apt-get update

        - Install Docker.
        $ sudo apt-get install docker-engine


        - Check version:
        vagrant@docker:~$ sudo docker --versio
        Docker version 1.12.1, build 23cf638




-	CREATE A DOCKER GROUP, ADD YOUR USERID (AVOID HAVING TO SUDO)

	vagrant@docker:~$ sudo groupadd docker
	vagrant@docker:~$ sudo usermod -aG docker vagrant

	logout / login

	vagrant@docker:~$ docker ps
	Cannot connect to the Docker daemon. Is the docker daemon running on 
	this host?



	- stop / restart the docker daemon (when docker daemon starts, it
	will make the unix socker used by docker r/w by docker group)

	vagrant@docker:~$ sudo service docker stop
	docker stop/waiting

	vagrant@docker:~$ sudo service docker start
	docker start/running, process 21539




-	START A CONTAINER (FROM EXISTING IMAGE - SEE DOCKER-DEMO README.TXT)

	
vagrant@docker:~/docker-demo$ docker build .
Sending build context to Docker daemon 4.096 kB
Step 1 : FROM node:0.12
0.12: Pulling from library/node
357ea8c3d80b: Pull complete
52befadefd24: Pull complete
3c0732d5313c: Pull complete
855820c72656: Pull complete
c4c405806e0a: Pull complete
07beaad2461f: Pull complete
Digest:
sha256:6b5849274b25442e0f401607a8d676ae1a20cdf98801b6a85c71762a2d99f3b2
Status: Downloaded newer image for node:0.12
 ---> 39e0208edb6d
Step 2 : WORKDIR /app
 ---> Running in 46df5c66a681
 ---> f183a506632a
Removing intermediate container 46df5c66a681
Step 3 : ADD . /app
 ---> 0fa3e5d619ef
Removing intermediate container 82d857975f19
Step 4 : RUN npm install
 ---> Running in f4ce3f29c0ee
mysql@2.11.1 node_modules/mysql
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ sqlstring@2.0.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ bignumber.js@2.3.0
ÎáÎ÷ÎåÎáÎ÷ÎõÎáÎ÷Îõ readable-stream@1.1.14 (isarray@0.0.1, inherits@2.0.1,
string_decoder@0.10.3
1, core-util-is@1.0.2)

express@4.14.0 node_modules/express
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ escape-html@1.0.3
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ array-flatten@1.1.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ utils-merge@1.0.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ cookie-signature@1.0.6
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ merge-descriptors@1.0.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ methods@1.1.2
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ fresh@0.3.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ vary@1.1.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ path-to-regexp@0.1.7
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ range-parser@1.2.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ encodeurl@1.0.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ parseurl@1.3.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ content-type@1.0.2
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ etag@1.7.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ cookie@0.3.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ content-disposition@0.5.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ serve-static@1.11.1
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ depd@1.1.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ qs@6.2.0
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ on-finished@2.3.0 (ee-first@1.1.1)
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ finalhandler@0.5.0 (unpipe@1.0.0, statuses@1.3.0)
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ proxy-addr@1.1.2 (forwarded@0.1.0, ipaddr.js@1.1.1)
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ debug@2.2.0 (ms@0.7.1)
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ type-is@1.6.13 (media-typer@0.3.0, mime-types@2.1.11)
ÎáÎ÷ÎñÎáÎ÷ÎõÎáÎ÷Îõ accepts@1.3.3 (negotiator@0.6.1, mime-types@2.1.11)
ÎáÎ÷ÎåÎáÎ÷ÎõÎáÎ÷Îõ send@0.14.1 (destroy@1.0.4, ms@0.7.1, statuses@1.3.0,
mime@1.3.4, http-error
s@1.5.0)
 ---> 5ccb64762759
Removing intermediate container f4ce3f29c0ee
Step 5 : EXPOSE 3000
 ---> Running in 618993047cf3
 ---> 555efb8008ee
Removing intermediate container 618993047cf3
Step 6 : CMD node index.js
 ---> Running in 9aeddda58fd4
 ---> 0a20ac3c3e54
Removing intermediate container 9aeddda58fd4
Successfully built 0a20ac3c3e54
vagrant@docker:~/docker-demo$



	- Run the docker container

vagrant@docker:~/docker-demo$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED
SIZE
<none>              <none>              0a20ac3c3e54        4 minutes ago
647.5 MB
node                0.12                39e0208edb6d        3 days ago
642.6 MB
vagrant@docker:~/docker-demo$ docker run -p 8080:3000 -t 0a20ac3c3e54
Docker-demo app listening at http://:::3000





-	COPY A FILE TO THE RUNNING CONTAINER  (Image vs Container concepts ..)

	Some notes
	Docker images are the basis for containers.
	Each Docker image references a list of read-only layers that 
	represent filesystem differences. Layers are stacked on top of 
	each other to form a base for a container's root filesystem.


	When you create a new container, you add a new, thin, writable 
	layer on top of the underlying stack. This layer is often called the 
	“container layer”. All changes made to the running container - 
	such as writing new files, modifying existing files, and deleting 
	files - are written to this thin writable container layer. 


	So, IMAGES are read only, Containers are R/W


	We can copy a file to our running container - AND, then save (commit) 
	this modified container as a new image.

vagrant@docker:~$ docker cp testfile1.txt stoic_yalow:/testfile1.txt
vagrant@docker:~$
vagrant@docker:~$ docker diff stoic_yalow
A /testfile1.txt
vagrant@docker:~$



vagrant@docker:~$ docker commit stoic_yalow
sha256:cd9cc5cbdccc322abd33427a3b1e36edf0a51ae0dccbc17b6bf79a23546bb097
vagrant@docker:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED
SIZE
<none>              <none>              cd9cc5cbdccc        4 seconds ago
647.5 MB
node                0.12                39e0208edb6d        3 days ago
642.6 MB
vagrant@docker:~$






-	DOCKER VOLUMES

	A volume is a persistent data store

	docker run -d -P --name web -v /webapp training/webapp python app.py

	-d	Run container in background and print container ID
	-P	Publish all exposed ports to random ports
	--name	Assign a name to the container (in this case 'web')
	-v	Bind mount a volume (default []) (in this case /webapp)
	
vagrant@docker:~/docker-demo$ docker run -d -P --name web -v /webapp
training/webapp python app.py
Unable to find image 'training/webapp:latest' locally
latest: Pulling from training/webapp
e190868d63f8: Pull complete
909cd34c6fd7: Pull complete
0b9bfabab7c1: Pull complete
a3ed95caeb02: Pull complete
10bbbc0fc0ff: Pull complete
fca59b508e9f: Pull complete
e7ae2541b15b: Pull complete
9dd97ef58ce9: Pull complete
a4c1b0cb7af7: Pull complete
Digest:
sha256:06e9c1983bd6d5db5fba376ccd63bfa529e8d02f23d5079b8f74a616308fb11d
Status: Downloaded newer image for training/webapp:latest
e944e749952f302ea7dffa734a711d06eda68cc661f8a25f31e80f4750bf177f
vagrant@docker:~/docker-demo$


	We now have a python web application running -  with a volume mounted.


vagrant@docker:~/docker-demo$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED
STATUS              PORTS                     NAMES
e944e749952f        training/webapp     "python app.py"     11 minutes ago
Up 11 minutes       0.0.0.0:32768->5000/tcp   web
vagrant@docker:~/docker-demo$

	vagrant@docker:~/docker-demo$ curl localhost:32768
	Hello world!vagrant@docker:~/docker-demo$


	vagrant@docker:~/docker-demo$ docker inspect web
[
    {
        "Id":
"e944e749952f302ea7dffa734a711d06eda68cc661f8a25f31e80f4750bf177f"
,
        "Created": "2016-08-31T17:00:18.37798944Z",
        "Path": "python",
        "Args": [
            "app.py"
.
.
.
.
.
.
        "Mounts": [
            {
                "Name":
"38b695c1c8bae060ac2a384deccc2f1917961fc312b8137f00839e7
343ddfc5e",
                "Source":
"/var/lib/docker/volumes/38b695c1c8bae060ac2a384deccc2
f1917961fc312b8137f00839e7343ddfc5e/_data",
                "Destination": "/webapp",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],	



	Mounting a host directory (/src/webapp) as a host volume (/opt/webapp)

	docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp
python app.py


	
