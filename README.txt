Docker-demo README
==================


Created by: Donal Cooney

Note: 	This demo is a very basic introduction to the topic named above created 
	purely as a tutorial for myself. Absolutely no gaurantees whatsoever 
	wrt these instructions and/or software.


Docker-demo Summary:
--------------------

1. 	Environment (Assumed installed/created already): Docker-demo runs on 
	Windows PC using Oracle VirtualBox managed by Vagrant. Essentially 
	Unbuntu running virtualised on Windows PC.
2. 	Creates/runs 1 docker  image (nodeJS application)







Instructions:
-------------

-	START DEMO
- 	Windoze: Create a directory 'docker_project'

- 	Windoze: From dir 'docker_project', create a file 'Vagrantfile' with 
	the following contents (ref: https://www.vagrantup.com/docs/)

	Vagrant.configure(2) do |config|
	  config.vm.define "docker" do |docker|
	    docker.vm.box = "ubuntu/trusty64"
	    docker.vm.network "private_network", ip: "192.168.0.244"
	    docker.vm.hostname = "docker.demo.com"
	    docker.vm.provider "virtualbox" do |vb|
	      vb.memory = 2048
	      vb.cpus = 2
	    end
	  end
	end

OR

-- Clone the following repo from my github

	https://github.com/coonedo/docker-demo


- 	Windoze: From dir 'docker_project', run the following command: 
	'vagrant up'
	C:\Users\coonedo\docker_project>vagrant up

- 	Windoze: From dir 'docker_project' check the status of your vm:
	C:\Users\coonedo\docker_project>vagrant status
	Current machine states:

	docker                    running (virtualbox)

	The VM is running. To stop this VM, you can run `vagrant halt` to
	shut it down forcefully, or you can run `vagrant suspend` to simply
	suspend the virtual machine. In either case, to restart it again,
	simply run `vagrant up`.

- 	Windoze: From dir 'docker_project', ssh to the docker vm
	C:\Users\coonedo\docker_project>vagrant ssh docker
	(note: you will need ssh installed on your windoze box. 
	I have git installed, which includeds ssh. 
	Also, vagrant up will generate a keypair + insert the public key 
	within the guest)

- 	** Next cmds are from the vm (linux) **
- 	install docker
	sudo apt-get install docker.io

- 	create a dir "docker-demo" and copy the follow files to that directory 
	(previously pulled from git to PC)
	vagrant@docker:~$ mkdir docker-demo
	vagrant@docker:~$ cd docker-demo/
	vagrant@docker:~/docker-demo$ cp /vagrant/docker-demo/index.js .
	vagrant@docker:~/docker-demo$ cp /vagrant/docker-demo/package.json
	vagrant@docker:~/docker-demo$ cp /vagrant/docker-demo/Dockerfile .

- 	Build the docker image 
	vagrant@docker:~/docker-demo$ sudo docker build .

- 	Run the docker image
	vagrant@docker:~/docker-demo$ sudo docker run -p 8080:3000 -t 
	17653fe98f52
	Example app listening at http://:::3000

- 	Use browser or curl to test the app is running
	vagrant@docker:~/docker-demo$ curl 192.168.0.244:8080
	Hello World!vagrant@docker:~/docker-demo$


-	END DEMO



Additional Notes
----------------



-	TERMINOLOGY/CONCEPTS
	Ok, some terminology ;-) At the end of above demo, we have a single 
	container running (this is a runtime instance of a docker image)
	Based on the demo above, we have



	DOCKER IMAGE/CONTAINER INSTANCE
	1 docker image:
	vagrant@docker:~/docker-demo$ sudo docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED
	VIRTUAL SIZE
	<none>              <none>              17653fe98f52        About an hour ago
	646.5 MB

	

	1 container instance:
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS              PORTS                    NAMES
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   7 seconds ago
     	Up 6 seconds        0.0.0.0:8080->3000/tcp   mad_hopper




	RUNNING MULTIPLE CONTAINER INSTANCES
	You can run multiple container instances from a single docker image
	Fire up a second container (note need to map the container port (3000) 
	to a different port on the host machine. in this case, we have already 
	used 8080, now use 8081
	vagrant@docker:~/docker-demo$ sudo docker run -p 8081:3000 -t 17653fe98f52
	Example app listening at http://:::3000

	We now have 2 containers running.
	vagrant@docker:~/docker-demo$ curl 192.168.0.244:8080
	Hello World!vagrant@docker:~/docker-demo$
	vagrant@docker:~/docker-demo$ curl 192.168.0.244:8081
	Hello World!vagrant@docker:~/docker-demo$
	vagrant@docker:~/docker-demo$

	2 container instances:
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS              PORTS                    NAMES
	8d1ea00de3a9        17653fe98f52:latest   "/bin/sh -c 'node in   4 minutes ago
     	Up 4 minutes        0.0.0.0:8081->3000/tcp   loving_fermat
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   17 minutes ago
     	Up 17 minutes       0.0.0.0:8080->3000/tcp   mad_hopper
	vagrant@docker:~/docker-demo$



	START/STOP/PAUSE/REMOVE CONTAINERS & REMOVE IMAGES 

	Pause a running container:

	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS              PORTS                    NAMES
	8d1ea00de3a9        17653fe98f52:latest   "/bin/sh -c 'node in   41 minutes ago
     	Up 41 minutes       0.0.0.0:8081->3000/tcp   loving_fermat
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   54 minutes ago
     	Up 54 minutes       0.0.0.0:8080->3000/tcp   mad_hopper
	vagrant@docker:~/docker-demo$ sudo docker pause 8d1ea00de3a9
	8d1ea00de3a9
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS                   PORTS                    NAMES
	8d1ea00de3a9        17653fe98f52:latest   "/bin/sh -c 'node in   42 minutes ago
     	Up 42 minutes (Paused)   0.0.0.0:8081->3000/tcp   loving_fermat
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   55 minutes ago
     	Up 55 minutes            0.0.0.0:8080->3000/tcp   mad_hopper
	vagrant@docker:~/docker-demo$


	Unpause a running container:
	vagrant@docker:~/docker-demo$ sudo docker unpause 8d1ea00de3a9
	8d1ea00de3a9
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS              PORTS                    NAMES
	8d1ea00de3a9        17653fe98f52:latest   "/bin/sh -c 'node in   44 minutes ago
     	Up 44 minutes       0.0.0.0:8081->3000/tcp   loving_fermat
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   58 minutes ago
     	Up 58 minutes       0.0.0.0:8080->3000/tcp   mad_hopper
	vagrant@docker:~/docker-demo$



	STOP BOTH CONTAINERS & THEN REMOVE DOCKER IMAGE
	vagrant@docker:~/docker-demo$ sudo docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED
	VIRTUAL SIZE
	<none>              <none>              17653fe98f52        2 hours ago
	646.5 MB
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE                 COMMAND                CREATED
     	STATUS              PORTS                    NAMES
	8d1ea00de3a9        17653fe98f52:latest   "/bin/sh -c 'node in   50 minutes ago
     	Up 50 minutes       0.0.0.0:8081->3000/tcp   loving_fermat
	6d72eb9d3955        17653fe98f52:latest   "/bin/sh -c 'node in   About an hour a
	go   Up About an hour    0.0.0.0:8080->3000/tcp   mad_hopper
	vagrant@docker:~/docker-demo$ sudo docker stop 8d1ea00de3a9
	8d1ea00de3a9
											vagrant@docker:~/docker-demo$ sudo docker stop 6d72eb9d3955
	6d72eb9d3955
	vagrant@docker:~/docker-demo$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED
	STATUS              PORTS               NAMES
	vagrant@docker:~/docker-demo$ sudo docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED
	VIRTUAL SIZE
	<none>              <none>              17653fe98f52        2 hours ago
	646.5 MB
	vagrant@docker:~/docker-demo$ sudo docker rmi 17653fe98f52
	Error response from daemon: Conflict, cannot delete 17653fe98f52 because the con
	tainer 8d1ea00de3a9 is using it, use -f to force
	FATA[0000] Error: failed to remove one or more images
	vagrant@docker:~/docker-demo$

	***** Not sure why, I needed to use the -f option. there seems to be 
	a 'residual container' hanging around that does not show up on docker 
	ps. *******

i




	STOP / REMOVE multiple containers

	One liner to stop / remove all of Docker containers:

	docker stop $(docker ps -a -q)
	docker rm $(docker ps -a -q)

	or

	docker rm -f $(docker ps -a -q)

	Same for images ..

	docker rmi $(docker images -q)





	- Dockerfile

	FROM node:0.12
	WORKDIR /app
	ADD . /app
	RUN npm install
	EXPOSE 3000
	CMD node index.js






        FROM node:0.12		FROM is always the first Dockerfile cmd. 
				Retrieve node from dockerhub. This sets the
				base image for subsequent instructions
        
	WORKDIR /app		Set the woring  directory for next cmds
        
	ADD . /app		Add files in currecnt dir (ie. Dockerfile
				index.js, package.json) to WORKDIR		
        
	RUN npm install		Run this cmd in the WORKDIR (ie. run npm
				install. npm is node package manager)
        
	EXPOSE 3000		The EXPOSE instruction informs Docker that 
				the container listens on the specified 
				network ports at runtime.
				EXPOSE does not make the ports of the
				container accessible to the host. To do that, 
				you must use either the -p flag to publish a 
				range of ports or the -P flag to publish all 
				of the exposed ports. You can expose one port 
				number and publish it externally under another
				number (See above where we map the port to a 
				diff prt for each container we fire up 8080 &
				8081) 

        CMD node index.js	Execute index.js under node (ie. run cmd
				'node' with parameter 'index.js'
				note: only 1 cmd per Dockerfile












	SHOW DOCKER CONTAINER or IMAGE DETAILS

Inspect Docker Image

vagrant@docker:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED
VIRTUAL SIZE
<none>              <none>              17653fe98f52        4 days ago
646.5 MB
vagrant@docker:~$ sudo docker inspect 17653fe98f52
[{
    "Architecture": "amd64",
    "Author": "",
    "Comment": "",
    "Config": {
        "AttachStderr": false,
        "AttachStdin": false,
        "AttachStdout": false,
        "Cmd": [
            "/bin/sh",
            "-c",
            "node index.js"
        ],
        "CpuShares": 0,
        "Cpuset": "",
        "Domainname": "",
        "Entrypoint": null,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",

            "NODE_VERSION=0.12.15"
        ],
        "ExposedPorts": {
            "3000/tcp": {}
        },
        "Hostname": "2da0903ff372",
        "Image":
"cd2d84f556b7179a55502d49d17707bf0f72478ce6bbd9b31d54d57b906332
09",
        "Labels": {},
        "MacAddress": "",
        "Memory": 0,
        "MemorySwap": 0,
        "NetworkDisabled": false,
        "OnBuild": [],
        "OpenStdin": false,
        "PortSpecs": null,
        "StdinOnce": false,
        "Tty": false,
        "User": "",
        "Volumes": null,
        "WorkingDir": "/app"
    },
    "Container":
"14a99a0b8d75cc7c43d583f5cd297171699899680eda7c7373545c6c4d2899
13",
    "ContainerConfig": {
        "AttachStderr": false,
        "AttachStdin": false,
        "AttachStdout": false,
        "Cmd": [
            "/bin/sh",
            "-c",
            "#(nop) CMD [\"/bin/sh\" \"-c\" \"node index.js\"]"
        ],
        "CpuShares": 0,
        "Cpuset": "",
        "Domainname": "",
        "Entrypoint": null,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",

            "NODE_VERSION=0.12.15"
        ],
        "ExposedPorts": {
            "3000/tcp": {}
        },
        "Hostname": "2da0903ff372",
        "Image":
"cd2d84f556b7179a55502d49d17707bf0f72478ce6bbd9b31d54d57b906332
09",
        "Labels": {},
        "MacAddress": "",
        "Memory": 0,
        "MemorySwap": 0,
        "NetworkDisabled": false,
        "OnBuild": [],
        "OpenStdin": false,
        "PortSpecs": null,
        "StdinOnce": false,
        "Tty": false,
        "User": "",
        "Volumes": null,
        "WorkingDir": "/app"
    },
    "Created": "2016-08-24T13:39:39.410512783Z",
    "DockerVersion": "1.6.2",
    "Id": "17653fe98f52973ef6f2d5be03b42b58b93f75d719bb15898435e9049650d579",
    "Os": "linux",
    "Parent":
"cd2d84f556b7179a55502d49d17707bf0f72478ce6bbd9b31d54d57b90633209"
,
    "Size": 0,
    "VirtualSize": 646538905
}
]
vagrant@docker:~$




	Inspect Docker Container


