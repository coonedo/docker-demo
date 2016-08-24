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

	https://github.com/donaltn3/docker-demo


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














