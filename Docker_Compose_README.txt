Docker-Compose README
======================


Created by: Donal Cooney

Note: 	This demo follows the very basic introduction to the docker (See 
	README.txt) created purely as a tutorial for myself. Absolutely 
	no gaurantees whatsoever wrt these instructions and/or software.


Docker-compose Summary:
------------------------

Starting point: You have followed the Docker-demo (README.txt in this repo), & 
you use that docker image as the basis here. Basically use docker Compose to
launch multiple containers




1.	Install docker-compose
2.	docker-compose filr overview (basic)
3. 	start multiple containers with docker compose



Instructions
------------


	INSTALL DOCKER COMPOSE
	
vagrant@docker:~/docker-demo$ sudo -s
root@docker:~/docker-demo#
root@docker:~/docker-demo#
root@docker:~/docker-demo#
root@docker:~/docker-demo#
root@docker:~/docker-demo#
root@docker:~/docker-demo# curl -L
https://github.com/docker/compose/releases/d
ownload/1.8.0/docker-compose-`uname -s`-`uname -m` >
/usr/local/bin/docker-comp
ose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time
  % Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   600    0   600    0     0    666      0 --:--:-- --:--:-- --:--:--   665
100 7783k  100 7783k    0     0  1367k      0  0:00:05  0:00:05 --:--:-- 1756k
root@docker:~/docker-demo# chmod +x /usr/local/bin/docker-compose


root@docker:~/docker-demo# usermod -G docker vagrant
root@docker:~/docker-demo#

	REMEMBER TO LOGOUT/LOGIN (TO ESURE GROUPS RELOAD). NOW - USE DOCKER 
	WITHOUT BEING ROOT



	Basic docker-compose file 
 

web:					web and db containers defined here
  build: .				build web contr image from current path
					Builds using Dockerfile
  command: node index-db.js		run indes-db (node file)
  ports:
    - "3000:3000"			expose port 3000
  links:
    - db				link to the db. So, db needs to be up
					before web container!!
  environment:				psss env variables
    MYSQL_DATABASE: myapp
    MYSQL_USER: myapp
    MYSQL_PASSWORD: mysecurepass
    MYSQL_HOST: db
db:
  image: orchardup/mysql
  ports:
    - "3306:3306"
  environment:
    MYSQL_DATABASE: myapp
    MYSQL_USER: myapp
    MYSQL_PASSWORD: mysecurepass
docker-compose.yml (END)




	START THE WEB AND DB CONTAINERS

	
vagrant@docker:~/docker-demo$ docker-compose build && docker-compose up db
db uses an image, skipping
Building web
Step 1 : FROM node:0.12
0.12: Pulling from library/node
8ad8b3f87b37: Pull complete
751fe39c4d34: Pull complete
ae3b77eefc06: Pull complete
7783aac582ec: Pull complete
393ad8a32e58: Pull complete
2f7393a3dc50: Pull complete
Digest:
sha256:35cb28d33c79f5c4df7c8fca92d7db0591721327c1695cebb0f635579d718a85
Status: Downloaded newer image for node:0.12
 ---> d419d6448a2d
Step 2 : WORKDIR /app



vagrant@docker:~/docker-demo$ docker-compose up -d web
dockerdemo_db_1 is up-to-date
Creating dockerdemo_web_1
vagrant@docker:~/docker-demo$


vagrant@docker:~/docker-demo$ curl localhost:3000
Hello World! You are visitor number 1vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$ curl localhost:3000
Hello World! You are visitor number 2vagrant@docker:~/docker-demo$ curl local
docker-compose up -d web
dockerdemo_db_1 is up-to-date
dockerdemo_web_1 is up-to-date
vagrant@docker:~/docker-demo$ curl localhost:3000
Hello World! You are visitor number 3vagrant@docker:~/docker-demo$