vagrant@docker:~$ sudo docker ps
CONTAINER ID        IMAGE                 COMMAND                CREATED
     STATUS              PORTS                    NAMES
c3ebfb50ccd3        17653fe98f52:latest   "/bin/sh -c 'node in   7 minutes ago
     Up 7 minutes        0.0.0.0:8080->3000/tcp   focused_jang
vagrant@docker:~$ sudo docker inspect c3ebfb50ccd3
[{
    "AppArmorProfile": "",
    "Args": [
        "-c",
        "node index.js"
    ],
    "Config": {
        "AttachStderr": true,
        "AttachStdin": false,
        "AttachStdout": true,
        "Cmd": [
            "/bin/sh",
            "-c",
            "node index.js"
        ],
        "CpuShares": 0,
        "Cpuset": "",
        "Domainname": "",
        "Entrypoint": null,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",

            "NODE_VERSION=0.12.15"
        ],
        "ExposedPorts": {
            "3000/tcp": {}
        },
        "Hostname": "c3ebfb50ccd3",
        "Image": "17653fe98f52",
        "Labels": {},
        "MacAddress": "",
        "Memory": 0,
        "MemorySwap": 0,
        "NetworkDisabled": false,
        "OnBuild": null,
        "OpenStdin": false,
        "PortSpecs": null,
        "StdinOnce": false,
        "Tty": true,
        "User": "",
        "Volumes": null,
        "WorkingDir": "/app"
    },
    "Created": "2016-08-29T13:23:40.353069086Z",
    "Driver": "devicemapper",
    "ExecDriver": "native-0.2",
    "ExecIDs": null,
    "HostConfig": {
        "Binds": null,
        "CapAdd": null,
        "CapDrop": null,
        "CgroupParent": "",
        "ContainerIDFile": "",
        "CpuShares": 0,
        "CpusetCpus": "",
        "Devices": [],
        "Dns": null,
        "DnsSearch": null,
        "ExtraHosts": null,
        "IpcMode": "",
        "Links": null,
        "LogConfig": {
            "Config": null,
            "Type": "json-file"
        },
        "LxcConf": [],
        "Memory": 0,
        "MemorySwap": 0,
        "NetworkMode": "bridge",
        "PidMode": "",
        "PortBindings": {
            "3000/tcp": [
                {
                    "HostIp": "",
                    "HostPort": "8080"
                }
            ]
        },
        "Privileged": false,
        "PublishAllPorts": false,
        "ReadonlyRootfs": false,
        "RestartPolicy": {
            "MaximumRetryCount": 0,
            "Name": "no"
        },
        "SecurityOpt": null,
        "Ulimits": null,
        "VolumesFrom": null
    },
    "HostnamePath":
"/var/lib/docker/containers/c3ebfb50ccd300ba080b6c65e0012727
b2e61a734d8b7b35a1189fe9c89d9d79/hostname",
    "HostsPath":
"/var/lib/docker/containers/c3ebfb50ccd300ba080b6c65e0012727b2e
61a734d8b7b35a1189fe9c89d9d79/hosts",
    "Id": "c3ebfb50ccd300ba080b6c65e0012727b2e61a734d8b7b35a1189fe9c89d9d79",
    "Image":
"17653fe98f52973ef6f2d5be03b42b58b93f75d719bb15898435e9049650d579",

    "LogPath":
"/var/lib/docker/containers/c3ebfb50ccd300ba080b6c65e0012727b2e61
a734d8b7b35a1189fe9c89d9d79/c3ebfb50ccd300ba080b6c65e0012727b2e61a734d8b7b35a118
9fe9c89d9d79-json.log",
    "MountLabel": "",
    "Name": "/focused_jang",
    "NetworkSettings": {
        "Bridge": "docker0",
        "Gateway": "172.17.42.1",
        "GlobalIPv6Address": "",
        "GlobalIPv6PrefixLen": 0,
        "IPAddress": "172.17.0.12",
        "IPPrefixLen": 16,
        "IPv6Gateway": "",
        "LinkLocalIPv6Address": "fe80::42:acff:fe11:c",
        "LinkLocalIPv6PrefixLen": 64,
        "MacAddress": "02:42:ac:11:00:0c",
        "PortMapping": null,
        "Ports": {
            "3000/tcp": [
                {
                    "HostIp": "0.0.0.0",
                    "HostPort": "8080"
                }
            ]
        }
    },
    "Path": "/bin/sh",
    "ProcessLabel": "",
    "ResolvConfPath":
"/var/lib/docker/containers/c3ebfb50ccd300ba080b6c65e00127
27b2e61a734d8b7b35a1189fe9c89d9d79/resolv.conf",
    "RestartCount": 0,
    "State": {
        "Dead": false,
        "Error": "",
        "ExitCode": 0,
        "FinishedAt": "0001-01-01T00:00:00Z",
        "OOMKilled": false,
        "Paused": false,
        "Pid": 15642,
        "Restarting": false,
        "Running": true,
        "StartedAt": "2016-08-29T13:23:40.665899647Z"
    },
    "Volumes": {},
    "VolumesRW": {}
}
]
vagrant@docker:~$


