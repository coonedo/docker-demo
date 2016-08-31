
Docker-demo Docker_Networks_README
==================================


Created by: Donal Cooney

Note: 	This demo is a very basic introduction to the topic named above created 
	purely as a tutorial for myself. Absolutely no gaurantees whatsoever 
	wrt these instructions and/or software.


Docker Networks Summary:
------------------------

1. 	User defined bridge network
2.	....
3.	....







Instructions:
-------------


-	USER DEFINED BRIDGE NETWORK



	Create a bridged network

vagrant@docker:~/docker-demo$ docker network create --driver bridge my_network
713a76d9260e3206c21440c4762dc34584840e7c6f554607d943f238e7ce6c26


	Fire up 2 containers running ununbu in this network

vagrant@docker:~/docker-demo$ docker run --net=my_network -itd
--name=container
1 ubuntu:14.04
Unable to find image 'ubuntu:14.04' locally
14.04: Pulling from library/ubuntu
862a3e9af0ae: Pull complete
6498e51874bf: Pull complete
159ebdd1959b: Pull complete
0fdbedd3771a: Pull complete
7a1f7116d1e3: Pull complete
Digest:
sha256:5b5d48912298181c3c80086e7d3982029b288678fccabf2265899199c24d7f89
Status: Downloaded newer image for ubuntu:14.04
cd38ecd402c2b27bca98d6ba13e9cc3ac07a278946825f3cf72245ae8a7b9862
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED
STATUS              PORTS               NAMES
cd38ecd402c2        ubuntu:14.04        "/bin/bash"         9 seconds ago
Up 7 seconds                            container1
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$ docker run --net=my_network -itd --name=container2 ubuntu:14.04
bc0620777c67621bf4b87659146a52723f353d08810797591d6f1157d6e08e04
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$
vagrant@docker:~/docker-demo$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED
STATUS              PORTS               NAMES
bc0620777c67        ubuntu:14.04        "/bin/bash"         8 seconds ago
Up 7 seconds                            container2
cd38ecd402c2        ubuntu:14.04        "/bin/bash"         5 minutes ago
Up 5 minutes                            container1



	Have a look at the bridge network we created

vagrant@docker:~/docker-demo$ docker network inspect my_bridge
[]
Error: No such network: my_bridge
vagrant@docker:~/docker-demo$ docker network inspect my_network
[
    {
        "Name": "my_network",
        "Id":
"713a76d9260e3206c21440c4762dc34584840e7c6f554607d943f238e7ce6c26"
,
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1/16"
                }
            ]
        },
        "Internal": false,
        "Containers": {
            "bc0620777c67621bf4b87659146a52723f353d08810797591d6f1157d6e08e04":
{
                "Name": "container2",
                "EndpointID":
"11dbd7297757aee355a612cd7d4807dac7e3ee857c04cdd79
59113d5f3903cad",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "cd38ecd402c2b27bca98d6ba13e9cc3ac07a278946825f3cf72245ae8a7b9862":
{
                "Name": "container1",
                "EndpointID":
"25275b0afbc1f05f3cbde69834fd87146ab39b92b83939d89
0019311d6ecbd20",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
vagrant@docker:~/docker-demo$





	Attach to container1, have a look at the network settings

vagrant@docker:/src/webapp$ docker attach container1
root@cd38ecd402c2:/#
root@cd38ecd402c2:/#
root@cd38ecd402c2:/#
root@cd38ecd402c2:/# ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:12:00:02
          inet addr:172.18.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe12:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:45 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:3498 (3.4 KB)  TX bytes:1908 (1.9 KB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@cd38ecd402c2:/#



	From within container1, ping container2

	
root@cd38ecd402c2:/# ping -w3 172.18.0.3
PING 172.18.0.3 (172.18.0.3) 56(84) bytes of data.
64 bytes from 172.18.0.3: icmp_seq=1 ttl=64 time=0.397 ms
64 bytes from 172.18.0.3: icmp_seq=2 ttl=64 time=0.168 ms
64 bytes from 172.18.0.3: icmp_seq=3 ttl=64 time=0.263 ms

--- 172.18.0.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 0.168/0.276/0.397/0.093 ms
root@cd38ecd402c2:/#


