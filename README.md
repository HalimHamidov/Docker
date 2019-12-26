
# docker-1


Docker
Introduction to Docker

Setup docker-machine:
export MACHINE_STORAGE_PATH=/tmp
docker-machine create default
eval $(docker-machine env default)

Setup docker-for-mac at 42:
cd ~ && rm -rf Library/com.docker.docker
mkdir /goinfre/docker
ln -s /goinfre/docker Library/com.docker.docker


https://github.com/VBrazhnik/docker-1/wiki/How-to-Docker

https://labs.play-with-docker.com/p/bnn6efot9690009ba990#bnn6efot_bnn6eg0t9690009ba99g
cshinoha - Саша
github.com/apranzo mikim42

`docker-1` is a School 42 Docker project.

It consists of three parts: 
1. How to Docker
2. Dockerfiles
3. Bonus

_At the [wiki-pages](../../wiki) of project you can find detailed explanations for each task._

[`docker.en.pdf`](/docker.en.pdf) is the task file.

### How to clone?

This repository includes submodule. Submodule is an app for `ex02` in `01_dockerfiles` part. It is used to demonstrate that created Dockerfile works correctly.

If you want to clone repository with submodule use:

```
git clone --recurse-submodules <repository url>
```

If you will use `git clone <repository url>`, you will get the empty `app` folder in `01_dockerfiles/ex02`.

===================================================
How to install:
https://stackoverflow.com/c/42network/questions/782
42toolbox to free space
https://github.com/alexandregv/42toolbox

V.2 Exercises
For each exercise, we will ask you to give the shell command(s) to:


il-j2% ls
01    02    03    04    05    06    07    08    09    10    11    12    13    14    15    16    17    18    19    20    21    22    23    24    25    26    27    28    29    30    31    32    33    34
il-j2% cat *
docker-machine create --driver virtualbox Char
docker-machine ip Char
eval $(docker-machine env Char)
docker pull hello-world
docker run hello-world
docker run -d -p 5000:80 --name overlord --restart=always nginx
docker inspect -f '{{.NetworkSettings.IPAddress}}' overlord
docker run -it --rm alpine /bin/sh
apt-get update && apt-get upgrade -y && apt-get install -y build-essential git
docker volume create --name hatchery
docker volume ls
docker run -d --name spawning-pool --restart=on-failure:10 -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings -v hatchery:/var/lib/mysql mysql --default-authentication-plugin=mysql_native_password
docker inspect -f '{{.Config.Env}}' spawning-pool
docker run -d --name lair -p 8080:80 --link spawning-pool:mysql wordpress
docker run --name roach-warden  -d --link spawning-pool:db -p 8081:80 phpmyadmin/phpmyadmin
docker logs --follow spawning-pool
docker ps
docker restart overlord
docker run --name Abathur -v ~/:/root -p 3000:3000 -dit python:2-slim
docker exec Abathur pip install Flask
echo 'from flask import Flask\napp = Flask(__name__)\n@app.route("/")\ndef hello_world():\n\treturn "<h1>Hello, World!</h1>"' > ~/app.py
docker exec -e FLASK_APP=/root/app.py Abathur flask run --host=0.0.0.0 --port 3000
docker swarm init --advertise-addr $(docker-machine ip Char)
docker-machine create --driver virtualbox Aiur
docker-machine ssh Aiur "docker swarm join --token $(docker swarm join-token worker -q) $(docker-machine ip Char):2377"
docker network create -d overlay overmind
docker service create -d --network overmind --name orbital-command -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=root rabbitmq
docker service ls
docker service create -d --network overmind --name engineering-bay --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/engineering-bay
docker service logs -f $(docker service ps engineering-bay -f "name=engineering-bay.1" -q)
docker service create -d --network overmind --name marines --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/marine-squad
docker service ps marines
docker service scale -d marines=20
docker service rm $(docker service ls -q)
docker rm -f $(docker ps -a -q)
docker rmi -f $(docker images -a -q)
docker-machine rm -y Aiur
1. Create a virtual machine with docker-machine using the virtualbox driver, and named Char.
Answer:
docker-machine create --driver virtualbox Char

Source:
https://docs.docker.com/machine/get-started/
https://docs.docker.com/machine/reference/create/

2. Get the IP address of the Char virtual machine.
Answer:
il-j2% docker-machine ip Char
192.168.99.109

Source:
https://docs.docker.com/machine/reference/ip/

3. Define the variables needed by your virtual machine Char in the general env of your terminal, so that you can run the docker ps command without errors. You have to fix all four environment variables with one command, and you are not allowed to use your shell’s builtin to set these variables by hand.
Answer:
eval $(docker-machine env Char)

Source:
https://docs.docker.com/machine/reference/env/


4.Get the hello-world container from the Docker Hub, where it’s available.
Answer:
docker pull hello-world
Source:
https://docs.docker.com/docker-hub/repos/
https://docs.docker.com/engine/reference/commandline/images/

5. Launch the hello-world container, and make sure that it prints its welcome mes-
sage, then leaves it.
Answer:
il-j2% docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
Source:
https://docs.docker.com/engine/reference/commandline/run/
https://docs.docker.com/engine/reference/commandline/image/

6. Launch an nginx container, available on Docker Hub, as a background task. It should be named overlord, be able to restart on its own, and have its 80 port attached to the 5000 port of Char. You can check that your container functions properly by visiting
http://<ip-de-char>:5000 on your web browser.

Answer:
docker run --name overlord -d -p 5000:80  --restart=always nginx


Source:
https://hub.docker.com/_/nginx/
https://docs.docker.com/engine/reference/commandline/run/
https://docs.docker.com/network/network-tutorial-host/

7. Get the internal IP address of the overlord container without starting its shell and
in one command.

Answer:
il-j2% docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
06f7f3d3d5d9        nginx               "nginx -g 'daemon of…"   30 minutes ago      Up 30 minutes       0.0.0.0:5000->80/tcp   overload
il-j2% docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 06f7f3d3d5d9
172.17.0.2
il-j2% docker inspect -f '{{.NetworkSettings.IPAddress}}' 06f7f3d3d5d9
172.17.0.2

Misc:
il-j2% docker-machine inspect --format='{{prettyjson .Driver}}' Char
{
"Boot2DockerImportVM": "",
"Boot2DockerURL": "",
"CPU": 1,
"DNSProxy": true,
"DiskSize": 20000,
"HostDNSResolver": false,
"HostInterfaces": {},
"HostOnlyCIDR": "192.168.99.1/24",
"HostOnlyNicType": "82540EM",
"HostOnlyNoDHCP": false,
"HostOnlyPromiscMode": "deny",
"IPAddress": "192.168.99.109",
"MachineName": "Char",
"Memory": 1024,
"NatNicType": "82540EM",
"NoShare": false,
"NoVTXCheck": false,
"SSHKeyPath": "/Users/apearl/.docker/machine/machines/Char/id_rsa",
"SSHPort": 50034,
"SSHUser": "docker",
"ShareFolder": "",
"StorePath": "/Users/apearl/.docker/machine",
"SwarmDiscovery": "",
"SwarmHost": "tcp://0.0.0.0:3376",
"SwarmMaster": false,
"UIType": "headless",
"VBoxManager": {}
}

Source:
https://docs.docker.com/engine/reference/commandline/inspect/


8. Launch a shell from an alpine container, and make sure that you can interact directly with the container via your terminal, and that the container deletes itself once the shell’s execution is done.
Answer:
il-j2% docker run -it alpine /bin/sh
/ #
Source:
https://docs.docker.com/network/network-tutorial-standalone/
https://stackoverflow.com/questions/35689628/starting-a-shell-in-the-docker-alpine-container/43564198#43564198


9. From the shell of a debian container, install via the container’s package manager everything you need to compile C source code and push it onto a git repo (of course, make sure before that the package manager and the packages already in the container are updated). For this exercise, you should only specify the commands to be run directly in the container.

Answer
il-j2% docker run -ti --rm debian
root@87cc156066e6:/# apt-get update && apt-get upgrade -y && apt-get install -y build-essential git
Get:1 http://security-cdn.debian.org/debian-security buster/updates InRelease [65.4 kB]

Source:
https://git-scm.com/download/linux
https://help.ubuntu.com/community/InstallingCompilers
https://stackoverflow.com/questions/35689628/starting-a-shell-in-the-docker-alpine-container/43564198#43564198
https://ru.stackoverflow.com/questions/535559/%D0%A7%D1%82%D0%BE-%D0%BE%D0%B7%D0%BD%D0%B0%D1%87%D0%B0%D0%B5%D1%82-y-%D0%BF%D1%80%D0%B8-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B5-%D0%B2-apt-get

10. Create a volume named hatchery.

Answer:
il-j2% docker volume create --name hatchery
hatchery

Source:
https://docs.docker.com/engine/reference/commandline/volume_create/
https://docs.docker.com/storage/volumes/

11. List all the Docker volumes created on the machine. Remember. VOLUMES.
Answer:
il-j2% docker volume ls
DRIVER              VOLUME NAME
local               hatchery
Source:
https://docs.docker.com/engine/reference/commandline/volume_ls/

12. Launch a mysql container as a background task. It should be able to restart on its own in case of error, and the root password of the database should be Kerrigan. You will also make sure that the database is stored in the hatchery volume, that the container directly creates a database named zerglings, and that the container itself is named spawning-pool.
il-j2% docker run -d --name spawning-pool --restart=on-failure:10 -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings -v hatchery:/var/lib/mysql mysql --default-authentication-plugin=mysql_native_password
nable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
d599a449871e: Pull complete
f287049d3170: Pull complete
08947732a1b0: Pull complete
96f3056887f2: Pull complete
871f7f65f017: Pull complete
1dd50c4b99cb: Pull complete
5bcbdf508448: Pull complete
a59dcbc3daa2: Pull complete
13e6809ab808: Pull complete
2148d51b084d: Pull complete
93982f7293d7: Pull complete
e736330a6d9c: Pull complete
Digest: sha256:c93ba1bafd65888947f5cd8bd45deb7b996885ec2a16c574c530c389335e9169
Status: Downloaded newer image for mysql:latest
6c69c1333fc0249cc7550493a45395bc86c8dcb3e2c61363179da757770e3d7f

13. Print the environment variables of the spawning-pool container in one command, to be sure that you have configured your container properly.
Answer:
il-j2% docker inspect -f '{{.Config.Env}}' spawning-pool
[MYSQL_ROOT_PASSWORD=Kerrigan MYSQL_DATABASE=zerglings PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin GOSU_VERSION=1.7 MYSQL_MAJOR=8.0 MYSQL_VERSION=8.0.18-1debian9]



14. Launch a wordpress container as a background task, just for fun. The container should be named lair, its 80 port should be bound to the 8080 port of the virtual machine, and it should be able to use the spawning-pool container as a database service. You can try to access lair on your machine via a web browser, with the IP address of the virtual machine as a URL.
Congratulations, you just deployed a functional Wordpress website in two com- mands!

il-j2% docker run -d --name lair -p 8080:80 --link spawning-pool:mysql wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
000eee12ec04: Pull complete
8ae4f9fcfeea: Pull complete
60f22fbbd07a: Pull complete
ccc7a63ad75f: Pull complete
a2427b8dd6e7: Pull complete
91cac3b30184: Pull complete
d6e40015fc10: Pull complete
3951eb02eb9d: Pull complete
a30c5ce4d825: Pull complete
1d2fc8e19e4d: Pull complete
8c746235d858: Pull complete
887411e72bcf: Pull complete
eba633920d44: Pull complete
9b7007daa55e: Pull complete
4d20187eeb14: Pull complete
3e1b35074ec2: Pull complete
a13668f53ada: Pull complete
38760061f7fb: Pull complete
9448b0eeecae: Pull complete
10f402ca3df6: Pull complete
6515b8350ad0: Pull complete
Digest: sha256:670136dc1c6b4fe232c126aaa0f25f596bda9696d535ff8ff25f7680235afcce
Status: Downloaded newer image for wordpress:latest
17e1169548e6d1524f6054de190a14d91526ca5f9ce5222780e1d45ecb4864f8


il-j2% docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
17e1169548e6        wordpress           "docker-entrypoint.s…"   12 seconds ago      Up 11 seconds       0.0.0.0:8080->80/tcp   lair
6c69c1333fc0        mysql               "docker-entrypoint.s…"   51 minutes ago      Up 51 minutes       3306/tcp, 33060/tcp    spawning-pool

on website
http://192.168.99.109:8080/wp-admin/install.php

15. Launch a phpmyadmin container as a background task. It should be named roach-warden, its 80 port should be bound to the 8081 port of the virtual machine and it should be able to explore the database stored in the spawning-pool container.
Answer:
il-j2% docker run --name roach-warden -d --link spawning-pool:db -p 8081:80 phpmyadmin/phpmyadmin
Unable to find image 'phpmyadmin/phpmyadmin:latest' locally
latest: Pulling from phpmyadmin/phpmyadmin
000eee12ec04: Already exists
8ae4f9fcfeea: Already exists
60f22fbbd07a: Already exists
ccc7a63ad75f: Already exists
a2427b8dd6e7: Already exists
91cac3b30184: Already exists
d6e40015fc10: Already exists
240e21c03bb4: Pull complete
504858e1e4aa: Pull complete
9a0523b2d73f: Pull complete
163ef1a69c2e: Pull complete
1a564803a07b: Pull complete
4ae2a0882c31: Pull complete
0ba6971ca6fc: Pull complete
55652e94064a: Pull complete
9e59ee976f51: Pull complete
0dc245440bd0: Pull complete
f0139774ec90: Pull complete
97f5943225b5: Pull complete
Digest: sha256:996393cc538b9a08dae7ecf7620898534e029f3ecd7c2f88922e597571d7ccd2
Status: Downloaded newer image for phpmyadmin/phpmyadmin:latest
a3a3ea851d71b4f9670aaa82c933fc8b1d8d69574117a510afbd040e8b22684b
il-j2% docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
a3a3ea851d71        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   6 seconds ago       Up 4 seconds        0.0.0.0:8081->80/tcp   roach-warden
17e1169548e6        wordpress               "docker-entrypoint.s…"   4 minutes ago       Up 4 minutes        0.0.0.0:8080->80/tcp   lair
6c69c1333fc0        mysql                   "docker-entrypoint.s…"   56 minutes ago      Up 56 minutes       3306/tcp, 33060/tcp    spawning-pool

http://192.168.99.109:8081/index.php

http://localhost:8080
42
apearl
halimjonik@gmail.com
uR^YPg&)BPe2#H9PIo
http://localhost:8080/wp-admin/


Source:https://docs.docker.com/network/links/
16. Look up the spawning-pool container’s logs in real time without running its shell.

il-j2% docker logs --follow spawning-pool
2019-12-09 14:51:25+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.18-1debian9 started.
2019-12-09 14:51:25+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2019-12-09 14:51:25+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.18-1debian9 started.
2019-12-09 14:51:25+00:00 [Note] [Entrypoint]: Initializing database files
2019-12-09T14:51:25.929721Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.
2019-12-09T14:51:25.929803Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.18) initializing of server in progress as process 44
2019-12-09T14:51:29.833699Z 5 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2019-12-09 14:51:32+00:00 [Note] [Entrypoint]: Database files initialized
2019-12-09 14:51:32+00:00 [Note] [Entrypoint]: Starting temporary server
2019-12-09T14:51:32.629450Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.
2019-12-09T14:51:32.629548Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.18) starting as process 93
2019-12-09T14:51:33.313394Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2019-12-09T14:51:33.315945Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2019-12-09T14:51:33.330389Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.18'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server - GPL.
2019-12-09 14:51:33+00:00 [Note] [Entrypoint]: Temporary server started.
2019-12-09T14:51:33.402233Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock'
Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
2019-12-09 14:51:36+00:00 [Note] [Entrypoint]: Creating database zerglings

2019-12-09 14:51:36+00:00 [Note] [Entrypoint]: Stopping temporary server
2019-12-09T14:51:36.304609Z 11 [System] [MY-013172] [Server] Received SHUTDOWN from user root. Shutting down mysqld (Version: 8.0.18).
2019-12-09T14:51:37.684030Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.18)  MySQL Community Server - GPL.
2019-12-09 14:51:38+00:00 [Note] [Entrypoint]: Temporary server stopped

2019-12-09 14:51:38+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

2019-12-09T14:51:38.562224Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.
2019-12-09T14:51:38.562317Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.18) starting as process 1
2019-12-09T14:51:38.995980Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2019-12-09T14:51:38.999479Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2019-12-09T14:51:39.014096Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.18'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
2019-12-09T14:51:39.189696Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock' bind-address: '::' port: 33060
mbind: Operation not permitted
mbind: Operation not permitted
mbind: Operation not permitted


17. Display all the currently active containers on the Char virtual machine.

il-j2% docker container ls
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
a3a3ea851d71        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   15 minutes ago      Up 15 minutes       0.0.0.0:8081->80/tcp   roach-warden
17e1169548e6        wordpress               "docker-entrypoint.s…"   20 minutes ago      Up 20 minutes       0.0.0.0:8080->80/tcp   lair
6c69c1333fc0        mysql                   "docker-entrypoint.s…"   About an hour ago   Up About an hour    3306/tcp, 33060/tcp    spawning-pool
il-j2% docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
a3a3ea851d71        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   15 minutes ago      Up 15 minutes       0.0.0.0:8081->80/tcp   roach-warden
17e1169548e6        wordpress               "docker-entrypoint.s…"   20 minutes ago      Up 20 minutes       0.0.0.0:8080->80/tcp   lair
6c69c1333fc0        mysql                   "docker-entrypoint.s…"   About an hour ago   Up About an hour    3306/tcp, 33060/tcp    spawning-pool

18. Relaunch the overlord container.


il-j2% docker run -d -p 5000:80 --name overlord --restart=always nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
000eee12ec04: Already exists
eb22865337de: Pull complete
bee5d581ef8b: Pull complete
Digest: sha256:50cf965a6e08ec5784009d0fccb380fc479826b6e0e65684d9879170a9df8566
Status: Downloaded newer image for nginx:latest
206d5dc963d75d661a920174c76865ab8ef2886fc10abb35c47e323098d457fa
il-j2% docker restart overlord
overlord
il-j2% docker container ls
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                  NAMES
206d5dc963d7        nginx                   "nginx -g 'daemon of…"   21 seconds ago      Up 6 seconds        0.0.0.0:5000->80/tcp   overlord
a3a3ea851d71        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   59 minutes ago      Up 59 minutes       0.0.0.0:8081->80/tcp   roach-warden
17e1169548e6        wordpress               "docker-entrypoint.s…"   About an hour ago   Up About an hour    0.0.0.0:8080->80/tcp   lair
6c69c1333fc0        mysql                   "docker-entrypoint.s…"   2 hours ago         Up 2 hours          3306/tcp, 33060/tcp    spawning-pool



19. Launch a container name Abathur. It will be a Python container, 2-slim version, its /root folder will be bound to a HOME folder on your host, and its 3000 port will be bound to the 3000 port of your virtual machine.
You will personalize this container so that you can use the Flask micro-framework in its latest version. You will make sure that an html page displaying "Hello World" with <h1> tags can be served by Flask. You will test that your container is properly set up by accessing, via curl or a web browser, the IP address of your virtual machine on the 3000 port.
You will also list all the necessary commands in your repository.

il-j2% docker run --name Abathur -v ~/:/root -p 3000:3000 -dit python:2-slim
Unable to find image 'python:2-slim' locally
2-slim: Pulling from library/python
000eee12ec04: Already exists
4a1fc2539e7f: Pull complete
21142e4357ee: Pull complete
1c09cd5028ba: Pull complete
Digest: sha256:dbbceeffb1e1a7c0761916ffce8ef8a2d96af3e43bb7e2a9839a85d236b9f6ea
Status: Downloaded newer image for python:2-slim
6864cb811202ef1d1a87b74e44045aa6d19fb0e47c78e51d7922338bc1711411

il-j2% docker exec Abathur pip install Flask
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Collecting Flask
Downloading https://files.pythonhosted.org/packages/9b/93/628509b8d5dc749656a9641f4caf13540e2cdec85276964ff8f43bbb1d3b/Flask-1.1.1-py2.py3-none-any.whl (94kB)
Collecting click>=5.1
Downloading https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl (81kB)
Collecting Werkzeug>=0.15
Downloading https://files.pythonhosted.org/packages/ce/42/3aeda98f96e85fd26180534d36570e4d18108d62ae36f87694b476b83d6f/Werkzeug-0.16.0-py2.py3-none-any.whl (327kB)
Collecting itsdangerous>=0.24
Downloading https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
Collecting Jinja2>=2.10.1
Downloading https://files.pythonhosted.org/packages/65/e0/eb35e762802015cab1ccee04e8a277b03f1d8e53da3ec3106882ec42558b/Jinja2-2.10.3-py2.py3-none-any.whl (125kB)
Collecting MarkupSafe>=0.23
Downloading https://files.pythonhosted.org/packages/fb/40/f3adb7cf24a8012813c5edb20329eb22d5d8e2a0ecf73d21d6b85865da11/MarkupSafe-1.1.1-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: click, Werkzeug, itsdangerous, MarkupSafe, Jinja2, Flask
Successfully installed Flask-1.1.1 Jinja2-2.10.3 MarkupSafe-1.1.1 Werkzeug-0.16.0 click-7.0 itsdangerous-1.1.0

il-j2% echo 'from flask import Flask\napp = Flask(__name__)\n@app.route("/")\ndef hello_world():\n\treturn "<h1>Hello, World!</h1>"' > ~/app.py

il-j2% docker exec -e FLASK_APP=/root/app.py Abathur flask run --host=0.0.0.0 --port 3000
* Serving Flask app "/root/app.py"
* Environment: production
WARNING: This is a development server. Do not use it in a production deployment.
Use a production WSGI server instead.
* Debug mode: off
* Running on http://0.0.0.0:3000/ (Press CTRL+C to quit)


On browser: http://192.168.99.109:3000


20. Create a local swarm, the Char virtual machine should be its manager.
il-j2% docker swarm init --advertise-addr $(docker-machine ip Char)
Swarm initialized: current node (n6mimodq4rlv0kbpti90pyau6) is now a manager.

To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-2yh43nsm0zrgfl9ce6vzp3m76g9d6yeooo6euqgt32clqau2fy-23xknjm9lvsquziw6n65cewoh 192.168.99.109:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
Create a swarm | Docker Documentation:

Run the following command to create a new swarm:

docker swarm init --advertise-addr <MANAGER-IP>
il-j2% docker info
Client:
Debug Mode: false

Server:
Containers: 15
Running: 5
Paused: 0
Stopped: 10
Images: 8
Server Version: 19.03.5
Storage Driver: overlay2
Backing Filesystem: extfs
Supports d_type: true
Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
Volume: local
Network: bridge host ipvlan macvlan null overlay
Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: active
NodeID: 51d7324r6hss9g6u7dn0o31t3
Is Manager: true
ClusterID: 4mbfcu98sr8wyull7dmz60guj
Managers: 1
Nodes: 1
Default Address Pool: 10.0.0.0/8
SubnetSize: 24
Data Path Port: 4789
Orchestration:
Task History Retention Limit: 5
Raft:
Snapshot Interval: 10000
Number of Old Snapshots to Retain: 0
Heartbeat Tick: 1
Election Tick: 10
Dispatcher:
Heartbeat Period: 5 seconds
CA Configuration:
Expiry Duration: 3 months
Force Rotate: 0
Autolock Managers: false
Root Rotation In Progress: false
Node Address: 192.168.99.109
Manager Addresses:
192.168.99.109:2377
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
init version: fec3683
Security Options:
seccomp
Profile: default
Kernel Version: 4.14.154-boot2docker
Operating System: Boot2Docker 19.03.5 (TCL 10.1)
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 989.5MiB
Name: Char
ID: GIYW:VZXD:2IA4:ZRIF:YBQ2:TRWT:U4QT:DSKF:C65E:2GZO:PHPP:SCIF
Docker Root Dir: /mnt/sda1/var/lib/docker
Debug Mode: false
Registry: https://index.docker.io/v1/
Labels:
provider=virtualbox
Experimental: false
Insecure Registries:
127.0.0.0/8
Live Restore Enabled: false
Product License: Community Engine
21. Create another virtual machine with docker-machine using the virtualbox driver, and name it Aiur.

il-j2% docker-machine create --driver virtualbox Aiur
Running pre-create checks...
Creating machine...
(Aiur) Copying /Users/apearl/.docker/machine/cache/boot2docker.iso to /Users/apearl/.docker/machine/machines/Aiur/boot2docker.iso...
(Aiur) Creating VirtualBox VM...
(Aiur) Creating SSH key...
(Aiur) Starting the VM...
(Aiur) Check network to re-create if needed...
(Aiur) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env Aiur


22. Turn Aiur into a slave of the local swarm in which Char is leader (the command to take control of Aiur is not requested).

il-j2% docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
Aiur   -        virtualbox   Running   tcp://192.168.99.111:2376           v19.03.5
Char   *        virtualbox   Running   tcp://192.168.99.109:2376           v19.03.5
il-j2% docker-machine ssh Aiur "docker swarm join --token $(docker swarm join-token worker -q) $(docker-machine ip Char):2377"
This node joined a swarm as a worker.
il-j2% docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
Aiur   -        virtualbox   Running   tcp://192.168.99.111:2376           v19.03.5
Char   *        virtualbox   Running   tcp://192.168.99.109:2376           v19.03.5
il-j2% docker info
Client:
Debug Mode: false

Server:
Containers: 15
Running: 5
Paused: 0
Stopped: 10
Images: 8
Server Version: 19.03.5
Storage Driver: overlay2
Backing Filesystem: extfs
Supports d_type: true
Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
Volume: local
Network: bridge host ipvlan macvlan null overlay
Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: active
NodeID: 51d7324r6hss9g6u7dn0o31t3
Is Manager: true
ClusterID: 4mbfcu98sr8wyull7dmz60guj
Managers: 1
Nodes: 2
Default Address Pool: 10.0.0.0/8
SubnetSize: 24
Data Path Port: 4789
Orchestration:
Task History Retention Limit: 5
Raft:
Snapshot Interval: 10000
Number of Old Snapshots to Retain: 0
Heartbeat Tick: 1
Election Tick: 10
Dispatcher:
Heartbeat Period: 5 seconds
CA Configuration:
Expiry Duration: 3 months
Force Rotate: 0
Autolock Managers: false
Root Rotation In Progress: false
Node Address: 192.168.99.109
Manager Addresses:
192.168.99.109:2377
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
init version: fec3683
Security Options:
seccomp
Profile: default
Kernel Version: 4.14.154-boot2docker
Operating System: Boot2Docker 19.03.5 (TCL 10.1)
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 989.5MiB
Name: Char
ID: GIYW:VZXD:2IA4:ZRIF:YBQ2:TRWT:U4QT:DSKF:C65E:2GZO:PHPP:SCIF
Docker Root Dir: /mnt/sda1/var/lib/docker
Debug Mode: false
Registry: https://index.docker.io/v1/
Labels:
provider=virtualbox
Experimental: false
Insecure Registries:
127.0.0.0/8
Live Restore Enabled: false
Product License: Community Engine

il-j2% docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
stz9vkio3vrpre3chr3tnj936     Aiur                Ready               Active                                  19.03.5
51d7324r6hss9g6u7dn0o31t3 *   Char                Ready               Active              Leader              19.03.5
il-j2% docker service ls


23. Create an overlay-type internal network that you will name overmind.
il-j2% docker network create -d overlay overmind
xamf4n59vue5f1f8e26p61n4n

il-j2% docker network create -d overlay overmind
xamf4n59vue5f1f8e26p61n4n
il-j2% docker network inspect overmind
[
{
"Name": "overmind",
"Id": "xamf4n59vue5f1f8e26p61n4n",
"Created": "2019-12-13T18:43:13.471347333Z",
"Scope": "swarm",
"Driver": "overlay",
"EnableIPv6": false,
"IPAM": {
"Driver": "default",
"Options": null,
"Config": [
{
"Subnet": "10.0.1.0/24",
"Gateway": "10.0.1.1"
}
]
},
"Internal": false,
"Attachable": false,
"Ingress": false,
"ConfigFrom": {
"Network": ""
},
"ConfigOnly": false,
"Containers": null,
"Options": {
"com.docker.network.driver.overlay.vxlanid_list": "4097"
},
"Labels": null
}
]

il-j2% docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5c0931bc4e8c        bridge              bridge              local
8e4aa7dfd281        docker_gwbridge     bridge              local
13f6f9b9c87e        host                host                local
im8y5dxjirb4        ingress             overlay             swarm
e7482d50882e        none                null                local
xamf4n59vue5        overmind            overlay             swarm


https://docs.docker.com/v17.09/engine/swarm/networking/


24. Launch a rabbitmq SERVICE that will be named orbital-command. You should define a specific user and password for the RabbitMQ service, they can be whatever you want. This service will be on the overmind network.

il-j2% docker service create -d --network overmind --name orbital-command -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=root rabbitmq
hq7jqaw8ezr5fm9j0j09vd5g2

il-j2% docker run -d --hostname my-rabbit --name orbital-command rabbitmq:3
Unable to find image 'rabbitmq:3' locally
3: Pulling from library/rabbitmq
Digest: sha256:1f91f51d065a8ba54d17675aace83a22b1a8d99880f7a602493b98cd9d950789
Status: Downloaded newer image for rabbitmq:3
e81305fbfe2cd364a1d68979f969619d704acc179ada87c0e9c241aa810ffb36

il-j2% docker logs orbital-command
2019-12-13 18:50:12.457 [info] <0.8.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.457 [info] <0.8.0> Feature flags:   [ ] implicit_default_bindings
2019-12-13 18:50:12.458 [info] <0.8.0> Feature flags:   [ ] quorum_queue
2019-12-13 18:50:12.458 [info] <0.8.0> Feature flags:   [ ] virtual_host_metadata
2019-12-13 18:50:12.458 [info] <0.8.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.483 [info] <0.258.0> ra: meta data store initialised. 0 record(s) recovered
2019-12-13 18:50:12.484 [info] <0.263.0> WAL: recovering []
2019-12-13 18:50:12.485 [info] <0.267.0>
Starting RabbitMQ 3.8.2 on Erlang 22.2
Copyright (c) 2007-2019 Pivotal Software, Inc.
Licensed under the MPL 1.1. Website: https://rabbitmq.com

##  ##      RabbitMQ 3.8.2
##  ##
##########  Copyright (c) 2007-2019 Pivotal Software, Inc.
######  ##
##########  Licensed under the MPL 1.1. Website: https://rabbitmq.com

Doc guides: https://rabbitmq.com/documentation.html
Support:    https://rabbitmq.com/contact.html
Tutorials:  https://rabbitmq.com/getstarted.html
Monitoring: https://rabbitmq.com/monitoring.html

Logs: <stdout>

Config file(s): /etc/rabbitmq/rabbitmq.conf

Starting broker...2019-12-13 18:50:12.486 [info] <0.267.0>
node           : rabbit@my-rabbit
home dir       : /var/lib/rabbitmq
config file(s) : /etc/rabbitmq/rabbitmq.conf
cookie hash    : S2YWwX5P1SwWjVcIbWJdkg==
log(s)         : <stdout>
database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit
2019-12-13 18:50:12.495 [info] <0.267.0> Running boot step pre_boot defined by app rabbit
2019-12-13 18:50:12.495 [info] <0.267.0> Running boot step rabbit_core_metrics defined by app rabbit
2019-12-13 18:50:12.495 [info] <0.267.0> Running boot step rabbit_alarm defined by app rabbit
2019-12-13 18:50:12.501 [info] <0.273.0> Memory high watermark set to 395 MiB (415014912 bytes) of 989 MiB (1037537280 bytes) total
2019-12-13 18:50:12.505 [info] <0.275.0> Enabling free disk space monitoring
2019-12-13 18:50:12.505 [info] <0.275.0> Disk free limit set to 50MB
2019-12-13 18:50:12.508 [info] <0.267.0> Running boot step code_server_cache defined by app rabbit
2019-12-13 18:50:12.508 [info] <0.267.0> Running boot step file_handle_cache defined by app rabbit
2019-12-13 18:50:12.508 [info] <0.278.0> Limiting to approx 1048479 file handles (943629 sockets)
2019-12-13 18:50:12.508 [info] <0.279.0> FHC read buffering:  OFF
2019-12-13 18:50:12.508 [info] <0.279.0> FHC write buffering: ON
2019-12-13 18:50:12.509 [info] <0.267.0> Running boot step worker_pool defined by app rabbit
2019-12-13 18:50:12.509 [info] <0.268.0> Will use 1 processes for default worker pool
2019-12-13 18:50:12.509 [info] <0.268.0> Starting worker pool 'worker_pool' with 1 processes in it
2019-12-13 18:50:12.509 [info] <0.267.0> Running boot step database defined by app rabbit
2019-12-13 18:50:12.510 [info] <0.267.0> Node database directory at /var/lib/rabbitmq/mnesia/rabbit@my-rabbit is empty. Assuming we need to join an existing cluster or initialise from scratch...
2019-12-13 18:50:12.510 [info] <0.267.0> Configured peer discovery backend: rabbit_peer_discovery_classic_config
2019-12-13 18:50:12.510 [info] <0.267.0> Will try to lock with peer discovery backend rabbit_peer_discovery_classic_config
2019-12-13 18:50:12.510 [info] <0.267.0> Peer discovery backend does not support locking, falling back to randomized delay
2019-12-13 18:50:12.510 [info] <0.267.0> Peer discovery backend rabbit_peer_discovery_classic_config does not support registration, skipping randomized startup delay.
2019-12-13 18:50:12.510 [info] <0.267.0> All discovered existing cluster peers:
2019-12-13 18:50:12.510 [info] <0.267.0> Discovered no peer nodes to cluster with. Some discovery backends can filter nodes out based on a readiness criteria. Enabling debug logging might help troubleshoot.
2019-12-13 18:50:12.514 [info] <0.43.0> Application mnesia exited with reason: stopped
2019-12-13 18:50:12.588 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.601 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.601 [info] <0.267.0> Feature flag `implicit_default_bindings`: supported, attempt to enable...
2019-12-13 18:50:12.602 [info] <0.267.0> Feature flag `implicit_default_bindings`: mark as enabled=state_changing
2019-12-13 18:50:12.607 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.607 [info] <0.267.0> Feature flags:   [~] implicit_default_bindings
2019-12-13 18:50:12.607 [info] <0.267.0> Feature flags:   [ ] quorum_queue
2019-12-13 18:50:12.607 [info] <0.267.0> Feature flags:   [ ] virtual_host_metadata
2019-12-13 18:50:12.607 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.616 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 0 retries left
2019-12-13 18:50:12.617 [info] <0.267.0> Feature flag `implicit_default_bindings`: mark as enabled=true
2019-12-13 18:50:12.624 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.624 [info] <0.267.0> Feature flags:   [x] implicit_default_bindings
2019-12-13 18:50:12.624 [info] <0.267.0> Feature flags:   [ ] quorum_queue
2019-12-13 18:50:12.624 [info] <0.267.0> Feature flags:   [ ] virtual_host_metadata
2019-12-13 18:50:12.625 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.635 [info] <0.267.0> Feature flag `quorum_queue`: supported, attempt to enable...
2019-12-13 18:50:12.635 [info] <0.267.0> Feature flag `quorum_queue`: mark as enabled=state_changing
2019-12-13 18:50:12.640 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.640 [info] <0.267.0> Feature flags:   [x] implicit_default_bindings
2019-12-13 18:50:12.641 [info] <0.267.0> Feature flags:   [~] quorum_queue
2019-12-13 18:50:12.641 [info] <0.267.0> Feature flags:   [ ] virtual_host_metadata
2019-12-13 18:50:12.641 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.650 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.652 [info] <0.267.0> Feature flag `quorum_queue`:   migrating Mnesia table rabbit_queue...
2019-12-13 18:50:12.665 [info] <0.267.0> Feature flag `quorum_queue`:   migrating Mnesia table rabbit_durable_queue...
2019-12-13 18:50:12.676 [info] <0.267.0> Feature flag `quorum_queue`:   Mnesia tables migration done
2019-12-13 18:50:12.676 [info] <0.267.0> Feature flag `quorum_queue`: mark as enabled=true
2019-12-13 18:50:12.683 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.683 [info] <0.267.0> Feature flags:   [x] implicit_default_bindings
2019-12-13 18:50:12.683 [info] <0.267.0> Feature flags:   [x] quorum_queue
2019-12-13 18:50:12.684 [info] <0.267.0> Feature flags:   [ ] virtual_host_metadata
2019-12-13 18:50:12.684 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.692 [info] <0.267.0> Feature flag `virtual_host_metadata`: supported, attempt to enable...
2019-12-13 18:50:12.693 [info] <0.267.0> Feature flag `virtual_host_metadata`: mark as enabled=state_changing
2019-12-13 18:50:12.698 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.698 [info] <0.267.0> Feature flags:   [x] implicit_default_bindings
2019-12-13 18:50:12.698 [info] <0.267.0> Feature flags:   [x] quorum_queue
2019-12-13 18:50:12.698 [info] <0.267.0> Feature flags:   [~] virtual_host_metadata
2019-12-13 18:50:12.698 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.707 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.724 [info] <0.267.0> Feature flag `virtual_host_metadata`: mark as enabled=true
2019-12-13 18:50:12.733 [info] <0.267.0> Feature flags: list of feature flags found:
2019-12-13 18:50:12.733 [info] <0.267.0> Feature flags:   [x] implicit_default_bindings
2019-12-13 18:50:12.733 [info] <0.267.0> Feature flags:   [x] quorum_queue
2019-12-13 18:50:12.733 [info] <0.267.0> Feature flags:   [x] virtual_host_metadata
2019-12-13 18:50:12.733 [info] <0.267.0> Feature flags: feature flag states written to disk: yes
2019-12-13 18:50:12.742 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.756 [info] <0.267.0> Waiting for Mnesia tables for 30000 ms, 9 retries left
2019-12-13 18:50:12.756 [info] <0.267.0> Peer discovery backend rabbit_peer_discovery_classic_config does not support registration, skipping registration.
2019-12-13 18:50:12.757 [info] <0.267.0> Running boot step database_sync defined by app rabbit
2019-12-13 18:50:12.757 [info] <0.267.0> Running boot step feature_flags defined by app rabbit
2019-12-13 18:50:12.757 [info] <0.267.0> Running boot step codec_correctness_check defined by app rabbit
2019-12-13 18:50:12.757 [info] <0.267.0> Running boot step external_infrastructure defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_registry defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_auth_mechanism_cr_demo defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_queue_location_random defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_event defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_auth_mechanism_amqplain defined by app rabbit
2019-12-13 18:50:12.758 [info] <0.267.0> Running boot step rabbit_auth_mechanism_plain defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_exchange_type_direct defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_exchange_type_fanout defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_exchange_type_headers defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_exchange_type_topic defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_mirror_queue_mode_all defined by app rabbit
2019-12-13 18:50:12.759 [info] <0.267.0> Running boot step rabbit_mirror_queue_mode_exactly defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step rabbit_mirror_queue_mode_nodes defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step rabbit_priority_queue defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Priority queues enabled, real BQ is rabbit_variable_queue
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step rabbit_queue_location_client_local defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step rabbit_queue_location_min_masters defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step kernel_ready defined by app rabbit
2019-12-13 18:50:12.760 [info] <0.267.0> Running boot step rabbit_sysmon_minder defined by app rabbit
2019-12-13 18:50:12.761 [info] <0.267.0> Running boot step rabbit_epmd_monitor defined by app rabbit
2019-12-13 18:50:12.764 [info] <0.500.0> epmd monitor knows us, inter-node communication (distribution) port: 25672
2019-12-13 18:50:12.764 [info] <0.267.0> Running boot step guid_generator defined by app rabbit
2019-12-13 18:50:12.765 [info] <0.267.0> Running boot step rabbit_node_monitor defined by app rabbit
2019-12-13 18:50:12.765 [info] <0.506.0> Starting rabbit_node_monitor
2019-12-13 18:50:12.765 [info] <0.267.0> Running boot step delegate_sup defined by app rabbit
2019-12-13 18:50:12.765 [info] <0.267.0> Running boot step rabbit_memory_monitor defined by app rabbit
2019-12-13 18:50:12.766 [info] <0.267.0> Running boot step core_initialized defined by app rabbit
2019-12-13 18:50:12.766 [info] <0.267.0> Running boot step upgrade_queues defined by app rabbit
2019-12-13 18:50:12.777 [info] <0.267.0> message_store upgrades: 1 to apply
2019-12-13 18:50:12.777 [info] <0.267.0> message_store upgrades: Applying rabbit_variable_queue:move_messages_to_vhost_store
2019-12-13 18:50:12.777 [info] <0.267.0> message_store upgrades: No durable queues found. Skipping message store migration
2019-12-13 18:50:12.777 [info] <0.267.0> message_store upgrades: Removing the old message store data
2019-12-13 18:50:12.779 [info] <0.267.0> message_store upgrades: All upgrades applied successfully
2019-12-13 18:50:12.791 [info] <0.267.0> Running boot step rabbit_connection_tracking defined by app rabbit
2019-12-13 18:50:12.791 [info] <0.267.0> Running boot step rabbit_connection_tracking_handler defined by app rabbit
2019-12-13 18:50:12.791 [info] <0.267.0> Running boot step rabbit_exchange_parameters defined by app rabbit
2019-12-13 18:50:12.792 [info] <0.267.0> Running boot step rabbit_mirror_queue_misc defined by app rabbit
2019-12-13 18:50:12.792 [info] <0.267.0> Running boot step rabbit_policies defined by app rabbit
2019-12-13 18:50:12.793 [info] <0.267.0> Running boot step rabbit_policy defined by app rabbit
2019-12-13 18:50:12.793 [info] <0.267.0> Running boot step rabbit_queue_location_validator defined by app rabbit
2019-12-13 18:50:12.793 [info] <0.267.0> Running boot step rabbit_quorum_memory_manager defined by app rabbit
2019-12-13 18:50:12.793 [info] <0.267.0> Running boot step rabbit_vhost_limit defined by app rabbit
2019-12-13 18:50:12.793 [info] <0.267.0> Running boot step recovery defined by app rabbit
2019-12-13 18:50:12.794 [info] <0.267.0> Running boot step load_core_definitions defined by app rabbit
2019-12-13 18:50:12.794 [info] <0.267.0> Running boot step empty_db_check defined by app rabbit
2019-12-13 18:50:12.795 [info] <0.267.0> Adding vhost '/' (description: 'Default virtual host')
2019-12-13 18:50:12.805 [info] <0.543.0> Making sure data directory '/var/lib/rabbitmq/mnesia/rabbit@my-rabbit/msg_stores/vhosts/628WB79CIFDYO9LJI6DKMI09L' for vhost '/' exists
2019-12-13 18:50:12.808 [info] <0.543.0> Starting message stores for vhost '/'
2019-12-13 18:50:12.809 [info] <0.547.0> Message store "628WB79CIFDYO9LJI6DKMI09L/msg_store_transient": using rabbit_msg_store_ets_index to provide index
2019-12-13 18:50:12.810 [info] <0.543.0> Started message store of type transient for vhost '/'
2019-12-13 18:50:12.811 [info] <0.550.0> Message store "628WB79CIFDYO9LJI6DKMI09L/msg_store_persistent": using rabbit_msg_store_ets_index to provide index
2019-12-13 18:50:12.811 [warning] <0.550.0> Message store "628WB79CIFDYO9LJI6DKMI09L/msg_store_persistent": rebuilding indices from scratch
2019-12-13 18:50:12.813 [info] <0.543.0> Started message store of type persistent for vhost '/'
2019-12-13 18:50:12.814 [info] <0.267.0> Creating user 'guest'
2019-12-13 18:50:12.816 [info] <0.267.0> Setting user tags for user 'guest' to [administrator]
2019-12-13 18:50:12.817 [info] <0.267.0> Setting permissions for 'guest' in '/' to '.*', '.*', '.*'
2019-12-13 18:50:12.818 [info] <0.267.0> Running boot step rabbit_looking_glass defined by app rabbit
2019-12-13 18:50:12.818 [info] <0.267.0> Running boot step rabbit_core_metrics_gc defined by app rabbit
2019-12-13 18:50:12.819 [info] <0.267.0> Running boot step background_gc defined by app rabbit
2019-12-13 18:50:12.819 [info] <0.267.0> Running boot step connection_tracking defined by app rabbit
2019-12-13 18:50:12.822 [info] <0.267.0> Setting up a table for connection tracking on this node: 'tracked_connection_on_node_rabbit@my-rabbit'
2019-12-13 18:50:12.825 [info] <0.267.0> Setting up a table for per-vhost connection counting on this node: 'tracked_connection_per_vhost_on_node_rabbit@my-rabbit'
2019-12-13 18:50:12.825 [info] <0.267.0> Running boot step routing_ready defined by app rabbit
2019-12-13 18:50:12.825 [info] <0.267.0> Running boot step pre_flight defined by app rabbit
2019-12-13 18:50:12.825 [info] <0.267.0> Running boot step notify_cluster defined by app rabbit
2019-12-13 18:50:12.826 [info] <0.267.0> Running boot step networking defined by app rabbit
2019-12-13 18:50:12.828 [info] <0.596.0> started TCP listener on [::]:5672
2019-12-13 18:50:12.828 [info] <0.267.0> Running boot step cluster_name defined by app rabbit
2019-12-13 18:50:12.828 [info] <0.267.0> Running boot step direct_client defined by app rabbit
2019-12-13 18:50:13.267 [info] <0.8.0> Server startup complete; 0 plugins started.
completed with 0 plugins.


25. List all the services of the local swarm.
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
hq7jqaw8ezr5        orbital-command     replicated          1/1                 rabbitmq:latest



26. Launch a 42school/engineering-bay service in two replicas and make sure that the service works properly (see the documentation provided at hub.docker.com). This service will be named engineering-bay and will be on the overmind network.
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
hq7jqaw8ezr5        orbital-command     replicated          1/1                 rabbitmq:latest
il-j2%
il-j2% docker service create -d --network overmind --name engineering-bay --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/engineering-bay
x3uvzhjwqjijsteecljabxwx7
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                             PORTS
x3uvzhjwqjij        engineering-bay     replicated          1/2                 42school/engineering-bay:latest
hq7jqaw8ezr5        orbital-command     replicated          1/1                 rabbitmq:latest

27. Get the real-time logs of one the tasks of the engineering-bay service.
il-j2% docker service logs -f $(docker service ps engineering-bay -f "name=engineering-bay.1" -q)
engineering-bay.1.jlu6gsrpacf4@Aiur    |  Zerg #01-00 from Hatchery a7c979367c91 : Bwaaaaarghfnle (I'm attacking this building!)
engineering-bay.1.jlu6gsrpacf4@Aiur    |  Zerg #01-01 from Hatchery a7c979367c91 : Bwaaaaarghfnle (I'm attacking this building!)



28. ... Damn it, a group of zergs is attacking orbital-command, and shutting down the engineering-bay service won’t help at all... You must send a troup of Marines to eliminate the intruders. Launch a 42school/marine-squad in two replicas, and make sure that the service works properly (see the documentation provided at hub.docker.com). This service will be named... marines and will be on the overmind network.
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                             PORTS
x3uvzhjwqjij        engineering-bay     replicated          2/2                 42school/engineering-bay:latest
hq7jqaw8ezr5        orbital-command     replicated          1/1                 rabbitmq:latest
il-j2% docker service create -d --network overmind --name marines --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/marine-squad
b9uwl8huropp4ijs8l3j1po3x
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                             PORTS
x3uvzhjwqjij        engineering-bay     replicated          2/2                 42school/engineering-bay:latest
b9uwl8huropp        marines             replicated          0/2                 42school/marine-squad:latest
hq7jqaw8ezr5        orbital-command     replicated          1/1                 rabbitmq:latest
il-j2%




29. Display all the tasks of the marines service.
il-j2% docker service ps marines
ID                  NAME                IMAGE                          NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
rirx7yrkgqck        marines.1           42school/marine-squad:latest   Aiur                Running             Running 58 seconds ago
z32ha0l5185a        marines.2           42school/marine-squad:latest   Char                Running             Running 58 seconds ago

30. Increase the number of copies of the marines service up to twenty, because there’s never enough Marines to eliminate Zergs. (Remember to take a look at the tasks and logs of the service, you’ll see, it’s fun.)
//docker service scale marines=20
il-j2% docker service scale -d marines=20
marines scaled to 20
il-j2% docker service ps marines
ID                  NAME                IMAGE                          NODE                DESIRED STATE       CURRENT STATE                    ERROR               PORTS
rirx7yrkgqck        marines.1           42school/marine-squad:latest   Aiur                Running             Running 3 minutes ago
z32ha0l5185a        marines.2           42school/marine-squad:latest   Char                Running             Running 3 minutes ago
pmvnce3su5ik        marines.3           42school/marine-squad:latest   Char                Running             Running less than a second ago
vc2m2l6zcwcr        marines.4           42school/marine-squad:latest   Aiur                Running             Running 1 second ago
8dxl0e0nsn7n        marines.5           42school/marine-squad:latest   Aiur                Running             Running less than a second ago
emyxubnr4fbj        marines.6           42school/marine-squad:latest   Char                Running             Running less than a second ago
qgkapk7uescl        marines.7           42school/marine-squad:latest   Char                Running             Running 2 seconds ago
uku36xw4l1e8        marines.8           42school/marine-squad:latest   Char                Running             Running less than a second ago
pynazp4orcy9        marines.9           42school/marine-squad:latest   Aiur                Running             Running less than a second ago
vtxokkt7e70e        marines.10          42school/marine-squad:latest   Char                Running             Running less than a second ago
pr1env4fzrrl        marines.11          42school/marine-squad:latest   Char                Running             Running less than a second ago
yf01575qxx71        marines.12          42school/marine-squad:latest   Aiur                Running             Running less than a second ago
ae3stqc5cqfs        marines.13          42school/marine-squad:latest   Aiur                Running             Running 1 second ago
oj0dkgy8n445        marines.14          42school/marine-squad:latest   Char                Running             Running less than a second ago
cvswa7e8e8qf        marines.15          42school/marine-squad:latest   Char                Running             Running less than a second ago
dh80vui6gh86        marines.16          42school/marine-squad:latest   Aiur                Running             Running less than a second ago
dolibfb8oyc4        marines.17          42school/marine-squad:latest   Aiur                Running             Running 1 second ago
t6cw6jf75pjs        marines.18          42school/marine-squad:latest   Aiur                Running             Running less than a second ago
3e9tmqcbsyfg        marines.19          42school/marine-squad:latest   Aiur                Running             Running 1 second ago
x3hw95ea4un3        marines.20          42school/marine-squad:latest   Char                Running             Running less than a second ago



il-j2% docker service logs -f $(docker service ps marines -f "name=marines.11" -q)
marines.11.pr1env4fzrrl@Char    |  [MARINE SQUAD - c90b09614100] - Waiting for orders
marines.11.pr1env4fzrrl@Char    |  [MARINE SQUAD - c90b09614100] - Attacking zerg#32-0 from hatchery 9024496f242f !!
marines.11.pr1env4fzrrl@Char    |  [x] Zerg Dead...
marines.11.pr1env4fzrrl@Char    |  [MARINE SQUAD - c90b09614100] - Attacking zerg#32-5 from hatchery 9024496f242f !!


31. Force quit and delete all the services on the local swarm, in one command.
il-j2% docker service rm $(docker service ls -q)
x3uvzhjwqjij
b9uwl8huropp
hq7jqaw8ezr5
il-j2% docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS

32. Force quit and delete all the containers (whatever their status), in one command.
il-j2% docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                                NAMES
e81305fbfe2c        rabbitmq:3              "docker-entrypoint.s…"   21 minutes ago      Up 21 minutes       4369/tcp, 5671-5672/tcp, 25672/tcp   orbital-command
45f43f32a42c        python:2-slim           "python2"                About an hour ago   Up About an hour    0.0.0.0:3000->3000/tcp               Abathur
97887793415a        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   About an hour ago   Up About an hour    0.0.0.0:8081->80/tcp                 roach-warden
8055743b3165        wordpress               "docker-entrypoint.s…"   2 hours ago         Up 2 hours          0.0.0.0:8080->80/tcp                 lair
0acf7687bfde        mysql                   "docker-entrypoint.s…"   2 hours ago         Up 2 hours          3306/tcp, 33060/tcp                  spawning-pool
3e37cea243ef        nginx                   "nginx -g 'daemon of…"   6 days ago          Up About an hour    0.0.0.0:5000->80/tcp                 overlord
il-j2% docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                         PORTS                                NAMES
e81305fbfe2c        rabbitmq:3              "docker-entrypoint.s…"   21 minutes ago      Up 21 minutes                  4369/tcp, 5671-5672/tcp, 25672/tcp   orbital-command
45f43f32a42c        python:2-slim           "python2"                About an hour ago   Up About an hour               0.0.0.0:3000->3000/tcp               Abathur
97887793415a        phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   About an hour ago   Up About an hour               0.0.0.0:8081->80/tcp                 roach-warden
cfde6b8be459        debian                  "bash"                   2 hours ago         Exited (0) About an hour ago                                        goofy_gagarin
8055743b3165        wordpress               "docker-entrypoint.s…"   2 hours ago         Up 2 hours                     0.0.0.0:8080->80/tcp                 lair
d92be349bfe5        debian                  "ls /"                   2 hours ago         Exited (0) 2 hours ago                                              wonderful_varahamihira
ee5380547529        alpine                  "ls /"                   2 hours ago         Exited (0) 2 hours ago                                              quizzical_ishizaka
bbd09a38d376        hello-world             "ls /"                   2 hours ago         Created                                                             eloquent_murdock
c0d1489962f0        debian                  "ls /"                   2 hours ago         Exited (0) 2 hours ago                                              vibrant_lumiere
3c6086e3f927        nginx                   "ls /"                   2 hours ago         Exited (0) 2 hours ago                                              clever_shtern
de88ff63b49f        mysql                   "docker-entrypoint.s…"   2 hours ago         Exited (0) 2 hours ago                                              fervent_edison
0acf7687bfde        mysql                   "docker-entrypoint.s…"   2 hours ago         Up 2 hours                     3306/tcp, 33060/tcp                  spawning-pool
2ccb5b0209d8        alpine                  "/bin/sh"                4 days ago          Exited (0) 4 days ago                                               magical_booth
1dd28eaa44e8        hello-world             "/hello"                 4 days ago          Exited (0) 4 days ago                                               serene_bhabha
3e37cea243ef        nginx                   "nginx -g 'daemon of…"   6 days ago          Up About an hour               0.0.0.0:5000->80/tcp                 overlord
3542aac756f0        hello-world             "/hello"                 6 days ago          Exited (0) 6 days ago                                               cool_bose
il-j2% docker rm -f $(docker ps -a -q)
e81305fbfe2c
45f43f32a42c
97887793415a
cfde6b8be459
8055743b3165
d92be349bfe5
ee5380547529
bbd09a38d376
c0d1489962f0
3c6086e3f927
de88ff63b49f
0acf7687bfde
2ccb5b0209d8
1dd28eaa44e8
3e37cea243ef
3542aac756f0
il-j2% docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
il-j2% docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES



33. Delete all the container images stored on the Char virtual machine, in one command as well.

docker rmi -f $(docker images -a -q)

il-j2% docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
phpmyadmin/phpmyadmin      latest              5c0865e6f546        2 days ago          454MB
rabbitmq                   3                   458123c67b79        2 days ago          151MB
rabbitmq                   latest              458123c67b79        2 days ago          151MB
wordpress                  latest              ee025cbcbc20        7 days ago          539MB
python                     2-slim              772727e1e204        2 weeks ago         137MB
mysql                      latest              d435eee2caa5        2 weeks ago         456MB
nginx                      latest              231d40e811cd        2 weeks ago         126MB
debian                     latest              67e34c1c9477        3 weeks ago         114MB
alpine                     latest              965ea09ff2eb        7 weeks ago         5.55MB
hello-world                latest              fce289e99eb9        11 months ago       1.84kB
42school/marine-squad      <none>              bdb625dad464        2 years ago         81.5MB
42school/engineering-bay   <none>              faa22fd5eedd        2 years ago         81.5MB
il-j2% docker images -a
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
phpmyadmin/phpmyadmin      latest              5c0865e6f546        2 days ago          454MB
rabbitmq                   3                   458123c67b79        2 days ago          151MB
rabbitmq                   latest              458123c67b79        2 days ago          151MB
wordpress                  latest              ee025cbcbc20        7 days ago          539MB
python                     2-slim              772727e1e204        2 weeks ago         137MB
mysql                      latest              d435eee2caa5        2 weeks ago         456MB
nginx                      latest              231d40e811cd        2 weeks ago         126MB
debian                     latest              67e34c1c9477        3 weeks ago         114MB
alpine                     latest              965ea09ff2eb        7 weeks ago         5.55MB
hello-world                latest              fce289e99eb9        11 months ago       1.84kB
42school/marine-squad      <none>              bdb625dad464        2 years ago         81.5MB
42school/engineering-bay   <none>              faa22fd5eedd        2 years ago         81.5MB
il-j2% docker images -a -q
5c0865e6f546
458123c67b79
458123c67b79
ee025cbcbc20
772727e1e204
d435eee2caa5
231d40e811cd
67e34c1c9477
965ea09ff2eb
fce289e99eb9
bdb625dad464
faa22fd5eedd
il-j2% docker rmi $(docker images -a -q)
Untagged: phpmyadmin/phpmyadmin:latest
Untagged: phpmyadmin/phpmyadmin@sha256:d5148b3263e4a3db2a42afcaa5565416b9b63bd8992b1c9aed9c18b7512d1014
Deleted: sha256:5c0865e6f546efd4d992d195f4eeef5afe173e15fa702373f088a46bb5531781
Deleted: sha256:2a61add490c0739a61efd1d3940da64a7e954e2bc2522267b42d8083209f60f7
Deleted: sha256:7366bea3f18623b421970d4893bf7de5f52de6cc1b7be559992bf70c0a110c11
Deleted: sha256:c570c8d723fe4e942a0d6242f3318bf074a9e17609e536a72b38e5eac80824c2
Deleted: sha256:086808af1f38e1ba7296dc5916b4440b67560b022bc5518133a3e222e019d452
Deleted: sha256:af5ad3b3d67299a1e63f98c24c8b1c80b3d08714b2e34faa76b865b3b8df42a7
Deleted: sha256:be30c86125eb687af0cd979ad2b9e0f0c89f81aa734932aa9407808d11225abb
Deleted: sha256:f1ab8d06de9da3a1edc7b5665a3f1e1c691512cc7aa8a55850b142f42f2f5b1f
Deleted: sha256:f133d17c174890fee97e415dfb64ec7a39342491fe07a00c478cb0f9678ce040
Deleted: sha256:728ae57552543bd8e6942a05693700ec72dcc632a738ecbbd574a659220ad80b
Deleted: sha256:7ef10f7a819528246f76727051a1d4f5a31d9a4cf60f3ac38ee3861b78a1654a
Deleted: sha256:c7901c1860de8b64e91472f2aab5f94c6b6a0d16986d3f4e5be8e4ca7656b961
Deleted: sha256:abe9b9d6261e15627681eb14c3dd2c5aeb6e17cd9545ca0967976a72869340f8
Untagged: wordpress:latest
Untagged: wordpress@sha256:670136dc1c6b4fe232c126aaa0f25f596bda9696d535ff8ff25f7680235afcce
Deleted: sha256:ee025cbcbc20b95a9e724abe0d8765d88effd253cecbacb7ab3b42dc1dceb462
Deleted: sha256:b2de1fbac09f5b8d632c7c3837e5e741b322cabeefb5cf8dd551ee1c6da505ff
Deleted: sha256:82cce4f6d253901d3e6f6d7a84e674735993d250c0b1faf13864117f5535e466
Deleted: sha256:12d6a557fb79a5a54e70e7f20ea25184feb86049c237447864ab88b17e5231a6
Deleted: sha256:8eea87809496ad2d60fcf07246bc5714c3154e4e8823b5bd0dabcccb2553ee44
Deleted: sha256:404bf5abc6b12163e3a2090cd7ab373f3152073981d44e963922ccf2cdcfa460
Deleted: sha256:ff854400e1606c6155259c55ab3fda6fdcd1685dc9fae7ecc184264fba77c552
Deleted: sha256:bd8ece6356ddca24338ee753ca3042c43dcf10c781fdebb4db22efcc26300a6b
Deleted: sha256:af899e5e5d980730ba382f84126d174967284dacb623c634a12870e64e943991
Deleted: sha256:3694607dc85c08dc2795ab47c8a5af9be67bcc1121725ad851b9ba7e2acd9467
Deleted: sha256:a607843b6009a85910d38131450f723502acd3ce070a7e189834a83fc8b5a7d5
Deleted: sha256:72d0194080b874a7d840840546709b249a48c6656e34bdc07502287eb41a5b79
Deleted: sha256:7eba01eeecbf73e16ef14c9e2f9715ba87cec8ed1b6aeeafc05eb7f491830570
Deleted: sha256:918e869bf2957a8b2fd4c1795fa33e82bb6348bce3a06e17506aaaec6bf1e64d
Deleted: sha256:10c525a6eea1d83abf9854f072ac9b575ca5a0f05914c07ddaf93daa51e297e6
Deleted: sha256:992f5bdd06a0e992710cb57c26e0b1f81c1ebe471fae06e7c4e57f38791d2427
Deleted: sha256:6cfbf8346c0fe7a8bb2e2c5312e2edb773f3f0b444d72ba8dbee9f6794144cd8
Deleted: sha256:44b9b55ea4a26bd23e337f7d7bdd2a692dccc78e64b460585773e70eb6ce3a49
Deleted: sha256:d4cf6e034ae560179c9feae8ebb01b181393e16040d70ee76c3aaeac8d9db9c8
Deleted: sha256:16362d40e91311cee6918d85161d42614753fa393cba107cbf785479b072d05e
Deleted: sha256:acbb341204ac30e6a049576ffeb3d68e1c05f5ae05913c63365e4e71d618b430
Untagged: python:2-slim
Untagged: python@sha256:dbbceeffb1e1a7c0761916ffce8ef8a2d96af3e43bb7e2a9839a85d236b9f6ea
Deleted: sha256:772727e1e204aae81e5c153a3136b095e38bcc74620dbd1e0a439b55a99d253d
Deleted: sha256:0e02ba71bcec2e6967301878c77c55e216145f26dc40fd5f3dc61b4d7ba01ac3
Deleted: sha256:bd3458fe78b22eb959f2c6cca0ebcf9eca9a9da6fe996653c42f0d7b2189ff67
Deleted: sha256:0fb81cb38e52898ffd05f128b095e81d8d1cf89168da7c0d82e783ac0ba144b1
Untagged: mysql:latest
Untagged: mysql@sha256:c93ba1bafd65888947f5cd8bd45deb7b996885ec2a16c574c530c389335e9169
Deleted: sha256:d435eee2caa52800a0b534b130fd8f8a94f772f9ed08d0da1053fa3a1b7daad2
Deleted: sha256:ae21db0dd817f30c04d5e9e0a70cfbfca2045f669df974330c783f841bc2740a
Deleted: sha256:2f70a51b7bbcad0a31ada04cf0ce10a5d641e07146310cc63291aaff9dd31ba8
Deleted: sha256:8fda2f97f253feba8c792344ad6d868e8a39fb2f826d1501f06511cc4ae49375
Deleted: sha256:08b5c195e5fca4357318ac02c02f3795b3a860957c7a599fdb20b8e55ac23cf4
Deleted: sha256:7d7dacacf6d1f13bd3594c7f9668cf0b41f3cac0cb6073614792a46cbcfceda7
Deleted: sha256:716f31b2e8d6601d02f6b47152ae5014c53c1239bfc46fce1dae97e6c332e94c
Deleted: sha256:b931154d48fa379790de00f12cb8ef69c4a2a0de1503a12de6524b13d4d81e15
Deleted: sha256:23564e7a9d5d1538f334f9a6b05b8c5f140356a83fe818c3a2e60903196263e8
Deleted: sha256:47bd75b99433b84ea120085de92e11cc2a77664be8be33db10542443dbb888e2
Deleted: sha256:4122ef4cce751f9a349611026d962d7eefedea9af10939fe90b1d49670bb72bb
Deleted: sha256:de2a383a7557d4bc8388ec86422dd01be3b1413dc55ee3f5e82342a226274392
Deleted: sha256:99b5261d397c75b1de2c91f0c89f4e6f287248669b72de6cb143e82ea67ad056
Untagged: nginx:latest
Untagged: nginx@sha256:50cf965a6e08ec5784009d0fccb380fc479826b6e0e65684d9879170a9df8566
Deleted: sha256:231d40e811cd970168fb0c4770f2161aa30b9ba6fe8e68527504df69643aa145
Deleted: sha256:dc8adf8fa0fc82a56c32efac9d0da5f84153888317c88ab55123d9e71777bc62
Deleted: sha256:77fcff986d3b13762e4777046b9210a109fda20cb261bd3bbe5d7161d4e73c8e
Deleted: sha256:831c5620387fb9efec59fc82a42b948546c6be601e3ab34a87108ecf852aa15f
Untagged: debian:latest
Untagged: debian@sha256:79f0b1682af1a6a29ff63182c8103027f4de98b22d8fb50040e9c4bb13e3de78
Deleted: sha256:67e34c1c9477023c0ce84c20ae5af961a6509f9952c2ebbf834c5ea0a286f2b8
Deleted: sha256:f2b4f0674ba3e6119088fe8a98c7921ed850c48d4d76e8caecd7f3d57721b4cb
Untagged: alpine:latest
Untagged: alpine@sha256:c19173c5ada610a5989151111163d28a67368362762534d8a8121ce95cf2bd5a
Deleted: sha256:965ea09ff2ebd2b9eeec88cd822ce156f6674c7e99be082c7efac3c62f3ff652
Deleted: sha256:77cae8ab23bf486355d1b3191259705374f4a11d483b24964d2f729dd8c076a0
Untagged: hello-world:latest
Untagged: hello-world@sha256:4df8ca8a7e309c256d60d7971ea14c27672fc0d10c5f303856d7bc48f8cc17ff
Deleted: sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e
Deleted: sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3
Untagged: 42school/marine-squad@sha256:2743413ee99183167014768298df9cf3fc5449eb2a735cc5386788087106b279
Deleted: sha256:bdb625dad464a80716adff58aba41920477baaf81ac36044d909460d5e27a00b
Deleted: sha256:c022d59383f9df31ba837f4fdc757d69d526d55b16845e1c7e5352208c95b2a4
Deleted: sha256:59b65ee35e8950106236aeb09117962168cde02e6f234bd9b0fb9854413b208e
Untagged: 42school/engineering-bay@sha256:5bc69a7b7ad5c5de54cffcfae09e94960ee66437ae0f7f2d270bde8e49559a62
Deleted: sha256:faa22fd5eedd1421ac84b89b728645f052914c560aae2cfd9431ff781fe5833a
Deleted: sha256:ea1c5846bc2910f1dab95d4a9ca3d40ff90cbb62749cacdb9746683781f84191
Deleted: sha256:db7ce7fc70649b8d53344fd4f696f830c53cdd1645b4ca8ad5b2852297e90235
Deleted: sha256:01e37fc3133816108efbffcee5c9648b934f4eb20ebc2c7b67a68976413105a3
Deleted: sha256:318da86d7b0a95efdbe3269ceacd7c438aa4701e72092da60dde4b0237d430e0
Deleted: sha256:7cbcbac42c44c6c38559e5df3a494f44987333c8023a40fec48df2fce1fc146b
Error response from daemon: conflict: unable to delete 458123c67b79 (must be forced) - image is referenced in multiple repositories
Error response from daemon: conflict: unable to delete 458123c67b79 (must be forced) - image is referenced in multiple repositories
il-j2% docker images -a -q
458123c67b79
458123c67b79
il-j2% docker images -a
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
rabbitmq            3                   458123c67b79        2 days ago          151MB
rabbitmq            latest              458123c67b79        2 days ago          151MB
il-j2% docker rmi -f $(docker images -a -q)
Untagged: rabbitmq:3
Untagged: rabbitmq:latest
Untagged: rabbitmq@sha256:1f91f51d065a8ba54d17675aace83a22b1a8d99880f7a602493b98cd9d950789
Deleted: sha256:458123c67b794bf274082a06d7b91a8a1dc7408a2ca9a2453de49144dc67c2de
Deleted: sha256:7ef5490d4e673333720d15d45908c9742b4f243ecc53b7850ed7fe223ed29ca9
Deleted: sha256:f9dd0ebde264afa62e026ef13a6f4ea295baddc1ccc9aa06a8558755e138411f
Deleted: sha256:114c18b0d2eb3f5a903056fd6bc231484f1769a4e54d25eb169462e3a004e5d9
Deleted: sha256:b1ad42ef15520d5d57126f1fd33883e84fa4c7034459042c8813ec4dedadb042
Deleted: sha256:28cab58fdc87edf680f2c9e9139784d247c329fe904323c30644a2f07edea314
Deleted: sha256:5948e2c971ab5c3c297f781986fcfbbf0a2175d6900c6ec40380fbb08fbcbed5
Deleted: sha256:4fc26b0b0c6903db3b4fe96856034a1bd9411ed963a96c1bc8f03f18ee92ac2a
Deleted: sha256:b53837dafdd21f67e607ae642ce49d326b0c30b39734b6710c682a50a9f932bf
Deleted: sha256:565879c6effe6a013e0b2e492f182b40049f1c083fc582ef61e49a98dca23f7e
Deleted: sha256:cc967c529ced563b7746b663d98248bc571afdb3c012019d7f54d6c092793b8b
Error: No such image: 458123c67b79
il-j2% docker images -a -q


34. Delete the Aiur virtual machine without using rm -rf.

il-j2% docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
Aiur   -        virtualbox   Running   tcp://192.168.99.111:2376           v19.03.5
Char   *        virtualbox   Running   tcp://192.168.99.109:2376           v19.03.5
il-j2% docker-machine rm -y Aiur
About to remove Aiur
WARNING: This action will delete both local reference and remote instance.
Successfully removed Aiur
il-j2% docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
Char   *        virtualbox   Running   tcp://192.168.99.109:2376           v19.03.5
il-j2% docker-machine rm -f Aiur
About to remove Aiur
WARNING: This action will delete both local reference and remote instance.
Error removing host "Aiur": Docker machine "Aiur" does not exist. Use "docker-machine ls" to list machines. Use "docker-machine create" to add a new one.
Can't remove "Aiur"
il-j2%







************************************
Exercise 00: vim/emacs

Exercise 00
Exercise 00: vim/emacs
Turn-in directory : ex00/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a
From an alpine image you’ll add to your container your favorite text editor, vim or emacs, that will launch along with your container.
============


il-j2% cat Dockerfile
FROM alpine

MAINTAINER apearl@student.21-school.ru

RUN apk update && \
apk upgrade && \
apk add vim

ENTRYPOINT vim
il-j2%




il-j2% pwd
/Users/apearl/Desktop/My_Docker/apearl/01_dockerfiles
il-j2% cd ex00
il-j2% docker build .
Sending build context to Docker daemon  4.096kB
Step 1/4 : FROM alpine
latest: Pulling from library/alpine
89d9c30c1d48: Pull complete
Digest: sha256:c19173c5ada610a5989151111163d28a67368362762534d8a8121ce95cf2bd5a
Status: Downloaded newer image for alpine:latest
---> 965ea09ff2eb
Step 2/4 : MAINTAINER apearl@student.21-school.ru
---> Running in 685b7d5dc162
Removing intermediate container 685b7d5dc162
---> f080a49489f2
Step 3/4 : RUN apk update &&     apk upgrade &&     apk add vim
---> Running in c7e425c71ccb
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
v3.10.3-77-g9739986c1e [http://dl-cdn.alpinelinux.org/alpine/v3.10/main]
v3.10.3-68-ge1e42c5d6c [http://dl-cdn.alpinelinux.org/alpine/v3.10/community]
OK: 10341 distinct packages available
(1/2) Upgrading busybox (1.30.1-r2 -> 1.30.1-r3)
Executing busybox-1.30.1-r3.post-upgrade
(2/2) Upgrading ssl_client (1.30.1-r2 -> 1.30.1-r3)
Executing busybox-1.30.1-r3.trigger
OK: 6 MiB in 14 packages
(1/5) Installing lua5.3-libs (5.3.5-r2)
(2/5) Installing ncurses-terminfo-base (6.1_p20190518-r0)
(3/5) Installing ncurses-terminfo (6.1_p20190518-r0)
(4/5) Installing ncurses-libs (6.1_p20190518-r0)
(5/5) Installing vim (8.1.1365-r0)
Executing busybox-1.30.1-r3.trigger
OK: 40 MiB in 19 packages
Removing intermediate container c7e425c71ccb
---> fb4d5d15d766
Step 4/4 : ENTRYPOINT vim
---> Running in 50b2cedca052
Removing intermediate container 50b2cedca052
---> 4dde880fc555
Successfully built 4dde880fc555


il-j2% docker run --rm -ti 4dde880fc555



**************************************************************************

VI.2 Exercise 01: BYOTSS

Exercise 01
Exercise 01: BYOTSS
Turn-in directory : ex01/
Files to turn in : Dockerfile + Scripts (if applicable)
Allowed functions : -
Notes : n/a
From a debian image you will add the appropriate sources to create a TeamSpeak server, that will launch along with your container. It will be deemed valid if at least one user can connect to it and engage in a normal discussion (no far-fetched setup), so be sure to create your Dockerfile with the right options. Your program should get the sources when it builds, they cannot be in your repository.
For the smarty-pants using the official docker image of TeamSpeak is
strictly FORBIDDEN, and will get you -42.

=================================
Answer:

il-j2% cat Dockerfile
FROM debian:10

MAINTAINER apearl@student.21-school.ru

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

EXPOSE 9987/udp 10011 30033

RUN apt-get update && apt-get upgrade -y && apt-get install -y wget bzip2 && \
wget https://files.teamspeak-services.com/releases/server/3.10.2/teamspeak3-server_linux_amd64-3.10.2.tar.bz2 && \
tar xvf *.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT ["sh", "ts3server_minimal_runscript.sh"]

=================================

https://support.teamspeakusa.com/index.php?/Knowledgebase/Article/View/344/16/how-to-accept-the-server-license-agreement-server--310
https://teamspeak.com/en/thanks-for-downloading-teamspeak/
https://files.teamspeak-services.com/releases/server/3.10.2/

docker-machine restart Char
eval $(docker-machine env Char)
docker build -t ex02 .
docker run -it --rm -p 30033:30033 -p 10011:10011 -p 9987:9987/udp  ex02
move Teamspeak icon to Desktop to create link
enter hit it.
when opened
enter CTRL S and enter IP of docker-machine ip Char
loginname= "serveradmin", password= "vunyM1G0"

token=d3HFW7rQq0fRV5ktHklHgZyv1s7YhOumRYGT0Vrw
￼<23:32:00> Trying to connect to server on 192.168.99.109
￼<23:32:01> Welcome to TeamSpeak, check www.teamspeak.com for latest information
￼<23:32:01> Connected to Server: "TeamSpeak ]I[ Server"
￼<23:32:12> "serveradmin" was added to server group "Server Admin" by "TeamSpeak ]I[ Server".
￼<23:32:12> Server properties have been edited by "TeamSpeak ]I[ Server"

il-j2% docker build .
Sending build context to Docker daemon  15.36kB
Step 1/8 : FROM debian:10
---> 67e34c1c9477
Step 2/8 : MAINTAINER apearl@student.21-school.ru
---> Using cache
---> 17385985390b
Step 3/8 : ENV TS3SERVER_LICENSE=accept
---> Using cache
---> 539b30dd82a5
Step 4/8 : WORKDIR /home/teamspeak
---> Using cache
---> 656d7677d29a
Step 5/8 : EXPOSE 9987/udp 10011 30033
---> Using cache
---> 95eceb273fbc
Step 6/8 : RUN apt-get update && apt-get upgrade -y && apt-get install -y wget bzip2 &&     wget https://files.teamspeak-services.com/releases/server/3.10.2/teamspeak3-server_linux_amd64-3.10.2.tar.bz2 &&     tar xvf *.tar.bz2
---> Running in 0d9cf08c6d1a
Get:1 http://security-cdn.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:2 http://cdn-fastly.deb.debian.org/debian buster InRelease [122 kB]
Get:4 http://security-cdn.debian.org/debian-security buster/updates/main amd64 Packages [162 kB]
Get:3 http://cdn-fastly.deb.debian.org/debian buster-updates InRelease [49.3 kB]
Get:5 http://cdn-fastly.deb.debian.org/debian buster/main amd64 Packages [7908 kB]
Get:6 http://cdn-fastly.deb.debian.org/debian buster-updates/main amd64 Packages [5792 B]
Fetched 8312 kB in 3s (2809 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
Calculating upgrade...
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
ca-certificates libpcre2-8-0 libpsl5 libssl1.1 openssl publicsuffix
Suggested packages:
bzip2-doc
The following NEW packages will be installed:
bzip2 ca-certificates libpcre2-8-0 libpsl5 libssl1.1 openssl publicsuffix
wget
0 upgraded, 8 newly installed, 0 to remove and 0 not upgraded.
Need to get 3872 kB of archives.
After this operation, 10.6 MB of additional disk space will be used.
Get:1 http://cdn-fastly.deb.debian.org/debian buster/main amd64 bzip2 amd64 1.0.6-9.2~deb10u1 [48.4 kB]
Get:2 http://cdn-fastly.deb.debian.org/debian buster/main amd64 libpcre2-8-0 amd64 10.32-5 [213 kB]
Get:3 http://cdn-fastly.deb.debian.org/debian buster/main amd64 libpsl5 amd64 0.20.2-2 [53.7 kB]
Get:4 http://cdn-fastly.deb.debian.org/debian buster/main amd64 wget amd64 1.20.1-1.1 [902 kB]
Get:5 http://cdn-fastly.deb.debian.org/debian buster/main amd64 libssl1.1 amd64 1.1.1d-0+deb10u2 [1538 kB]
Get:6 http://cdn-fastly.deb.debian.org/debian buster/main amd64 openssl amd64 1.1.1d-0+deb10u2 [843 kB]
Get:7 http://cdn-fastly.deb.debian.org/debian buster/main amd64 ca-certificates all 20190110 [157 kB]
Get:8 http://cdn-fastly.deb.debian.org/debian buster/main amd64 publicsuffix all 20190415.1030-1 [116 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 3872 kB in 1s (3792 kB/s)
Selecting previously unselected package bzip2.
(Reading database ... 6674 files and directories currently installed.)
Preparing to unpack .../0-bzip2_1.0.6-9.2~deb10u1_amd64.deb ...
Unpacking bzip2 (1.0.6-9.2~deb10u1) ...
Selecting previously unselected package libpcre2-8-0:amd64.
Preparing to unpack .../1-libpcre2-8-0_10.32-5_amd64.deb ...
Unpacking libpcre2-8-0:amd64 (10.32-5) ...
Selecting previously unselected package libpsl5:amd64.
Preparing to unpack .../2-libpsl5_0.20.2-2_amd64.deb ...
Unpacking libpsl5:amd64 (0.20.2-2) ...
Selecting previously unselected package wget.
Preparing to unpack .../3-wget_1.20.1-1.1_amd64.deb ...
Unpacking wget (1.20.1-1.1) ...
Selecting previously unselected package libssl1.1:amd64.
Preparing to unpack .../4-libssl1.1_1.1.1d-0+deb10u2_amd64.deb ...
Unpacking libssl1.1:amd64 (1.1.1d-0+deb10u2) ...
Selecting previously unselected package openssl.
Preparing to unpack .../5-openssl_1.1.1d-0+deb10u2_amd64.deb ...
Unpacking openssl (1.1.1d-0+deb10u2) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../6-ca-certificates_20190110_all.deb ...
Unpacking ca-certificates (20190110) ...
Selecting previously unselected package publicsuffix.
Preparing to unpack .../7-publicsuffix_20190415.1030-1_all.deb ...
Unpacking publicsuffix (20190415.1030-1) ...
Setting up libpsl5:amd64 (0.20.2-2) ...
Setting up libssl1.1:amd64 (1.1.1d-0+deb10u2) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.28.1 /usr/local/share/perl/5.28.1 /usr/lib/x86_64-linux-gnu/perl5/5.28 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.28 /usr/share/perl/5.28 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up bzip2 (1.0.6-9.2~deb10u1) ...
Setting up libpcre2-8-0:amd64 (10.32-5) ...
Setting up openssl (1.1.1d-0+deb10u2) ...
Setting up publicsuffix (20190415.1030-1) ...
Setting up wget (1.20.1-1.1) ...
Setting up ca-certificates (20190110) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.28.1 /usr/local/share/perl/5.28.1 /usr/lib/x86_64-linux-gnu/perl5/5.28 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.28 /usr/share/perl/5.28 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Updating certificates in /etc/ssl/certs...
128 added, 0 removed; done.
Processing triggers for libc-bin (2.28-10) ...
Processing triggers for ca-certificates (20190110) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
--2019-12-14 13:34:04--  https://files.teamspeak-services.com/releases/server/3.10.2/teamspeak3-server_linux_amd64-3.10.2.tar.bz2
Resolving files.teamspeak-services.com (files.teamspeak-services.com)... 151.139.128.10
Connecting to files.teamspeak-services.com (files.teamspeak-services.com)|151.139.128.10|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9242362 (8.8M) [application/x-tar]
Saving to: 'teamspeak3-server_linux_amd64-3.10.2.tar.bz2'

0K .......... .......... .......... .......... ..........  0% 1.60M 5s
50K .......... .......... .......... .......... ..........  1% 2.50M 4s
100K .......... .......... .......... .......... ..........  1% 2.35M 4s
150K .......... .......... .......... .......... ..........  2% 2.32M 4s
200K .......... .......... .......... .......... ..........  2% 3.57M 4s
250K .......... .......... .......... .......... ..........  3% 2.40M 4s
300K .......... .......... .......... .......... ..........  3% 2.32M 4s
350K .......... .......... .......... .......... ..........  4% 3.73M 3s
400K .......... .......... .......... .......... ..........  4% 2.66M 3s
450K .......... .......... .......... .......... ..........  5% 2.31M 3s
500K .......... .......... .......... .......... ..........  6% 2.55M 3s
550K .......... .......... .......... .......... ..........  6% 1.86M 3s
600K .......... .......... .......... .......... ..........  7% 2.62M 3s
650K .......... .......... .......... .......... ..........  7% 2.83M 3s
700K .......... .......... .......... .......... ..........  8% 97.9M 3s
750K .......... .......... .......... .......... ..........  8% 2.25M 3s
800K .......... .......... .......... .......... ..........  9% 2.50M 3s
850K .......... .......... .......... .......... ..........  9% 2.38M 3s
900K .......... .......... .......... .......... .......... 10% 2.30M 3s
950K .......... .......... .......... .......... .......... 11% 17.6M 3s
1000K .......... .......... .......... .......... .......... 11% 2.39M 3s
1050K .......... .......... .......... .......... .......... 12% 2.44M 3s
1100K .......... .......... .......... .......... .......... 12% 2.58M 3s
1150K .......... .......... .......... .......... .......... 13% 3.15M 3s
1200K .......... .......... .......... .......... .......... 13% 6.58M 3s
1250K .......... .......... .......... .......... .......... 14% 2.34M 3s
1300K .......... .......... .......... .......... .......... 14% 3.55M 3s
1350K .......... .......... .......... .......... .......... 15% 1.12M 3s
1400K .......... .......... .......... .......... .......... 16%  163M 3s
1450K .......... .......... .......... .......... .......... 16%  185M 3s
1500K .......... .......... .......... .......... .......... 17% 7.82M 3s
1550K .......... .......... .......... .......... .......... 17% 2.55M 3s
1600K .......... .......... .......... .......... .......... 18% 45.4M 2s
1650K .......... .......... .......... .......... .......... 18% 2.38M 2s
1700K .......... .......... .......... .......... .......... 19% 2.45M 2s
1750K .......... .......... .......... .......... .......... 19% 46.8M 2s
1800K .......... .......... .......... .......... .......... 20% 2.40M 2s
1850K .......... .......... .......... .......... .......... 21% 2.37M 2s
1900K .......... .......... .......... .......... .......... 21% 28.8M 2s
1950K .......... .......... .......... .......... .......... 22% 1.95M 2s
2000K .......... .......... .......... .......... .......... 22% 98.0M 2s
2050K .......... .......... .......... .......... .......... 23% 3.19M 2s
2100K .......... .......... .......... .......... .......... 23% 18.7M 2s
2150K .......... .......... .......... .......... .......... 24% 1.88M 2s
2200K .......... .......... .......... .......... .......... 24% 22.9M 2s
2250K .......... .......... .......... .......... .......... 25% 2.38M 2s
2300K .......... .......... .......... .......... .......... 26%  117M 2s
2350K .......... .......... .......... .......... .......... 26% 3.09M 2s
2400K .......... .......... .......... .......... .......... 27% 25.8M 2s
2450K .......... .......... .......... .......... .......... 27% 2.80M 2s
2500K .......... .......... .......... .......... .......... 28% 21.8M 2s
2550K .......... .......... .......... .......... .......... 28% 2.68M 2s
2600K .......... .......... .......... .......... .......... 29% 20.2M 2s
2650K .......... .......... .......... .......... .......... 29% 3.06M 2s
2700K .......... .......... .......... .......... .......... 30% 17.6M 2s
2750K .......... .......... .......... .......... .......... 31% 2.76M 2s
2800K .......... .......... .......... .......... .......... 31% 12.2M 2s
2850K .......... .......... .......... .......... .......... 32% 2.90M 2s
2900K .......... .......... .......... .......... .......... 32% 15.9M 2s
2950K .......... .......... .......... .......... .......... 33% 2.77M 2s
3000K .......... .......... .......... .......... .......... 33% 14.1M 2s
3050K .......... .......... .......... .......... .......... 34% 2.58M 2s
3100K .......... .......... .......... .......... .......... 34% 29.1M 2s
3150K .......... .......... .......... .......... .......... 35% 2.64M 2s
3200K .......... .......... .......... .......... .......... 36% 14.2M 2s
3250K .......... .......... .......... .......... .......... 36% 2.96M 2s
3300K .......... .......... .......... .......... .......... 37% 11.8M 2s
3350K .......... .......... .......... .......... .......... 37% 63.3M 2s
3400K .......... .......... .......... .......... .......... 38% 2.72M 2s
3450K .......... .......... .......... .......... .......... 38% 13.1M 2s
3500K .......... .......... .......... .......... .......... 39% 3.07M 1s
3550K .......... .......... .......... .......... .......... 39% 11.1M 1s
3600K .......... .......... .......... .......... .......... 40% 3.01M 1s
3650K .......... .......... .......... .......... .......... 40% 15.3M 1s
3700K .......... .......... .......... .......... .......... 41% 31.3M 1s
3750K .......... .......... .......... .......... .......... 42% 2.93M 1s
3800K .......... .......... .......... .......... .......... 42% 11.2M 1s
3850K .......... .......... .......... .......... .......... 43% 3.07M 1s
3900K .......... .......... .......... .......... .......... 43% 11.0M 1s
3950K .......... .......... .......... .......... .......... 44% 46.3M 1s
4000K .......... .......... .......... .......... .......... 44% 2.78M 1s
4050K .......... .......... .......... .......... .......... 45% 20.9M 1s
4100K .......... .......... .......... .......... .......... 45% 2.70M 1s
4150K .......... .......... .......... .......... .......... 46% 8.03M 1s
4200K .......... .......... .......... .......... .......... 47%  162M 1s
4250K .......... .......... .......... .......... .......... 47% 3.77M 1s
4300K .......... .......... .......... .......... .......... 48% 11.3M 1s
4350K .......... .......... .......... .......... .......... 48% 2.95M 1s
4400K .......... .......... .......... .......... .......... 49% 15.3M 1s
4450K .......... .......... .......... .......... .......... 49% 29.3M 1s
4500K .......... .......... .......... .......... .......... 50% 3.01M 1s
4550K .......... .......... .......... .......... .......... 50% 13.0M 1s
4600K .......... .......... .......... .......... .......... 51% 30.6M 1s
4650K .......... .......... .......... .......... .......... 52% 2.35M 1s
4700K .......... .......... .......... .......... .......... 52% 19.1M 1s
4750K .......... .......... .......... .......... .......... 53% 3.53M 1s
4800K .......... .......... .......... .......... .......... 53% 14.8M 1s
4850K .......... .......... .......... .......... .......... 54% 22.0M 1s
4900K .......... .......... .......... .......... .......... 54% 2.82M 1s
4950K .......... .......... .......... .......... .......... 55% 44.0M 1s
5000K .......... .......... .......... .......... .......... 55% 22.8M 1s
5050K .......... .......... .......... .......... .......... 56% 2.63M 1s
5100K .......... .......... .......... .......... .......... 57% 18.0M 1s
5150K .......... .......... .......... .......... .......... 57% 86.1M 1s
5200K .......... .......... .......... .......... .......... 58% 2.52M 1s
5250K .......... .......... .......... .......... .......... 58% 66.7M 1s
5300K .......... .......... .......... .......... .......... 59% 3.04M 1s
5350K .......... .......... .......... .......... .......... 59% 16.6M 1s
5400K .......... .......... .......... .......... .......... 60% 19.4M 1s
5450K .......... .......... .......... .......... .......... 60% 2.46M 1s
5500K .......... .......... .......... .......... .......... 61% 23.6M 1s
5550K .......... .......... .......... .......... .......... 62%  121M 1s
5600K .......... .......... .......... .......... .......... 62% 3.24M 1s
5650K .......... .......... .......... .......... .......... 63% 12.8M 1s
5700K .......... .......... .......... .......... .......... 63%  186M 1s
5750K .......... .......... .......... .......... .......... 64% 2.43M 1s
5800K .......... .......... .......... .......... .......... 64% 38.3M 1s
5850K .......... .......... .......... .......... .......... 65% 37.9M 1s
5900K .......... .......... .......... .......... .......... 65% 3.82M 1s
5950K .......... .......... .......... .......... .......... 66% 11.2M 1s
6000K .......... .......... .......... .......... .......... 67% 62.7M 1s
6050K .......... .......... .......... .......... .......... 67% 2.33M 1s
6100K .......... .......... .......... .......... .......... 68% 14.3M 1s
6150K .......... .......... .......... .......... .......... 68%  171M 1s
6200K .......... .......... .......... .......... .......... 69%  132M 1s
6250K .......... .......... .......... .......... .......... 69% 2.27M 1s
6300K .......... .......... .......... .......... .......... 70% 75.6M 1s
6350K .......... .......... .......... .......... .......... 70%  116M 1s
6400K .......... .......... .......... .......... .......... 71% 1.25M 1s
6450K .......... .......... .......... .......... .......... 72% 70.7M 1s
6500K .......... .......... .......... .......... .......... 72%  135M 1s
6550K .......... .......... .......... .......... .......... 73%  142M 1s
6600K .......... .......... .......... .......... .......... 73%  191M 1s
6650K .......... .......... .......... .......... .......... 74%  215M 1s
6700K .......... .......... .......... .......... .......... 74% 3.45M 0s
6750K .......... .......... .......... .......... .......... 75% 15.4M 0s
6800K .......... .......... .......... .......... .......... 75% 65.8M 0s
6850K .......... .......... .......... .......... .......... 76% 3.33M 0s
6900K .......... .......... .......... .......... .......... 77% 7.76M 0s
6950K .......... .......... .......... .......... .......... 77%  180M 0s
7000K .......... .......... .......... .......... .......... 78%  217M 0s
7050K .......... .......... .......... .......... .......... 78% 1.96M 0s
7100K .......... .......... .......... .......... .......... 79% 13.1M 0s
7150K .......... .......... .......... .......... .......... 79% 72.5M 0s
7200K .......... .......... .......... .......... .......... 80% 3.61M 0s
7250K .......... .......... .......... .......... .......... 80% 63.2M 0s
7300K .......... .......... .......... .......... .......... 81%  113M 0s
7350K .......... .......... .......... .......... .......... 81% 2.91M 0s
7400K .......... .......... .......... .......... .......... 82% 75.2M 0s
7450K .......... .......... .......... .......... .......... 83%  119M 0s
7500K .......... .......... .......... .......... .......... 83%  134M 0s
7550K .......... .......... .......... .......... .......... 84% 2.73M 0s
7600K .......... .......... .......... .......... .......... 84% 38.9M 0s
7650K .......... .......... .......... .......... .......... 85% 43.8M 0s
7700K .......... .......... .......... .......... .......... 85% 2.90M 0s
7750K .......... .......... .......... .......... .......... 86% 9.48M 0s
7800K .......... .......... .......... .......... .......... 86%  128M 0s
7850K .......... .......... .......... .......... .......... 87%  100M 0s
7900K .......... .......... .......... .......... .......... 88% 2.83M 0s
7950K .......... .......... .......... .......... .......... 88% 81.8M 0s
8000K .......... .......... .......... .......... .......... 89%  124M 0s
8050K .......... .......... .......... .......... .......... 89% 16.2M 0s
8100K .......... .......... .......... .......... .......... 90% 2.74M 0s
8150K .......... .......... .......... .......... .......... 90% 43.3M 0s
8200K .......... .......... .......... .......... .......... 91%  124M 0s
8250K .......... .......... .......... .......... .......... 91% 2.33M 0s
8300K .......... .......... .......... .......... .......... 92% 30.0M 0s
8350K .......... .......... .......... .......... .......... 93%  107M 0s
8400K .......... .......... .......... .......... .......... 93%  163M 0s
8450K .......... .......... .......... .......... .......... 94% 1.80M 0s
8500K .......... .......... .......... .......... .......... 94% 7.24M 0s
8550K .......... .......... .......... .......... .......... 95% 80.5M 0s
8600K .......... .......... .......... .......... .......... 95% 1.87M 0s
8650K .......... .......... .......... .......... .......... 96% 48.7M 0s
8700K .......... .......... .......... .......... .......... 96%  233M 0s
8750K .......... .......... .......... .......... .......... 97%  220M 0s
8800K .......... .......... .......... .......... .......... 98%  190M 0s
8850K .......... .......... .......... .......... .......... 98%  222M 0s
8900K .......... .......... .......... .......... .......... 99%  210M 0s
8950K .......... .......... .......... .......... .......... 99%  175M 0s
9000K .......... .......... .....                           100% 31.0M=1.7s

2019-12-14 13:34:06 (5.14 MB/s) - 'teamspeak3-server_linux_amd64-3.10.2.tar.bz2' saved [9242362/9242362]

teamspeak3-server_linux_amd64/
teamspeak3-server_linux_amd64/LICENSE
teamspeak3-server_linux_amd64/tsdns/
teamspeak3-server_linux_amd64/tsdns/tsdnsserver
teamspeak3-server_linux_amd64/tsdns/README
teamspeak3-server_linux_amd64/tsdns/USAGE
teamspeak3-server_linux_amd64/tsdns/tsdns_settings.ini.sample
teamspeak3-server_linux_amd64/sql/
teamspeak3-server_linux_amd64/sql/group_member_delete.sql
teamspeak3-server_linux_amd64/sql/update_sqlite_32.sql
teamspeak3-server_linux_amd64/sql/update_28.sql
teamspeak3-server_linux_amd64/sql/perm_insert_bulk.sql
teamspeak3-server_linux_amd64/sql/channel_delete.sql
teamspeak3-server_linux_amd64/sql/update_21.sql
teamspeak3-server_linux_amd64/sql/complain_delete_prune.sql
teamspeak3-server_linux_amd64/sql/groups_get_by_serverid_type.sql
teamspeak3-server_linux_amd64/sql/update_16.sql
teamspeak3-server_linux_amd64/sql/permission_load_channel_group_total.sql
teamspeak3-server_linux_amd64/sql/update_15.sql
teamspeak3-server_linux_amd64/sql/client_clear_traffic_stats.sql
teamspeak3-server_linux_amd64/sql/ban_list.sql
teamspeak3-server_linux_amd64/sql/client_get_by_id.sql
teamspeak3-server_linux_amd64/sql/update_mariadb_32.sql
teamspeak3-server_linux_amd64/sql/revocations_delete.sql
teamspeak3-server_linux_amd64/sql/server_update_machine_id.sql
teamspeak3-server_linux_amd64/sql/perm_update_get_groups.sql
teamspeak3-server_linux_amd64/sql/update_27.sql
teamspeak3-server_linux_amd64/sql/group_member_get.sql
teamspeak3-server_linux_amd64/sql/message_update_flag.sql
teamspeak3-server_linux_amd64/sql/client_update_stats.sql
teamspeak3-server_linux_amd64/sql/groups_get.sql
teamspeak3-server_linux_amd64/sql/channel_insert.sql
teamspeak3-server_linux_amd64/sql/client_count_queries.sql
teamspeak3-server_linux_amd64/sql/group_member_delete_by_groupid.sql
teamspeak3-server_linux_amd64/sql/update_24.sql
teamspeak3-server_linux_amd64/sql/binding_delete.sql
teamspeak3-server_linux_amd64/sql/channel_insert_bulk.sql
teamspeak3-server_linux_amd64/sql/update_23.sql
teamspeak3-server_linux_amd64/sql/info_get_by_ident.sql
teamspeak3-server_linux_amd64/sql/client_delete_prune.sql
teamspeak3-server_linux_amd64/sql/client_get_queries_per_server_limit.sql
teamspeak3-server_linux_amd64/sql/properties_delete_by_string_id.sql
teamspeak3-server_linux_amd64/sql/temporary_password_list.sql
teamspeak3-server_linux_amd64/sql/channel_insert_bulk_fixup.sql
teamspeak3-server_linux_amd64/sql/perm_group_copy.sql
teamspeak3-server_linux_amd64/sql/custom_insert.sql
teamspeak3-server_linux_amd64/sql/complain_get_by_serverid.sql
teamspeak3-server_linux_amd64/sql/group_insert.sql
teamspeak3-server_linux_amd64/sql/client_insert.sql
teamspeak3-server_linux_amd64/sql/perm_copy_default_permissions.sql
teamspeak3-server_linux_amd64/sql/properties_insert_by_string_id.sql
teamspeak3-server_linux_amd64/sql/integration_action_list.sql
teamspeak3-server_linux_amd64/sql/server_list_detailed.sql
teamspeak3-server_linux_amd64/sql/token_list.sql
teamspeak3-server_linux_amd64/sql/temporary_password_delete.sql
teamspeak3-server_linux_amd64/sql/update_20.sql
teamspeak3-server_linux_amd64/sql/complain_insert.sql
teamspeak3-server_linux_amd64/sql/update_31.sql
teamspeak3-server_linux_amd64/sql/client_get_by_serverid_bulk.sql
teamspeak3-server_linux_amd64/sql/update_26.sql
teamspeak3-server_linux_amd64/sql/channel_update_parentid.sql
teamspeak3-server_linux_amd64/sql/client_remove_query_login.sql
teamspeak3-server_linux_amd64/sql/client_get_by_name_or_uid.sql
teamspeak3-server_linux_amd64/sql/update_19.sql
teamspeak3-server_linux_amd64/sql/complain_delete_all.sql
teamspeak3-server_linux_amd64/sql/token_delete_by_key.sql
teamspeak3-server_linux_amd64/sql/group_id_get_by_name.sql
teamspeak3-server_linux_amd64/sql/server_list_by_machine_id_detailed.sql
teamspeak3-server_linux_amd64/sql/integration_action_insert.sql
teamspeak3-server_linux_amd64/sql/group_rename.sql
teamspeak3-server_linux_amd64/sql/integration_list.sql
teamspeak3-server_linux_amd64/sql/update_mariadb_27.sql
teamspeak3-server_linux_amd64/sql/channel_server_list_properties_bulk.sql
teamspeak3-server_linux_amd64/sql/integration_insert.sql
teamspeak3-server_linux_amd64/sql/update_mariadb_29.sql
teamspeak3-server_linux_amd64/sql/ban_delete.sql
teamspeak3-server_linux_amd64/sql/message_insert.sql
teamspeak3-server_linux_amd64/sql/group_members_get_by_groupid.sql
teamspeak3-server_linux_amd64/sql/properties_insert_by_id.sql
teamspeak3-server_linux_amd64/sql/integration_action_delete.sql
teamspeak3-server_linux_amd64/sql/perm_delete_by_groupid.sql
teamspeak3-server_linux_amd64/sql/update_permissions_12.sql
teamspeak3-server_linux_amd64/sql/update_mariadb_26.sql
teamspeak3-server_linux_amd64/sql/defaults.sql
teamspeak3-server_linux_amd64/sql/update_22.sql
teamspeak3-server_linux_amd64/sql/permission_load_server_and_channel_group_total.sql
teamspeak3-server_linux_amd64/sql/message_delete.sql
teamspeak3-server_linux_amd64/sql/perm_delete_by_serverid.sql
teamspeak3-server_linux_amd64/sql/perm_delete_by_permid.sql
teamspeak3-server_linux_amd64/sql/perm_group_perm_copy.sql
teamspeak3-server_linux_amd64/sql/update_13.sql
teamspeak3-server_linux_amd64/sql/client_get.sql
teamspeak3-server_linux_amd64/sql/custom_get_by_ident.sql
teamspeak3-server_linux_amd64/sql/group_member_detail_get_by_groupid.sql
teamspeak3-server_linux_amd64/sql/client_delete.sql
teamspeak3-server_linux_amd64/sql/perm_rename.sql
teamspeak3-server_linux_amd64/sql/group_members_get_by_id.sql
teamspeak3-server_linux_amd64/sql/server_snapshot_delete.sql
teamspeak3-server_linux_amd64/sql/group_member_insert.sql
teamspeak3-server_linux_amd64/sql/groups_get_by_serverid.sql
teamspeak3-server_linux_amd64/sql/custom_get_by_id.sql
teamspeak3-server_linux_amd64/sql/ban_insert.sql
teamspeak3-server_linux_amd64/sql/channel_insert_bulk_mapping.sql
teamspeak3-server_linux_amd64/sql/server_delete_get_qa_clients.sql
teamspeak3-server_linux_amd64/sql/permission_load_other.sql
teamspeak3-server_linux_amd64/sql/message_get_unread_by_clientid.sql
teamspeak3-server_linux_amd64/sql/server_update_port.sql
teamspeak3-server_linux_amd64/sql/bindings_insert.sql
teamspeak3-server_linux_amd64/sql/update_25.sql
teamspeak3-server_linux_amd64/sql/channel_properties_bulk_insert.sql
teamspeak3-server_linux_amd64/sql/custom_delete.sql
teamspeak3-server_linux_amd64/sql/client_get_by_uid.sql
teamspeak3-server_linux_amd64/sql/info_insert.sql
teamspeak3-server_linux_amd64/sql/temporary_password_insert.sql
teamspeak3-server_linux_amd64/sql/server_update_autostart.sql
teamspeak3-server_linux_amd64/sql/revocations_getlist.sql
teamspeak3-server_linux_amd64/sql/update_mariadb_28.sql
teamspeak3-server_linux_amd64/sql/server_get_byport.sql
teamspeak3-server_linux_amd64/sql/perm_group_get_mapping.sql
teamspeak3-server_linux_amd64/sql/update_sqlite_29.sql
teamspeak3-server_linux_amd64/sql/client_get_by_serverid_limit.sql
teamspeak3-server_linux_amd64/sql/group_delete.sql
teamspeak3-server_linux_amd64/sql/update_12.sql
teamspeak3-server_linux_amd64/sql/updates_and_fixes/
teamspeak3-server_linux_amd64/sql/updates_and_fixes/mariadb_fix_latin_utf8.sql
teamspeak3-server_linux_amd64/sql/updates_and_fixes/convert_mysql_to_mariadb.sql
teamspeak3-server_linux_amd64/sql/create_mariadb/
teamspeak3-server_linux_amd64/sql/create_mariadb/drop_tables.sql
teamspeak3-server_linux_amd64/sql/create_mariadb/create_tables.sql
teamspeak3-server_linux_amd64/sql/channel_server_list.sql
teamspeak3-server_linux_amd64/sql/permission_load_server_group_total.sql
teamspeak3-server_linux_amd64/sql/perm_insert.sql
teamspeak3-server_linux_amd64/sql/message_get_by_clientid.sql
teamspeak3-server_linux_amd64/sql/properties_list_by_id.sql
teamspeak3-server_linux_amd64/sql/perm_get_by_serverid.sql
teamspeak3-server_linux_amd64/sql/client_get_queries_limit.sql
teamspeak3-server_linux_amd64/sql/clientid_get_by_name_pw_serverid.sql
teamspeak3-server_linux_amd64/sql/update_29.sql
teamspeak3-server_linux_amd64/sql/client_update_login_info.sql
teamspeak3-server_linux_amd64/sql/server_delete.sql
teamspeak3-server_linux_amd64/sql/bindings_list.sql
teamspeak3-server_linux_amd64/sql/custom_update.sql
teamspeak3-server_linux_amd64/sql/client_insert_bulk.sql
teamspeak3-server_linux_amd64/sql/update_14.sql
teamspeak3-server_linux_amd64/sql/client_insert_query.sql
teamspeak3-server_linux_amd64/sql/update_sqlite_28.sql
teamspeak3-server_linux_amd64/sql/client_insert_bulk_mapping.sql
teamspeak3-server_linux_amd64/sql/server_update_traffic_stats.sql
teamspeak3-server_linux_amd64/sql/channel_delete_bulk.sql
teamspeak3-server_linux_amd64/sql/update_30.sql
teamspeak3-server_linux_amd64/sql/complain_delete.sql
teamspeak3-server_linux_amd64/sql/client_get_by_serverid.sql
teamspeak3-server_linux_amd64/sql/server_snapshot_delete_failed.sql
teamspeak3-server_linux_amd64/sql/perm_get_by_id.sql
teamspeak3-server_linux_amd64/sql/create_sqlite/
teamspeak3-server_linux_amd64/sql/create_sqlite/drop_tables.sql
teamspeak3-server_linux_amd64/sql/create_sqlite/create_tables.sql
teamspeak3-server_linux_amd64/sql/revocations_insert_bulk.sql
teamspeak3-server_linux_amd64/sql/properties_list_by_string_id.sql
teamspeak3-server_linux_amd64/sql/message_list_by_clientid.sql
teamspeak3-server_linux_amd64/sql/token_insert.sql
teamspeak3-server_linux_amd64/sql/update_32.sql
teamspeak3-server_linux_amd64/sql/client_update_traffic_stats.sql
teamspeak3-server_linux_amd64/sql/group_insert_bulk_snapshot_get_mapping.sql
teamspeak3-server_linux_amd64/sql/client_properties_bulk_insert.sql
teamspeak3-server_linux_amd64/sql/group_member_insert_bulk.sql
teamspeak3-server_linux_amd64/sql/server_get_byid.sql
teamspeak3-server_linux_amd64/sql/properties_delete_by_id.sql
teamspeak3-server_linux_amd64/sql/integration_delete.sql
teamspeak3-server_linux_amd64/sql/server_list.sql
teamspeak3-server_linux_amd64/sql/update_18.sql
teamspeak3-server_linux_amd64/sql/server_list_by_machine_id.sql
teamspeak3-server_linux_amd64/sql/server_clear_traffic_stats.sql
teamspeak3-server_linux_amd64/sql/server_insert.sql
teamspeak3-server_linux_amd64/sql/client_count_by_serverid.sql
teamspeak3-server_linux_amd64/sql/update_sqlite_27.sql
teamspeak3-server_linux_amd64/sql/update_database_version.sql
teamspeak3-server_linux_amd64/sql/client_update_name.sql
teamspeak3-server_linux_amd64/sql/group_insert_bulk_snapshot.sql
teamspeak3-server_linux_amd64/sql/client_count_queries_per_server.sql
teamspeak3-server_linux_amd64/sql/clientid_get_by_name_pw.sql
teamspeak3-server_linux_amd64/sql/info_delete.sql
teamspeak3-server_linux_amd64/sql/group_members_get_by_serverid.sql
teamspeak3-server_linux_amd64/sql/update_17.sql
teamspeak3-server_linux_amd64/sql/token_get_by_key.sql
teamspeak3-server_linux_amd64/ts3server
teamspeak3-server_linux_amd64/libts3db_sqlite3.so
teamspeak3-server_linux_amd64/doc/
teamspeak3-server_linux_amd64/doc/serverquery/
teamspeak3-server_linux_amd64/doc/serverquery/serverquery.html
teamspeak3-server_linux_amd64/doc/serverquery/TeamSpeak_Logo.png
teamspeak3-server_linux_amd64/doc/serverquery/stylesheet.css
teamspeak3-server_linux_amd64/doc/server_quickstart.txt
teamspeak3-server_linux_amd64/doc/privilegekey_guide.txt
teamspeak3-server_linux_amd64/doc/accounting.txt
teamspeak3-server_linux_amd64/doc/permissiondoc.txt
teamspeak3-server_linux_amd64/doc/server_upgrade.txt
teamspeak3-server_linux_amd64/doc/update_mysql_to_mariadb.txt
teamspeak3-server_linux_amd64/ts3server_startscript.sh
teamspeak3-server_linux_amd64/libts3db_mariadb.so
teamspeak3-server_linux_amd64/redist/
teamspeak3-server_linux_amd64/redist/libmariadb.so.2
teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh
teamspeak3-server_linux_amd64/libts3_ssh.so
teamspeak3-server_linux_amd64/serverquerydocs/
teamspeak3-server_linux_amd64/serverquerydocs/tokendelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientgetnamefromuid.txt
teamspeak3-server_linux_amd64/serverquerydocs/quit.txt
teamspeak3-server_linux_amd64/serverquerydocs/instanceedit.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftgetfilelist.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientpermlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/tokenuse.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverstart.txt
teamspeak3-server_linux_amd64/serverquerydocs/servernotifyregister.txt
teamspeak3-server_linux_amd64/serverquerydocs/complaindel.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/logview.txt
teamspeak3-server_linux_amd64/serverquerydocs/channeldelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/permoverview.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupdel.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientgetnamefromdbid.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupdelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/logadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdbedit.txt
teamspeak3-server_linux_amd64/serverquerydocs/querylogindel.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdelservergroup.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftdeletefile.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientkick.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelclientdelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/servertemppasswordadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientgetuidfromclid.txt
teamspeak3-server_linux_amd64/serverquerydocs/messageget.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverrequestconnectioninfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/serversnapshotdeploy.txt
teamspeak3-server_linux_amd64/serverquerydocs/customsearch.txt
teamspeak3-server_linux_amd64/serverquerydocs/privilegekeyuse.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupdelclient.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftgetfileinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/custominfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/banclient.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientgetdbidfromuid.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergrouppermlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/instanceinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/channeldelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/serversnapshotcreate.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupdel.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupcopy.txt
teamspeak3-server_linux_amd64/serverquerydocs/complainadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelclientaddperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdbfind.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdbdelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupclientlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupaddperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/banlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/tokenlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/messageadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/queryloginlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/setclientchannelgroup.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientaddperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelcreate.txt
teamspeak3-server_linux_amd64/serverquerydocs/sendtextmessage.txt
teamspeak3-server_linux_amd64/serverquerydocs/logout.txt
teamspeak3-server_linux_amd64/serverquerydocs/bindinglist.txt
teamspeak3-server_linux_amd64/serverquerydocs/hostinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/messageupdateflag.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverdelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelclientpermlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/complaindelall.txt
teamspeak3-server_linux_amd64/serverquerydocs/bandel.txt
teamspeak3-server_linux_amd64/serverquerydocs/queryloginadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergrouprename.txt
teamspeak3-server_linux_amd64/serverquerydocs/help.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupcopy.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/servertemppasswordlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/channellist.txt
teamspeak3-server_linux_amd64/serverquerydocs/privilegekeydelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/permfind.txt
teamspeak3-server_linux_amd64/serverquerydocs/serveredit.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverstop.txt
teamspeak3-server_linux_amd64/serverquerydocs/privilegekeylist.txt
teamspeak3-server_linux_amd64/serverquerydocs/use.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelmove.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdblist.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/permidgetbyname.txt
teamspeak3-server_linux_amd64/serverquerydocs/permreset.txt
teamspeak3-server_linux_amd64/serverquerydocs/serveridgetbyport.txt
teamspeak3-server_linux_amd64/serverquerydocs/channeladdperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupaddclient.txt
teamspeak3-server_linux_amd64/serverquerydocs/servernotifyunregister.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientedit.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftrenamefile.txt
teamspeak3-server_linux_amd64/serverquerydocs/customdelete.txt
teamspeak3-server_linux_amd64/serverquerydocs/servertemppassworddel.txt
teamspeak3-server_linux_amd64/serverquerydocs/tokenadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/banadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientfind.txt
teamspeak3-server_linux_amd64/serverquerydocs/bandelall.txt
teamspeak3-server_linux_amd64/serverquerydocs/gm.txt
teamspeak3-server_linux_amd64/serverquerydocs/complainlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientsetserverquerylogin.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientpoke.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientgetids.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftinitupload.txt
teamspeak3-server_linux_amd64/serverquerydocs/permget.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupclientlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupsbyclientid.txt
teamspeak3-server_linux_amd64/serverquerydocs/customset.txt
teamspeak3-server_linux_amd64/serverquerydocs/messagedel.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftinitdownload.txt
teamspeak3-server_linux_amd64/serverquerydocs/privilegekeyadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgrouppermlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/permissionlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/version.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/whoami.txt
teamspeak3-server_linux_amd64/serverquerydocs/serverprocessstop.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelpermlist.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupautodelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/servercreate.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupadd.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftcreatedir.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelfind.txt
teamspeak3-server_linux_amd64/serverquerydocs/login.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergrouplist.txt
teamspeak3-server_linux_amd64/serverquerydocs/messagelist.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientdbinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgrouplist.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientaddservergroup.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelinfo.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgroupaddperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/channeledit.txt
teamspeak3-server_linux_amd64/serverquerydocs/channelgrouprename.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientupdate.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupdelperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/servergroupautoaddperm.txt
teamspeak3-server_linux_amd64/serverquerydocs/ftstop.txt
teamspeak3-server_linux_amd64/serverquerydocs/clientmove.txt
teamspeak3-server_linux_amd64/CHANGELOG
Removing intermediate container 0d9cf08c6d1a
---> d936a3322738
Step 7/8 : WORKDIR teamspeak3-server_linux_amd64
---> Running in 2940a5ad4043
Removing intermediate container 2940a5ad4043
---> d4ff4bfd528f
Step 8/8 : ENTRYPOINT ["sh", "ts3server_minimal_runscript.sh"]
---> Running in 160fd6399457
Removing intermediate container 160fd6399457
---> f3ed5b009f12
Successfully built f3ed5b009f12

il-j2% docker run -it --rm -p=9987:9987/udp -p=10011:10011 -p=30033:30033 f3ed5b009f12
2019-12-14 13:35:29.212340|INFO    |ServerLibPriv |   |TeamSpeak 3 Server 3.10.2 (2019-11-20 08:39:31)
2019-12-14 13:35:29.212792|INFO    |ServerLibPriv |   |SystemInformation: Linux 4.9.184-linuxkit #1 SMP Tue Jul 2 22:58:16 UTC 2019 x86_64 Binary: 64bit
2019-12-14 13:35:29.212884|WARNING |ServerLibPriv |   |The system locale is set to "C" this can cause unexpected behavior. We advice you to repair your locale!
2019-12-14 13:35:29.212932|INFO    |ServerLibPriv |   |Using hardware aes
2019-12-14 13:35:29.213686|INFO    |DatabaseQuery |   |dbPlugin name:    SQLite3 plugin, Version 3, (c)TeamSpeak Systems GmbH
2019-12-14 13:35:29.213834|INFO    |DatabaseQuery |   |dbPlugin version: 3.11.1
2019-12-14 13:35:29.214170|INFO    |DatabaseQuery |   |checking database integrity (may take a while)
2019-12-14 13:35:29.255379|INFO    |SQL           |   |db_CreateTables() tables created

------------------------------------------------------------------
I M P O R T A N T
------------------------------------------------------------------
Server Query Admin Account created
loginname= "serveradmin", password= "5V4LFF+6"
------------------------------------------------------------------

2019-12-14 13:35:29.299877|WARNING |Accounting    |   |Unable to open licensekey.dat, falling back to limited functionality
2019-12-14 13:35:29.300326|INFO    |Accounting    |   |Licensing Information
2019-12-14 13:35:29.300415|INFO    |Accounting    |   |licensed to       : Anonymous
2019-12-14 13:35:29.300448|INFO    |Accounting    |   |type              : No License
2019-12-14 13:35:29.300527|INFO    |Accounting    |   |starting date     : Mon Jul  1 00:00:00 2019
2019-12-14 13:35:29.300550|INFO    |Accounting    |   |ending date       : Wed Jul  1 00:00:00 2020
2019-12-14 13:35:29.300564|INFO    |Accounting    |   |max virtualservers: 1
2019-12-14 13:35:29.300595|INFO    |Accounting    |   |max slots         : 32
2019-12-14 13:35:29.396429|INFO    |              |   |myTeamSpeak identifier revocation list was downloaded successfully - all related features are activated
2019-12-14 13:35:29.949635|INFO    |              |   |Puzzle precompute time: 629
2019-12-14 13:35:29.951069|INFO    |FileManager   |   |listening on 0.0.0.0:30033, [::]:30033
2019-12-14 13:35:29.952717|INFO    |VirtualSvrMgr |   |executing monthly interval
2019-12-14 13:35:29.953478|INFO    |VirtualSvrMgr |   |reset virtualserver traffic statistics
2019-12-14 13:35:30.004916|INFO    |VirtualServerBase|1  |listening on 0.0.0.0:9987, [::]:9987
2019-12-14 13:35:30.006354|WARNING |VirtualServer |1  |--------------------------------------------------------
2019-12-14 13:35:30.006425|WARNING |VirtualServer |1  |ServerAdmin privilege key created, please use the line below
2019-12-14 13:35:30.007111|WARNING |VirtualServer |1  |token=ebZtF0POGueUMR0zZ8HGa1pPIBuK1RIZXcYL6LJo
2019-12-14 13:35:30.007130|WARNING |VirtualServer |1  |--------------------------------------------------------

------------------------------------------------------------------
I M P O R T A N T
------------------------------------------------------------------
ServerAdmin privilege key created, please use it to gain
serveradmin rights for your virtualserver. please
also check the doc/privilegekey_guide.txt for details.

token=ebZtF0POGueUMR0zZ8HGa1pPIBuK1RIZXcYL6LJo
------------------------------------------------------------------

2019-12-14 13:35:30.011545|INFO    |Query         |   |listening for query on 0.0.0.0:10011, [::]:10011
2019-12-14 13:35:30.012679|INFO    |Query         |   |listening for query ssh on 0.0.0.0:10022, [::]:10022
2019-12-14 13:35:30.013245|INFO    |Query         |   |creating QUERY_SSH_RSA_HOST_KEY file: ssh_host_rsa_key
2019-12-14 13:35:32.498904|INFO    |CIDRManager   |   |updated query_ip_whitelist ips: 127.0.0.1/32, ::1/128,
2019-12-14 13:37:49.325153|INFO    |VirtualServer |1  |client (id:3) was added to servergroup 'Server Admin'(id:6) by client 'server'(id:0)
2019-12-14 13:37:49.325821|INFO    |VirtualServer |1  |client 'TeamSpeakUser'(id:3) used privilege key 'ebZtF0POGueUMR0zZ8HGa1pPIBuK1RIZXcYL6LJo' and was added to servergroup 'Server Admin'(id:6)
^Z^C2019-12-14 13:38:10.253913|INFO    |ServerMain    |   |Received signal SIGINT, shutting down.
2019-12-14 13:38:10.254712|INFO    |VirtualServerBase|1  |stopped
il-j2% vim Dockerfile
il-j2% cat Dockerfile
FROM debian:10

MAINTAINER apearl@student.21-school.ru

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

EXPOSE 9987/udp 10011 30033

RUN apt-get update && apt-get upgrade -y && apt-get install -y wget bzip2 && \
wget https://files.teamspeak-services.com/releases/server/3.10.2/teamspeak3-server_linux_amd64-3.10.2.tar.bz2 && \
tar xvf *.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT ["sh", "ts3server_minimal_runscript.sh"]

+++++++++++++++++
HOW TO CHECK
--------------------------
il-j2% ifconfig
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
inet 127.0.0.1 netmask 0xff000000
inet6 ::1 prefixlen 128
inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
nd6 options=201<PERFORMNUD,DAD>
gif0: flags=8010<POINTOPOINT,MULTICAST> mtu 1280
stf0: flags=0<> mtu 1280
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
options=10b<RXCSUM,TXCSUM,VLAN_HWTAGGING,AV>
ether 78:7b:8a:c9:46:a6
inet6 fe80::148a:d50b:8245:d5a4%en0 prefixlen 64 secured scopeid 0x4
inet 192.168.20.59 netmask 0xfffffc00 broadcast 192.168.23.255
nd6 options=201<PERFORMNUD,DAD>
media: autoselect (1000baseT <full-duplex,energy-efficient-ethernet>)
status: active
en1: flags=8822<BROADCAST,SMART,SIMPLEX,MULTICAST> mtu 1500
ether 24:f6:77:0b:33:f0
media: autoselect (<unknown type>)
status: inactive
en2: flags=963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX> mtu 1500
options=60<TSO4,TSO6>
ether 8e:00:ac:68:ef:00
media: autoselect <full-duplex>
status: inactive
en3: flags=963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX> mtu 1500
options=60<TSO4,TSO6>
ether 8e:00:ac:68:ef:01
media: autoselect <full-duplex>
status: inactive
bridge0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
options=63<RXCSUM,TXCSUM,TSO4,TSO6>
ether 8e:00:ac:68:ef:00
Configuration:
id 0:0:0:0:0:0 priority 0 hellotime 0 fwddelay 0
maxage 0 holdcnt 0 proto stp maxaddr 100 timeout 1200
root id 0:0:0:0:0:0 priority 0 ifcost 0 port 0
ipfilter disabled flags 0x2
member: en2 flags=3<LEARNING,DISCOVER>
ifmaxaddr 0 port 6 priority 0 path cost 0
member: en3 flags=3<LEARNING,DISCOVER>
ifmaxaddr 0 port 7 priority 0 path cost 0
nd6 options=201<PERFORMNUD,DAD>
media: <unknown type>
status: inactive
p2p0: flags=8802<BROADCAST,SIMPLEX,MULTICAST> mtu 2304
ether 06:f6:77:0b:33:f0
media: autoselect
status: inactive
awdl0: flags=8902<BROADCAST,PROMISC,SIMPLEX,MULTICAST> mtu 1484
ether e6:5c:8d:46:44:78
nd6 options=201<PERFORMNUD,DAD>
media: autoselect
status: inactive
utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
inet6 fe80::d277:f124:6c6d:2a2%utun0 prefixlen 64 scopeid 0xb
nd6 options=201<PERFORMNUD,DAD>
vboxnet0: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:00
vboxnet1: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:01
vboxnet2: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:02
vboxnet3: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:03
vboxnet4: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:04
vboxnet5: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:05
vboxnet6: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
ether 0a:00:27:00:00:06
inet 192.168.99.1 netmask 0xffffff00 broadcast 192.168.99.255

RUNNING MUST BE DONE WHILE RUNNING THE TEAMSPEAK IN TERMINAL
RUN APP FROM https://teamspeak.com/en/thanks-for-downloading-teamspeak/
COMMAND + S
SERVER NICKNAME OR ADDRESS: 192.168.20.59
SERVER PASSWORD: 5V4LFF+6

ebZtF0POGueUMR0zZ8HGa1pPIBuK1RIZXcYL6LJo


***************************************

===============================================

VI.3 Exercise 02: Dockerfile in a Dockerfile... in a Dockerfile ?
You are going to create your first Dockerfile to containerize Rails applications. That’s a special configuration: this particular Dockerfile will be generic, and called in another Dockerfile, that will look something like this:
Your generic container should install, via a ruby container, all the necessary depen- dencies and gems, then copy your rails application in the /opt/app folder of your container. Docker has to install the approtiate gems when it builds, but also launch the migrations and the db population for your application. The child Dockerfile should launch the rails server (see example below). If you don’t know what commands to use, it’s high time to look at the Ruby on Rails documentation.
===========================
https://guides.rubyonrails.org/getting_started.html
Пакет: libpq-dev (12.1-1 и другие)
https://packages.debian.org/sid/libpq-dev
https://www.sqlitetutorial.net/download-install-sqlite/
https://www.sqlite.org/index.html
https://docs.docker.com/compose/rails/

Yes, links are not available during the build phase. There are a few ways to get your fixtures installed during build, but none of them are really straightforward:

you can create a new Dockerfile for the database, add all the project source, and ruby dependencies, and run the rake db:create as part of the image build
you can run the rake db:create as part of the entrypoint script of a database image (something I'm not a fan of, and don't really recommend)
you can exclude it from the build, and make it a manual step in setting up the environment (also not a fan, but it's the easiest option, and what we use in the example docs http://docs.docker.com/compose/rails/)
make a multi-step build that starts a temporary environment, which you can use to create a sql dump of the data, and include that in the database image. There isn't great tooling for that yet, but I'm working on something in https://github.com/dnephin/buildpipe. There are no docs for it yet, so you probably don't want to use it.


============
Answer:


il-j2% ls
Dockerfile    app        build.sh    ft_rails
il-j2% cd ft_rails
il-j2% ls
Dockerfile
il-j2% docker build -t ft-rails:on-build .
Sending build context to Docker daemon  15.36kB
Step 1/8 : FROM ruby
---> 2ff4e698f315
Step 2/8 : RUN apt-get update -y && apt-get upgrade -y &&     apt-get install wget nodejs sqlite3 -y --no-install-recommends
---> Running in ec4427f49deb
Get:1 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:2 http://deb.debian.org/debian buster InRelease [122 kB]
Get:3 http://deb.debian.org/debian buster-updates InRelease [49.3 kB]
Get:4 http://security.debian.org/debian-security buster/updates/main amd64 Packages [162 kB]
Get:5 http://deb.debian.org/debian buster/main amd64 Packages [7908 kB]
Get:6 http://deb.debian.org/debian buster-updates/main amd64 Packages [5792 B]
Fetched 8312 kB in 3s (2725 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
Calculating upgrade...
The following packages will be upgraded:
git git-man
2 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 7239 kB of archives.
After this operation, 18.4 kB of additional disk space will be used.
Get:1 http://security.debian.org/debian-security buster/updates/main amd64 git-man all 1:2.20.1-2+deb10u1 [1620 kB]
Get:2 http://security.debian.org/debian-security buster/updates/main amd64 git amd64 1:2.20.1-2+deb10u1 [5620 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 7239 kB in 1s (5083 kB/s)
(Reading database ... 23960 files and directories currently installed.)
Preparing to unpack .../git-man_1%3a2.20.1-2+deb10u1_all.deb ...
Unpacking git-man (1:2.20.1-2+deb10u1) over (1:2.20.1-2) ...
Preparing to unpack .../git_1%3a2.20.1-2+deb10u1_amd64.deb ...
Unpacking git (1:2.20.1-2+deb10u1) over (1:2.20.1-2) ...
Setting up git-man (1:2.20.1-2+deb10u1) ...
Setting up git (1:2.20.1-2+deb10u1) ...
Reading package lists...
Building dependency tree...
Reading state information...
wget is already the newest version (1.20.1-1.1).
The following additional packages will be installed:
libc-ares2 libnode64 libuv1
Suggested packages:
npm sqlite3-doc
Recommended packages:
nodejs-doc
The following NEW packages will be installed:
libc-ares2 libnode64 libuv1 nodejs sqlite3
0 upgraded, 5 newly installed, 0 to remove and 0 not upgraded.
Need to get 6742 kB of archives.
After this operation, 25.6 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian buster/main amd64 libc-ares2 amd64 1.14.0-1 [85.8 kB]
Get:2 http://deb.debian.org/debian buster/main amd64 libuv1 amd64 1.24.1-1 [110 kB]
Get:3 http://deb.debian.org/debian buster/main amd64 libnode64 amd64 10.15.2~dfsg-2 [5521 kB]
Get:4 http://deb.debian.org/debian buster/main amd64 nodejs amd64 10.15.2~dfsg-2 [86.2 kB]
Get:5 http://deb.debian.org/debian buster/main amd64 sqlite3 amd64 3.27.2-3 [939 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 6742 kB in 1s (9782 kB/s)
Selecting previously unselected package libc-ares2:amd64.
(Reading database ... 23967 files and directories currently installed.)
Preparing to unpack .../libc-ares2_1.14.0-1_amd64.deb ...
Unpacking libc-ares2:amd64 (1.14.0-1) ...
Selecting previously unselected package libuv1:amd64.
Preparing to unpack .../libuv1_1.24.1-1_amd64.deb ...
Unpacking libuv1:amd64 (1.24.1-1) ...
Selecting previously unselected package libnode64:amd64.
Preparing to unpack .../libnode64_10.15.2~dfsg-2_amd64.deb ...
Unpacking libnode64:amd64 (10.15.2~dfsg-2) ...
Selecting previously unselected package nodejs.
Preparing to unpack .../nodejs_10.15.2~dfsg-2_amd64.deb ...
Unpacking nodejs (10.15.2~dfsg-2) ...
Selecting previously unselected package sqlite3.
Preparing to unpack .../sqlite3_3.27.2-3_amd64.deb ...
Unpacking sqlite3 (3.27.2-3) ...
Setting up libc-ares2:amd64 (1.14.0-1) ...
Setting up libuv1:amd64 (1.24.1-1) ...
Setting up libnode64:amd64 (10.15.2~dfsg-2) ...
Setting up sqlite3 (3.27.2-3) ...
Setting up nodejs (10.15.2~dfsg-2) ...
update-alternatives: using /usr/bin/nodejs to provide /usr/bin/js (js) in auto mode
Processing triggers for libc-bin (2.28-10) ...
Removing intermediate container ec4427f49deb
---> c3b898239d1b
Step 3/8 : ONBUILD COPY app /opt/app
---> Running in 78c09de7d6b0
Removing intermediate container 78c09de7d6b0
---> 0f0769d7d7e4
Step 4/8 : ONBUILD WORKDIR /opt/app
---> Running in 037e7c39fa5c
Removing intermediate container 037e7c39fa5c
---> 7ba621fb31c1
Step 5/8 : ONBUILD EXPOSE 3000
---> Running in a8216480254e
Removing intermediate container a8216480254e
---> 348518d383c5
Step 6/8 : ONBUILD RUN bundle install
---> Running in 6335c4ad7f5f
Removing intermediate container 6335c4ad7f5f
---> 55b3d984ec1b
Step 7/8 : ONBUILD RUN bundle exec rake db:migrate
---> Running in ce9c1fb17391
Removing intermediate container ce9c1fb17391
---> 998ce23d5c77
Step 8/8 : ONBUILD RUN bundle exec rake db:seed
---> Running in d56616717ee8
Removing intermediate container d56616717ee8
---> f21f91fd482e
Successfully built f21f91fd482e
Successfully tagged ft-rails:on-build
il-j2% cd ..
il-j2% ls
Dockerfile    app        build.sh    ft_rails
il-j2% docker build -t .
invalid argument "." for "-t, --tag" flag: invalid reference format
See 'docker build --help'.
il-j2% docker build .
Sending build context to Docker daemon  474.6kB
Step 1/3 : FROM     ft-rails:on-build
# Executing 6 build triggers
---> Running in ecbf0a158b0d
Removing intermediate container ecbf0a158b0d
---> Running in 9b1129bb2f14
Removing intermediate container 9b1129bb2f14
---> Running in e2e3740c69ab
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
Fetching gem metadata from https://rubygems.org/........
Fetching rake 12.3.1
Installing rake 12.3.1
Fetching CFPropertyList 2.3.6
Installing CFPropertyList 2.3.6
Fetching concurrent-ruby 1.0.5
Installing concurrent-ruby 1.0.5
Fetching i18n 0.9.5
Installing i18n 0.9.5
Fetching minitest 5.10.3
Installing minitest 5.10.3
Fetching thread_safe 0.3.6
Installing thread_safe 0.3.6
Fetching tzinfo 1.2.5
Installing tzinfo 1.2.5
Fetching activesupport 5.1.6
Installing activesupport 5.1.6
Fetching builder 3.2.3
Installing builder 3.2.3
Fetching erubi 1.7.1
Installing erubi 1.7.1
Fetching mini_portile2 2.3.0
Installing mini_portile2 2.3.0
Fetching nokogiri 1.8.4
Installing nokogiri 1.8.4 with native extensions
Fetching rails-dom-testing 2.0.3
Installing rails-dom-testing 2.0.3
Fetching crass 1.0.4
Installing crass 1.0.4
Fetching loofah 2.2.2
Installing loofah 2.2.2
Fetching rails-html-sanitizer 1.0.4
Installing rails-html-sanitizer 1.0.4
Fetching actionview 5.1.6
Installing actionview 5.1.6
Fetching rack 2.0.5
Installing rack 2.0.5
Fetching rack-test 1.1.0
Installing rack-test 1.1.0
Fetching actionpack 5.1.6
Installing actionpack 5.1.6
Fetching nio4r 2.3.1
Installing nio4r 2.3.1 with native extensions
Fetching websocket-extensions 0.1.3
Installing websocket-extensions 0.1.3
Fetching websocket-driver 0.6.5
Installing websocket-driver 0.6.5 with native extensions
Fetching actioncable 5.1.6
Installing actioncable 5.1.6
Fetching globalid 0.4.1
Installing globalid 0.4.1
Fetching activejob 5.1.6
Installing activejob 5.1.6
Fetching mini_mime 1.0.0
Installing mini_mime 1.0.0
Fetching mail 2.7.0
Installing mail 2.7.0
Fetching actionmailer 5.1.6
Installing actionmailer 5.1.6
Fetching activemodel 5.1.6
Installing activemodel 5.1.6
Fetching arel 8.0.0
Installing arel 8.0.0
Fetching activerecord 5.1.6
Installing activerecord 5.1.6
Fetching ansi 1.5.0
Installing ansi 1.5.0
Fetching execjs 2.7.0
Installing execjs 2.7.0
Fetching autoprefixer-rails 9.0.2
Installing autoprefixer-rails 9.0.2
Fetching bcrypt 3.1.12
Installing bcrypt 3.1.12 with native extensions
Fetching bindex 0.5.0
Installing bindex 0.5.0 with native extensions
Fetching rb-fsevent 0.10.3
Installing rb-fsevent 0.10.3
Fetching ffi 1.9.25
Installing ffi 1.9.25 with native extensions
Fetching rb-inotify 0.9.10
Installing rb-inotify 0.9.10
Fetching sass-listen 4.0.0
Installing sass-listen 4.0.0
Fetching sass 3.5.7
Installing sass 3.5.7
Fetching bootstrap-sass 3.3.7
Installing bootstrap-sass 3.3.7
Fetching will_paginate 3.1.6
Installing will_paginate 3.1.6
Fetching bootstrap-will_paginate 1.0.0
Installing bootstrap-will_paginate 1.0.0
Using bundler 1.17.2
Fetching byebug 9.0.6
Installing byebug 9.0.6 with native extensions
Fetching mime-types-data 3.2016.0521
Installing mime-types-data 3.2016.0521
Fetching mime-types 3.1
Installing mime-types 3.1
Fetching carrierwave 1.2.2
Installing carrierwave 1.2.2
Fetching coderay 1.1.2
Installing coderay 1.1.2
Fetching coffee-script-source 1.12.2
Installing coffee-script-source 1.12.2
Fetching coffee-script 2.4.1
Installing coffee-script 2.4.1
Fetching method_source 0.9.0
Installing method_source 0.9.0
Fetching thor 0.20.0
Installing thor 0.20.0
Fetching railties 5.1.6
Installing railties 5.1.6
Fetching coffee-rails 4.2.2
Installing coffee-rails 4.2.2
Fetching unf_ext 0.0.7.5
Installing unf_ext 0.0.7.5 with native extensions
Fetching unf 0.1.4
Installing unf 0.1.4
Fetching domain_name 0.5.20180417
Installing domain_name 0.5.20180417
Fetching excon 0.62.0
Installing excon 0.62.0
Fetching faker 1.7.3
Installing faker 1.7.3
Fetching fission 0.5.0
Installing fission 0.5.0
Fetching formatador 0.2.5
Installing formatador 0.2.5
Fetching fog-core 1.45.0
Installing fog-core 1.45.0
Fetching multi_json 1.13.1
Installing multi_json 1.13.1
Fetching fog-json 1.2.0
Installing fog-json 1.2.0
Fetching ipaddress 0.8.3
Installing ipaddress 0.8.3
Fetching xml-simple 1.1.5
Installing xml-simple 1.1.5
Fetching fog-aliyun 0.3.2
Installing fog-aliyun 0.3.2
Fetching fog-xml 0.1.3
Installing fog-xml 0.1.3
Fetching fog-atmos 0.1.0
Installing fog-atmos 0.1.0
Fetching fog-aws 2.0.1
Installing fog-aws 2.0.1
Fetching inflecto 0.0.2
Installing inflecto 0.0.2
Fetching fog-brightbox 0.15.0
Installing fog-brightbox 0.15.0
Fetching fog-cloudatcost 0.1.2
Installing fog-cloudatcost 0.1.2
Fetching fog-digitalocean 0.3.0
Installing fog-digitalocean 0.3.0
Fetching fog-dnsimple 1.0.0
Installing fog-dnsimple 1.0.0
Fetching fog-dynect 0.0.3
Installing fog-dynect 0.0.3
Fetching fog-ecloud 0.3.0
Installing fog-ecloud 0.3.0
Fetching fog-google 0.1.0
Installing fog-google 0.1.0
Fetching fog-internet-archive 0.0.1
Installing fog-internet-archive 0.0.1
Fetching fog-joyent 0.0.1
Installing fog-joyent 0.0.1
Fetching fog-local 0.5.0
Installing fog-local 0.5.0
Fetching fog-openstack 0.1.27
Installing fog-openstack 0.1.27
Using json 2.1.0
Fetching ovirt-engine-sdk 4.2.4
Installing ovirt-engine-sdk 4.2.4 with native extensions
Fetching http-cookie 1.0.3
Installing http-cookie 1.0.3
Fetching netrc 0.11.0
Installing netrc 0.11.0
Fetching rest-client 2.0.2
Installing rest-client 2.0.2
Fetching rbovirt 0.1.7
Installing rbovirt 0.1.7
Fetching fog-ovirt 1.1.1
Installing fog-ovirt 1.1.1
Fetching fog-powerdns 0.2.0
Installing fog-powerdns 0.2.0
Fetching fog-profitbricks 4.1.1
Installing fog-profitbricks 4.1.1
Fetching fog-rackspace 0.1.5
Installing fog-rackspace 0.1.5
Fetching fog-radosgw 0.0.5
Installing fog-radosgw 0.0.5
Fetching fog-riakcs 0.1.0
Installing fog-riakcs 0.1.0
Fetching fog-sakuracloud 1.7.5
Installing fog-sakuracloud 1.7.5
Fetching fog-serverlove 0.1.2
Installing fog-serverlove 0.1.2
Fetching fog-softlayer 1.1.4
Installing fog-softlayer 1.1.4
Fetching fog-storm_on_demand 0.1.1
Installing fog-storm_on_demand 0.1.1
Fetching fog-terremark 0.1.0
Installing fog-terremark 0.1.0
Fetching fog-vmfusion 0.1.0
Installing fog-vmfusion 0.1.0
Fetching fog-voxel 0.1.0
Installing fog-voxel 0.1.0
Fetching trollop 2.1.3
Installing trollop 2.1.3
Fetching rbvmomi 1.13.0
Installing rbvmomi 1.13.0
Fetching fog-vsphere 2.3.0
Installing fog-vsphere 2.3.0
Fetching fog-xenserver 0.3.0
Installing fog-xenserver 0.3.0
Fetching fog 1.42.0
Installing fog 1.42.0
Fetching ruby_dep 1.5.0
Installing ruby_dep 1.5.0
Fetching listen 3.1.5
Installing listen 3.1.5
Fetching lumberjack 1.0.13
Installing lumberjack 1.0.13
Fetching nenv 0.3.0
Installing nenv 0.3.0
Fetching shellany 0.0.1
Installing shellany 0.0.1
Fetching notiffany 0.1.1
Installing notiffany 0.1.1
Fetching pry 0.11.3
Installing pry 0.11.3
Fetching guard 2.14.1
Installing guard 2.14.1
Fetching guard-compat 1.2.1
Installing guard-compat 1.2.1
Fetching guard-minitest 2.4.6
Installing guard-minitest 2.4.6
Fetching jbuilder 2.7.0
Installing jbuilder 2.7.0
Fetching jquery-rails 4.3.1
Installing jquery-rails 4.3.1
Fetching mini_magick 4.7.0
Installing mini_magick 4.7.0
Fetching ruby-progressbar 1.9.0
Installing ruby-progressbar 1.9.0
Fetching minitest-reporters 1.1.14
Installing minitest-reporters 1.1.14
Fetching pg 0.20.0
Installing pg 0.20.0 with native extensions
Fetching puma 3.9.1
Installing puma 3.9.1 with native extensions
Fetching sprockets 3.7.2
Installing sprockets 3.7.2
Fetching sprockets-rails 3.2.1
Installing sprockets-rails 3.2.1
Fetching rails 5.1.6
Installing rails 5.1.6
Fetching rails-controller-testing 1.0.2
Installing rails-controller-testing 1.0.2
Fetching tilt 2.0.8
Installing tilt 2.0.8
Fetching sass-rails 5.0.6
Installing sass-rails 5.0.6
Fetching spring 2.0.2
Installing spring 2.0.2
Fetching spring-watcher-listen 2.0.1
Installing spring-watcher-listen 2.0.1
Fetching sqlite3 1.3.13
Installing sqlite3 1.3.13 with native extensions
Fetching turbolinks-source 5.1.0
Installing turbolinks-source 5.1.0
Fetching turbolinks 5.0.1
Installing turbolinks 5.0.1
Fetching uglifier 3.2.0
Installing uglifier 3.2.0
Fetching web-console 3.5.1
Installing web-console 3.5.1
Bundle complete! 29 Gemfile dependencies, 139 gems now installed.
Bundled gems are installed into `/usr/local/bundle`
Post-install message from sass:

Ruby Sass is deprecated and will be unmaintained as of 26 March 2019.

* If you use Sass as a command-line tool, we recommend using Dart Sass, the new
primary implementation: https://sass-lang.com/install

* If you use Sass as a plug-in for a Ruby web framework, we recommend using the
sassc gem: https://github.com/sass/sassc-ruby#readme

* For more details, please refer to the Sass blog:
http://sass.logdown.com/posts/7081811

Post-install message from fog:
------------------------------
Thank you for installing fog!

IMPORTANT NOTICE:
If there's a metagem available for your cloud provider, e.g. `fog-aws`,
you should be using it instead of requiring the full fog collection to avoid
unnecessary dependencies.

'fog' should be required explicitly only if:
- The provider you use doesn't yet have a metagem available.
- You require Ruby 1.9.3 support.
------------------------------
Removing intermediate container e2e3740c69ab
---> Running in 70ccfadaf9bd
== 20160523185459 CreateUsers: migrating ======================================
-- adapter_name()
-> 0.0000s
-- adapter_name()
-> 0.0000s
-- create_table(:users, {:id=>:integer})
-> 0.0011s
== 20160523185459 CreateUsers: migrated (0.0012s) =============================

== 20160523202806 AddIndexToUsersEmail: migrating =============================
-- add_index(:users, :email, {:unique=>true})
-> 0.0011s
== 20160523202806 AddIndexToUsersEmail: migrated (0.0013s) ====================

== 20160523203059 AddPasswordDigestToUsers: migrating =========================
-- add_column(:users, :password_digest, :string)
-> 0.0009s
== 20160523203059 AddPasswordDigestToUsers: migrated (0.0010s) ================

== 20160602174637 AddRememberDigestToUsers: migrating =========================
-- add_column(:users, :remember_digest, :string)
-> 0.0012s
== 20160602174637 AddRememberDigestToUsers: migrated (0.0014s) ================

== 20160605021434 AddAdminToUsers: migrating ==================================
-- add_column(:users, :admin, :boolean, {:default=>false})
-> 0.0012s
== 20160605021434 AddAdminToUsers: migrated (0.0014s) =========================

== 20160606194223 AddActivationToUsers: migrating =============================
-- add_column(:users, :activation_digest, :string)
-> 0.0010s
-- add_column(:users, :activated, :boolean, {:default=>false})
-> 0.0005s
-- add_column(:users, :activated_at, :datetime)
-> 0.0003s
== 20160606194223 AddActivationToUsers: migrated (0.0019s) ====================

== 20160606233616 AddResetToUsers: migrating ==================================
-- add_column(:users, :reset_digest, :string)
-> 0.0005s
-- add_column(:users, :reset_sent_at, :datetime)
-> 0.0009s
== 20160606233616 AddResetToUsers: migrated (0.0015s) =========================

== 20160608164853 CreateMicroposts: migrating =================================
-- adapter_name()
-> 0.0000s
-- adapter_name()
-> 0.0000s
-- create_table(:microposts, {:id=>:integer})
-> 0.0020s
== 20160608164853 CreateMicroposts: migrated (0.0022s) ========================

== 20160608202205 AddPictureToMicroposts: migrating ===========================
-- add_column(:microposts, :picture, :string)
-> 0.0006s
== 20160608202205 AddPictureToMicroposts: migrated (0.0011s) ==================

== 20160609220802 CreateRelationships: migrating ==============================
-- adapter_name()
-> 0.0000s
-- adapter_name()
-> 0.0000s
-- create_table(:relationships, {:id=>:integer})
-> 0.0010s
-- add_index(:relationships, :follower_id)
-> 0.0006s
-- add_index(:relationships, :followed_id)
-> 0.0008s
-- add_index(:relationships, [:follower_id, :followed_id], {:unique=>true})
-> 0.0010s
== 20160609220802 CreateRelationships: migrated (0.0041s) =====================

Removing intermediate container 70ccfadaf9bd
---> Running in 4adabe4b18ca
Removing intermediate container 4adabe4b18ca
---> ba61f0225b7b
Step 2/3 : EXPOSE  3000
---> Running in 907e6a7804b0
Removing intermediate container 907e6a7804b0
---> ca3a4c2a6746
Step 3/3 : CMD        ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
---> Running in 54e4113b8876
Removing intermediate container 54e4113b8876
---> 64f04cc7c40a
Successfully built 64f04cc7c40a
il-j2% ls
Dockerfile    app        ft_rails
il-j2% docker run --rm -it -p 3000:3000 64f04cc7c40a
=> Booting Puma
=> Rails 5.1.6 application starting in development
=> Run `rails server -h` for more startup options
[1] Puma starting in cluster mode...
[1] * Version 3.9.1 (ruby 2.6.5-p114), codename: Private Caller
[1] * Min threads: 5, max threads: 5
[1] * Environment: development
[1] * Process workers: 2
[1] * Preloading application
[1] * Listening on tcp://0.0.0.0:3000
[1] Use Ctrl-C to stop
[1] - Worker 1 (pid: 13) booted, phase: 0
[1] - Worker 0 (pid: 11) booted, phase: 0
Started GET "/" for 172.17.0.1 at 2019-12-14 16:36:23 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
(0.1ms)  SELECT "schema_migrations"."version" FROM "schema_migrations" ORDER BY "schema_migrations"."version" ASC
Processing by StaticPagesController#home as HTML
Rendering static_pages/home.html.erb within layouts/application
Rendered static_pages/home.html.erb within layouts/application (5242.7ms)
Rendered layouts/_shim.html.erb (0.2ms)
Rendered layouts/_header.html.erb (1.5ms)
Rendered layouts/_footer.html.erb (0.9ms)
Completed 200 OK in 5382ms (Views: 5373.9ms | ActiveRecord: 0.0ms)


Started GET "/assets/account_activations.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:29 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/custom.self-47d41c9df4afde34eca50f90012ed7669ca39c644c365a86f658f004e7907c05.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/password_resets.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Started GET "/assets/microposts.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/relationships.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/users.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/application.self-af04b226fd7202dfc532ce7aedb95a0128277937e90d3b3a3d35e1cce9e16886.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/static_pages.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/jquery.self-bd7ddd393353a8d2480a622e80342adf488fb6006d667e8b42e4c0073393abee.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/rails-ujs.self-8944eaf3f9a2615ce7c830a810ed630e296633063af8bb7441d5702fbe3ea597.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Started GET "/assets/bootstrap.self-b38817c3e3a3049abb1fc08dd6ae448b23330f8453226efdb074710209474f75.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/turbolinks.self-2db6ec539b9190f75e1d477b305df53d12904d5cafdd47c7ffd91ba25cbec128.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/account_activations.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/action_cable.self-69fddfcddf4fdef9828648f9330d6ce108b93b82b0b8d3affffc59a114853451.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/cable.self-6e0514260c1aa76eaf252412ce74e63f68819fd19bf740595f592c5ba4c36537.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/password_resets.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/relationships.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/microposts.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/users.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/static_pages.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Started GET "/assets/application.self-7c370d9536d7d0d6a0f7cd7f9826692acd93e4fb05ba46f7b630b879740343d3.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/sessions.self-877aef30ae1b040ab8a3aba4e3e309a11d7f2612f44dde450b5c157aa5f95c05.js?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/rails-c094bc3a4bf50e5bb477109e5cb0d213af27ad75b481c4df249f50974dbeefe6.png" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/assets/sessions.self-e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855.css?body=1" for 172.17.0.1 at 2019-12-14 16:36:30 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Started GET "/" for 172.17.0.1 at 2019-12-14 16:37:06 +0000
Cannot render console from 172.17.0.1! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255
Processing by StaticPagesController#home as HTML
Rendering static_pages/home.html.erb within layouts/application
Rendered static_pages/home.html.erb within layouts/application (8.8ms)
Rendered layouts/_shim.html.erb (0.2ms)
Rendered layouts/_header.html.erb (0.7ms)
Rendered layouts/_footer.html.erb (0.4ms)
Completed 200 OK in 41ms (Views: 40.1ms | ActiveRecord: 0.0ms)


http://localhost:3000/
http://192.168.99.109:3000/

^C[1] - Gracefully shutting down workers...
[1] === puma shutdown: 2019-12-14 16:41:08 +0000 ===
[1] - Goodbye!
Exiting

il-j2% cat Dockerfile
FROM     ft-rails:on-build

EXPOSE  3000
CMD        ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
il-j2% ls
Dockerfile    app        ft_rails
il-j2% cd ft_rails
il-j2% cat Dockerfile
FROM ruby

RUN apt-get update -y && apt-get upgrade -y && \
apt-get install wget nodejs sqlite3 -y --no-install-recommends

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN bundle exec rake db:migrate
ONBUILD RUN bundle exec rake db:seed
il-j2%

docker build -t ft-rails:on-build . /ft-rails && \
docker build -t ex02 . && \
docker run --rm -it -p 3000:3000 ex02

http://192.168.99.109:3000/


=========
VI.4 Exercise 03: ... and bacon strips ... and bacon strips ...
Docker can be useful to test an application that’s still being developed without pol- luting your libraries. You will have to design a Dockerfile that gets the development version of Gitlab - Community Edition installs it with all the dependencies and the nec- essary configurations, and launches the application, all as it builds. The container will be deemed valid if you can access the web client, create users and interact via GIT with this container (HTTPS and SSH). Obviously, you are not allowed to use the o cial container from Gitlab, it would be a shame...


apearl@student.21-school.ru

--
docker build -t ex03 .
docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
--

--
* ruby_block[Delete unmanaged env files for alertmanager service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/alertmanager/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/alertmanager/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/alertmanager/control] action create (up to date)
* link[/opt/gitlab/init/alertmanager] action create (up to date)
* file[/opt/gitlab/sv/alertmanager/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/alertmanager] action create
- create symlink at /opt/gitlab/service/alertmanager to /opt/gitlab/sv/alertmanager
* ruby_block[wait for alertmanager service socket] action run
- execute the ruby block wait for alertmanager service socket
- execute the ruby block restart_log_service
* ruby_block[reload_log_service] action create
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/alertmanager] action create (up to date)
* template[/opt/gitlab/sv/alertmanager/run] action create (up to date)
* directory[/opt/gitlab/sv/alertmanager/log] action create (up to date)
* directory[/opt/gitlab/sv/alertmanager/log/main] action create (up to date)
* template[/opt/gitlab/sv/alertmanager/log/run] action create (up to date)
* template[/var/log/gitlab/alertmanager/config] action create (up to date)
* ruby_block[verify_chown_persisted_on_alertmanager] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/alertmanager/env] action create (up to date)
* ruby_block[Delete unmanaged env files for alertmanager service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/alertmanager/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/alertmanager/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/alertmanager/control] action create (up to date)
* link[/opt/gitlab/init/alertmanager] action create (up to date)
* file[/opt/gitlab/sv/alertmanager/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/alertmanager] action create (up to date)
* ruby_block[wait for alertmanager service socket] action run (skipped due to not_if)
- execute the ruby block reload_log_service
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/alertmanager] action create (up to date)
* ruby_block[wait for alertmanager service socket] action run (skipped due to not_if)

* execute[/opt/gitlab/bin/gitlab-ctl start alertmanager] action run
[execute] ok: run: alertmanager: (pid 1040) 1s
- execute /opt/gitlab/bin/gitlab-ctl start alertmanager
Recipe: monitoring::postgres-exporter
* directory[/var/log/gitlab/postgres-exporter] action create
- create new directory /var/log/gitlab/postgres-exporter
- change mode from '' to '0700'
- change owner from '' to 'gitlab-psql'
* directory[/var/opt/gitlab/postgres-exporter] action create
- create new directory /var/opt/gitlab/postgres-exporter
- change mode from '' to '0700'
- change owner from '' to 'gitlab-psql'
* env_dir[/opt/gitlab/etc/postgres-exporter/env] action create
* directory[/opt/gitlab/etc/postgres-exporter/env] action create
- create new directory /opt/gitlab/etc/postgres-exporter/env
* file[/opt/gitlab/etc/postgres-exporter/env/SSL_CERT_DIR] action create
- create new file /opt/gitlab/etc/postgres-exporter/env/SSL_CERT_DIR
- update content in file /opt/gitlab/etc/postgres-exporter/env/SSL_CERT_DIR from none to 4f45cf
--- /opt/gitlab/etc/postgres-exporter/env/SSL_CERT_DIR    2019-12-14 17:26:52.666569000 +0000
+++ /opt/gitlab/etc/postgres-exporter/env/.chef-SSL_CERT_DIR20191214-12-959kjh    2019-12-14 17:26:52.666569000 +0000
@@ -1 +1,2 @@
+/opt/gitlab/embedded/ssl/certs/
* file[/opt/gitlab/etc/postgres-exporter/env/DATA_SOURCE_NAME] action create
- create new file /opt/gitlab/etc/postgres-exporter/env/DATA_SOURCE_NAME
- update content in file /opt/gitlab/etc/postgres-exporter/env/DATA_SOURCE_NAME from none to cd58e5
--- /opt/gitlab/etc/postgres-exporter/env/DATA_SOURCE_NAME    2019-12-14 17:26:52.676569000 +0000
+++ /opt/gitlab/etc/postgres-exporter/env/.chef-DATA_SOURCE_NAME20191214-12-10tsovw    2019-12-14 17:26:52.676569000 +0000
@@ -1 +1,2 @@
+user=gitlab-psql host=/var/opt/gitlab/postgresql database=postgres sslmode=allow

Recipe: <Dynamically Defined Resource>
* service[postgres-exporter] action nothing (skipped due to action :nothing)
Recipe: monitoring::postgres-exporter
* runit_service[postgres-exporter] action enable
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter] action create
- create new directory /opt/gitlab/sv/postgres-exporter
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/opt/gitlab/sv/postgres-exporter/run] action create
- create new file /opt/gitlab/sv/postgres-exporter/run
- update content in file /opt/gitlab/sv/postgres-exporter/run from none to b40d34
--- /opt/gitlab/sv/postgres-exporter/run    2019-12-14 17:26:52.716569000 +0000
+++ /opt/gitlab/sv/postgres-exporter/.chef-run20191214-12-1pz5gxy    2019-12-14 17:26:52.716569000 +0000
@@ -1 +1,7 @@
+#!/bin/sh
+exec 2>&1
+
+umask 077
+exec chpst -e /opt/gitlab/etc/postgres-exporter/env -P -U gitlab-psql:git -u gitlab-psql:git /opt/gitlab/embedded/bin/postgres_exporter --web.listen-address=localhost:9187 --extend.query-path=/var/opt/gitlab/postgres-exporter/queries.yaml
+
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* directory[/opt/gitlab/sv/postgres-exporter/log] action create
- create new directory /opt/gitlab/sv/postgres-exporter/log
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* directory[/opt/gitlab/sv/postgres-exporter/log/main] action create
- create new directory /opt/gitlab/sv/postgres-exporter/log/main
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/opt/gitlab/sv/postgres-exporter/log/run] action create
- create new file /opt/gitlab/sv/postgres-exporter/log/run
- update content in file /opt/gitlab/sv/postgres-exporter/log/run from none to b971c9
--- /opt/gitlab/sv/postgres-exporter/log/run    2019-12-14 17:26:52.726569000 +0000
+++ /opt/gitlab/sv/postgres-exporter/log/.chef-run20191214-12-18q82v8    2019-12-14 17:26:52.726569000 +0000
@@ -1 +1,3 @@
+#!/bin/sh
+exec svlogd -tt /var/log/gitlab/postgres-exporter
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/var/log/gitlab/postgres-exporter/config] action create
- create new file /var/log/gitlab/postgres-exporter/config
- update content in file /var/log/gitlab/postgres-exporter/config from none to 623c00
--- /var/log/gitlab/postgres-exporter/config    2019-12-14 17:26:52.736569000 +0000
+++ /var/log/gitlab/postgres-exporter/.chef-config20191214-12-jqu0vb    2019-12-14 17:26:52.736569000 +0000
@@ -1 +1,7 @@
+s209715200
+n30
+t86400
+!gzip
+
+
- change mode from '' to '0644'
- change owner from '' to 'root'
- change group from '' to 'root'
* ruby_block[verify_chown_persisted_on_postgres-exporter] action create
- execute the ruby block verify_chown_persisted_on_postgres-exporter
* ruby_block[verify_chown_persisted_on_postgres-exporter] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter/env] action create
- create new directory /opt/gitlab/sv/postgres-exporter/env
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* ruby_block[Delete unmanaged env files for postgres-exporter service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/postgres-exporter/control] action create
- create new directory /opt/gitlab/sv/postgres-exporter/control
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* link[/opt/gitlab/init/postgres-exporter] action create
- create symlink at /opt/gitlab/init/postgres-exporter to /opt/gitlab/embedded/bin/sv
* file[/opt/gitlab/sv/postgres-exporter/down] action delete (up to date)
* ruby_block[restart_service] action run (skipped due to only_if)
* ruby_block[restart_log_service] action create
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter] action create (up to date)
* template[/opt/gitlab/sv/postgres-exporter/run] action create (up to date)
* directory[/opt/gitlab/sv/postgres-exporter/log] action create (up to date)
* directory[/opt/gitlab/sv/postgres-exporter/log/main] action create (up to date)
* template[/opt/gitlab/sv/postgres-exporter/log/run] action create (up to date)
* template[/var/log/gitlab/postgres-exporter/config] action create (up to date)
* ruby_block[verify_chown_persisted_on_postgres-exporter] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter/env] action create (up to date)
* ruby_block[Delete unmanaged env files for postgres-exporter service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/postgres-exporter/control] action create (up to date)
* link[/opt/gitlab/init/postgres-exporter] action create (up to date)
* file[/opt/gitlab/sv/postgres-exporter/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/postgres-exporter] action create
- create symlink at /opt/gitlab/service/postgres-exporter to /opt/gitlab/sv/postgres-exporter
* ruby_block[wait for postgres-exporter service socket] action run
- execute the ruby block wait for postgres-exporter service socket
- execute the ruby block restart_log_service
* ruby_block[reload_log_service] action create
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter] action create (up to date)
* template[/opt/gitlab/sv/postgres-exporter/run] action create (up to date)
* directory[/opt/gitlab/sv/postgres-exporter/log] action create (up to date)
* directory[/opt/gitlab/sv/postgres-exporter/log/main] action create (up to date)
* template[/opt/gitlab/sv/postgres-exporter/log/run] action create (up to date)
* template[/var/log/gitlab/postgres-exporter/config] action create (up to date)
* ruby_block[verify_chown_persisted_on_postgres-exporter] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/postgres-exporter/env] action create (up to date)
* ruby_block[Delete unmanaged env files for postgres-exporter service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/postgres-exporter/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/postgres-exporter/control] action create (up to date)
* link[/opt/gitlab/init/postgres-exporter] action create (up to date)
* file[/opt/gitlab/sv/postgres-exporter/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/postgres-exporter] action create (up to date)
* ruby_block[wait for postgres-exporter service socket] action run (skipped due to not_if)
- execute the ruby block reload_log_service
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/postgres-exporter] action create (up to date)
* ruby_block[wait for postgres-exporter service socket] action run (skipped due to not_if)

* template[/var/opt/gitlab/postgres-exporter/queries.yaml] action create
- create new file /var/opt/gitlab/postgres-exporter/queries.yaml
- update content in file /var/opt/gitlab/postgres-exporter/queries.yaml from none to 40142b
--- /var/opt/gitlab/postgres-exporter/queries.yaml    2019-12-14 17:26:59.926569000 +0000
+++ /var/opt/gitlab/postgres-exporter/.chef-queries20191214-12-gybaij.yaml    2019-12-14 17:26:59.926569000 +0000
@@ -1 +1,175 @@
+pg_replication:
+  query: "SELECT EXTRACT(EPOCH FROM (now() - pg_last_xact_replay_timestamp()))::INT as lag, CASE WHEN pg_is_in_recovery() THEN 1 ELSE 0 END as is_replica"
+  metrics:
+    - lag:
+        usage: "GAUGE"
+        description: "Replication lag behind master in seconds"
+    - is_replica:
+        usage: "GAUGE"
+        description: "Indicates if this host is a slave"
+
+pg_postmaster:
+  query: "SELECT pg_postmaster_start_time as start_time_seconds from pg_postmaster_start_time()"
+  metrics:
+    - start_time_seconds:
+        usage: "GAUGE"
+        description: "Time at which postmaster started"
+
+pg_stat_user_tables:
+  query: "SELECT schemaname, relname, seq_scan, seq_tup_read, idx_scan, idx_tup_fetch, n_tup_ins, n_tup_upd, n_tup_del, n_tup_hot_upd, n_live_tup, n_dead_tup, n_mod_since_analyze, last_vacuum, last_autovacuum, last_analyze, last_autoanalyze, vacuum_count, autovacuum_count, analyze_count, autoanalyze_count FROM pg_stat_user_tables"
+  metrics:
+    - schemaname:
+        usage: "LABEL"
+        description: "Name of the schema that this table is in"
+    - relname:
+        usage: "LABEL"
+        description: "Name of this table"
+    - seq_scan:
+        usage: "COUNTER"
+        description: "Number of sequential scans initiated on this table"
+    - seq_tup_read:
+        usage: "COUNTER"
+        description: "Number of live rows fetched by sequential scans"
+    - idx_scan:
+        usage: "COUNTER"
+        description: "Number of index scans initiated on this table"
+    - idx_tup_fetch:
+        usage: "COUNTER"
+        description: "Number of live rows fetched by index scans"
+    - n_tup_ins:
+        usage: "COUNTER"
+        description: "Number of rows inserted"
+    - n_tup_upd:
+        usage: "COUNTER"
+        description: "Number of rows updated"
+    - n_tup_del:
+        usage: "COUNTER"
+        description: "Number of rows deleted"
+    - n_tup_hot_upd:
+        usage: "COUNTER"
+        description: "Number of rows HOT updated (i.e., with no separate index update required)"
+    - n_live_tup:
+        usage: "GAUGE"
+        description: "Estimated number of live rows"
+    - n_dead_tup:
+        usage: "GAUGE"
+        description: "Estimated number of dead rows"
+    - n_mod_since_analyze:
+        usage: "GAUGE"
+        description: "Estimated number of rows changed since last analyze"
+    - last_vacuum:
+        usage: "GAUGE"
+        description: "Last time at which this table was manually vacuumed (not counting VACUUM FULL)"
+    - last_autovacuum:
+        usage: "GAUGE"
+        description: "Last time at which this table was vacuumed by the autovacuum daemon"
+    - last_analyze:
+        usage: "GAUGE"
+        description: "Last time at which this table was manually analyzed"
+    - last_autoanalyze:
+        usage: "GAUGE"
+        description: "Last time at which this table was analyzed by the autovacuum daemon"
+    - vacuum_count:
+        usage: "COUNTER"
+        description: "Number of times this table has been manually vacuumed (not counting VACUUM FULL)"
+    - autovacuum_count:
+        usage: "COUNTER"
+        description: "Number of times this table has been vacuumed by the autovacuum daemon"
+    - analyze_count:
+        usage: "COUNTER"
+        description: "Number of times this table has been manually analyzed"
+    - autoanalyze_count:
+        usage: "COUNTER"
+        description: "Number of times this table has been analyzed by the autovacuum daemon"
+
+pg_total_relation_size:
+  query: |
+    SELECT relnamespace::regnamespace as schemaname,
+           relname as relname,
+           pg_total_relation_size(oid) bytes
+      FROM pg_class
+     WHERE relkind = 'r';
+  metrics:
+    - schemaname:
+        usage: "LABEL"
+        description: "Name of the schema that this table is in"
+    - relname:
+        usage: "LABEL"
+        description: "Name of this table"
+    - bytes:
+        usage: "GAUGE"
+        description: "total disk space usage for the specified table and associated indexes"
+
+pg_blocked:
+  query: |
+    SELECT
+      count(blocked.transactionid) AS queries,
+      '__transaction__' AS table
+    FROM pg_catalog.pg_locks blocked
+    WHERE NOT blocked.granted AND locktype = 'transactionid'
+    GROUP BY locktype
+    UNION
+    SELECT
+      count(blocked.relation) AS queries,
+      blocked.relation::regclass::text AS table
+    FROM pg_catalog.pg_locks blocked
+    WHERE NOT blocked.granted AND locktype != 'transactionid'
+    GROUP BY relation
+  metrics:
+    - queries:
+        usage: "GAUGE"
+        description: "The current number of blocked queries"
+    - table:
+        usage: "LABEL"
+        description: "The table on which a query is blocked"
+
+pg_slow:
+  query: |
+    SELECT COUNT(*) AS queries
+    FROM pg_stat_activity
+    WHERE state = 'active' AND (now() - query_start) > '1 seconds'::interval
+  metrics:
+    - queries:
+        usage: "GAUGE"
+        description: "Current number of slow queries"
+
+pg_vacuum:
+  query: |
+    SELECT
+      COUNT(*) AS queries,
+      MAX(EXTRACT(EPOCH FROM (clock_timestamp() - query_start))) AS age_in_seconds
+    FROM pg_catalog.pg_stat_activity
+    WHERE state = 'active' AND trim(query) ~* '\AVACUUM (?!ANALYZE)'
+  metrics:
+    - queries:
+        usage: "GAUGE"
+        description: "The current number of VACUUM queries"
+    - age_in_seconds:
+        usage: "GAUGE"
+        description: "The current maximum VACUUM query age in seconds"
+
+pg_vacuum_analyze:
+  query: |
+    SELECT
+      COUNT(*) AS queries,
+      MAX(EXTRACT(EPOCH FROM (clock_timestamp() - query_start))) AS age_in_seconds
+    FROM pg_catalog.pg_stat_activity
+    WHERE state = 'active' AND trim(query) ~* '\AVACUUM ANALYZE'
+  metrics:
+    - queries:
+        usage: "GAUGE"
+        description: "The current number of VACUUM ANALYZE queries"
+    - age_in_seconds:
+        usage: "GAUGE"
+        description: "The current maximum VACUUM ANALYZE query age in seconds"
+
+pg_stuck_idle_in_transaction:
+  query: |
+    SELECT COUNT(*) AS queries
+    FROM pg_stat_activity
+    WHERE state = 'idle in transaction' AND (now() - query_start) > '10 minutes'::interval
+  metrics:
+    - queries:
+        usage: "GAUGE"
+        description: "Current number of queries that are stuck being idle in transactions"
- change mode from '' to '0644'
- change owner from '' to 'gitlab-psql'
* execute[/opt/gitlab/bin/gitlab-ctl start postgres-exporter] action run
[execute] ok: run: postgres-exporter: (pid 1072) 5s
- execute /opt/gitlab/bin/gitlab-ctl start postgres-exporter
* consul_service[postgres-exporter] action delete
* file[/var/opt/gitlab/consul/config.d/postgres-exporter-service.json] action delete (up to date)
(up to date)
Recipe: monitoring::grafana
* directory[/var/log/gitlab/grafana] action create
- create new directory /var/log/gitlab/grafana
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* directory[/var/opt/gitlab/grafana] action create
- create new directory /var/opt/gitlab/grafana
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* directory[/var/opt/gitlab/grafana/provisioning] action create
- create new directory /var/opt/gitlab/grafana/provisioning
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* directory[/var/opt/gitlab/grafana/provisioning/dashboards] action create
- create new directory /var/opt/gitlab/grafana/provisioning/dashboards
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* directory[/var/opt/gitlab/grafana/provisioning/datasources] action create
- create new directory /var/opt/gitlab/grafana/provisioning/datasources
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* directory[/var/opt/gitlab/grafana/provisioning/notifiers] action create
- create new directory /var/opt/gitlab/grafana/provisioning/notifiers
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* link[/var/opt/gitlab/grafana/conf] action create
- create symlink at /var/opt/gitlab/grafana/conf to /opt/gitlab/embedded/service/grafana/conf
* link[/var/opt/gitlab/grafana/public] action create
- create symlink at /var/opt/gitlab/grafana/public to /opt/gitlab/embedded/service/grafana/public
* directory[/opt/gitlab/etc/grafana/env] action create
- create new directory /opt/gitlab/etc/grafana/env
- change mode from '' to '0700'
- change owner from '' to 'gitlab-prometheus'
* ruby_block[authorize Grafana with GitLab] action run
- execute the ruby block authorize Grafana with GitLab
* ruby_block[populate Grafana configuration options] action run
- execute the ruby block populate Grafana configuration options
* env_dir[/opt/gitlab/etc/grafana/env] action create
* directory[/opt/gitlab/etc/grafana/env] action create (up to date)
* file[/opt/gitlab/etc/grafana/env/SSL_CERT_DIR] action create
- create new file /opt/gitlab/etc/grafana/env/SSL_CERT_DIR
- update content in file /opt/gitlab/etc/grafana/env/SSL_CERT_DIR from none to 4f45cf
--- /opt/gitlab/etc/grafana/env/SSL_CERT_DIR    2019-12-14 17:29:31.627204000 +0000
+++ /opt/gitlab/etc/grafana/env/.chef-SSL_CERT_DIR20191214-12-1h2t5dt    2019-12-14 17:29:31.627204000 +0000
@@ -1 +1,2 @@
+/opt/gitlab/embedded/ssl/certs/

* template[/var/opt/gitlab/grafana/grafana.ini] action create
- create new file /var/opt/gitlab/grafana/grafana.ini
- update content in file /var/opt/gitlab/grafana/grafana.ini from none to 8466e9
--- /var/opt/gitlab/grafana/grafana.ini    2019-12-14 17:29:31.927204000 +0000
+++ /var/opt/gitlab/grafana/.chef-grafana20191214-12-gjj5jz.ini    2019-12-14 17:29:31.927204000 +0000
@@ -1 +1,401 @@
+##################### GitLab Grafana Configuration #####################
+#
+# Everything has defaults so you only need to uncomment things you want to
+# change
+
+# possible values : production, development
+;app_mode = production
+
+# instance name, defaults to HOSTNAME environment variable value or hostname if HOSTNAME var is empty
+;instance_name = ${HOSTNAME}
+
+#################################### Paths ####################################
+[paths]
+# Path to where grafana can store temp files, sessions, and the sqlite3 db (if that is used)
+data = /var/opt/gitlab/grafana/data
+
+# Temporary files in `data` directory older than given duration will be removed
+;temp_data_lifetime = 24h
+
+# Directory where grafana can store logs
+logs = /var/log/gitlab/grafana
+
+# Directory where grafana will automatically scan and look for plugins
+;plugins = /var/lib/grafana/plugins
+
+# folder that contains provisioning config files that grafana will apply on startup and while running.
+provisioning = provisioning
+
+#################################### Server ####################################
+[server]
+# Protocol (http, https, socket)
+protocol = http
+
+# The ip address to bind to, empty will bind to all interfaces
+http_addr = localhost
+
+# The http port  to use
+http_port = 3000
+
+# The public facing domain name used to access grafana from a browser
+;domain = localhost
+
+# Redirect to correct domain if host header does not match domain
+# Prevents DNS rebinding attacks
+enforce_domain = false
+
+# The full public facing url you use in browser, used for redirects and emails
+# If you use reverse proxy and sub path specify full url (with sub path)
+root_url = http://gitlab.example.com/-/grafana
+
+# Log web requests
+;router_logging = false
+
+# the path relative working path
+;static_root_path = public
+
+# enable gzip
+;enable_gzip = false
+
+# https certs & key file
+;cert_file =
+;cert_key =
+
+# Unix socket path
+;socket =
+
+#################################### Database ####################################
+[database]
+# You can configure the database connection by specifying type, host, name, user and password
+# as separate properties or as on string using the url properties.
+
+# Either "mysql", "postgres" or "sqlite3", it's your choice
+;type = sqlite3
+;host = 127.0.0.1:3306
+;name = grafana
+;user = root
+# If the password contains # or ; you have to wrap it with triple quotes. Ex """#password;"""
+;password =
+
+# Use either URL or the previous fields to configure the database
+# Example: mysql://user:secret@host:port/database
+;url =
+
+# For "postgres" only, either "disable", "require" or "verify-full"
+;ssl_mode = disable
+
+# For "sqlite3" only, path relative to data_path setting
+;path = grafana.db
+
+# Max idle conn setting default is 2
+;max_idle_conn = 2
+
+# Max conn setting default is 0 (mean not set)
+;max_open_conn =
+
+# Connection Max Lifetime default is 14400 (means 14400 seconds or 4 hours)
+;conn_max_lifetime = 14400
+
+# Set to true to log the sql calls and execution times.
+log_queries =
+
+#################################### Session ####################################
+[session]
+# Either "memory", "file", "redis", "mysql", "postgres", default is "file"
+;provider = file
+
+# Provider config options
+# memory: not have any config yet
+# file: session dir path, is relative to grafana data_path
+# redis: config like redis server e.g. `addr=127.0.0.1:6379,pool_size=100,db=grafana`
+# mysql: go-sql-driver/mysql dsn config string, e.g. `user:password@tcp(127.0.0.1:3306)/database_name`
+# postgres: user=a password=b host=localhost port=5432 dbname=c sslmode=disable
+;provider_config = sessions
+
+# Session cookie name
+;cookie_name = grafana_sess
+
+# If you use session in https only, default is false
+;cookie_secure = false
+
+# Session life time, default is 86400
+;session_life_time = 86400
+
+#################################### Data proxy ###########################
+[dataproxy]
+
+# This enables data proxy logging, default is false
+;logging = false
+
+#################################### Analytics ####################################
+[analytics]
+# Server reporting, sends usage counters to stats.grafana.org every 24 hours.
+# No ip addresses are being tracked, only simple counters to track
+# running instances, dashboard and error counts. It is very helpful to us.
+# Change this option to false to disable reporting.
+;reporting_enabled = true
+
+# Set to false to disable all checks to https://grafana.net
+# for new vesions (grafana itself and plugins), check is used
+# in some UI views to notify that grafana or plugin update exists
+# This option does not cause any auto updates, nor send any information
+# only a GET request to http://grafana.com to get latest versions
+;check_for_updates = true
+
+# Google Analytics universal tracking code, only enabled if you specify an id here
+;google_analytics_ua_id =
+
+#################################### Security ####################################
+[security]
+# default admin user, created on startup
+;admin_user = admin
+
+# default admin password, can be changed before first start of grafana,  or in profile settings
+admin_password = 05a50b3da5a43414d1a0a7140f858bc3
+
+# used for signing
+secret_key = 3e8d9a61715caad1b7866e1d9a4a7a6f
+
+# Auto-login remember days
+;login_remember_days = 7
+;cookie_username = grafana_user
+;cookie_remember_name = grafana_remember
+
+# disable gravatar profile images
+;disable_gravatar = false
+
+# data source proxy whitelist (ip_or_domain:port separated by spaces)
+;data_source_proxy_whitelist =
+
+# disable protection against brute force login attempts
+;disable_brute_force_login_protection = false
+
+#################################### Snapshots ###########################
+[snapshots]
+# snapshot sharing options
+;external_enabled = true
+;external_snapshot_url = https://snapshots-origin.raintank.io
+;external_snapshot_name = Publish to snapshot.raintank.io
+
+# remove expired snapshot
+;snapshot_remove_expired = true
+
+#################################### Dashboards History ##################
+[dashboards]
+# Number dashboard versions to keep (per dashboard). Default: 20, Minimum: 1
+;versions_to_keep = 20
+
+#################################### Users ###############################
+[users]
+# disable user signup / registration
+allow_sign_up = false
+
+# Allow non admin users to create organizations
+;allow_org_create = true
+
+# Set to true to automatically assign new users to the default organization (id 1)
+;auto_assign_org = true
+
+# Default role new users will be automatically assigned (if disabled above is set to true)
+;auto_assign_org_role = Viewer
+
+# Background text for the user field on the login page
+;login_hint = email or username
+
+# Default UI theme ("dark" or "light")
+;default_theme = dark
+
+# External user management, these options affect the organization users view
+;external_manage_link_url =
+;external_manage_link_name =
+;external_manage_info =
+
+# Viewers can edit/inspect dashboard settings in the browser. But not save the dashboard.
+;viewers_can_edit = false
+
+[auth]
+# Set to true to disable (hide) the login form, useful if you use OAuth, defaults to false
+;disable_login_form = false
+
+# Set to true to disable the signout link in the side menu. useful if you use auth.proxy, defaults to false
+;disable_signout_menu = false
+
+# URL to redirect the user to after sign out
+;signout_redirect_url =
+
+# Set to true to attempt login with OAuth automatically, skipping the login screen.
+# This setting is ignored if multiple OAuth providers are configured.
+;oauth_auto_login = false
+
+#################################### Anonymous Auth ##########################
+[auth.anonymous]
+# enable anonymous access
+;enabled = false
+
+# specify organization name that should be used for unauthenticated users
+;org_name = Main Org.
+
+# specify role for unauthenticated users
+;org_role = Viewer
+
+#################################### GitLab Auth ##########################
+[auth.gitlab]
+enabled = true
+allow_sign_up = true
+client_id = 23b8dbdd01d88ee838bd1d2e9a6dee974e23081d1689125117b0a1870b2ad30a
+client_secret = 6f0f9f688d7e558790a260d5c071eb7e4b7aeb6d8cb91b5d6f5fc089aa94b4c1
+scopes = api
+auth_url = http://gitlab.example.com/oauth/authorize
+token_url = http://gitlab.example.com/oauth/token
+api_url = http://gitlab.example.com/api/v4
+allowed_groups =
+
+#################################### Auth Proxy ##########################
+[auth.proxy]
+;enabled = false
+;header_name = X-WEBAUTH-USER
+;header_property = username
+;auto_sign_up = true
+;ldap_sync_ttl = 60
+;whitelist = 192.168.1.1, 192.168.2.1
+;headers = Email:X-User-Email, Name:X-User-Name
+
+#################################### Basic Auth ##########################
+[auth.basic]
+enabled = false
+disable_login_form = true
+
+#################################### Auth LDAP ##########################
+[auth.ldap]
+;enabled = false
+;config_file = /etc/grafana/ldap.toml
+;allow_sign_up = true
+
+#################################### SMTP / Emailing ##########################
+[smtp]
+;enabled = false
+;host = localhost:25
+;user =
+# If the password contains # or ; you have to wrap it with trippel quotes. Ex """#password;"""
+;password =
+;cert_file =
+;key_file =
+;skip_verify = false
+;from_address = admin@grafana.localhost
+;from_name = Grafana
+# EHLO identity in SMTP dialog (defaults to instance_name)
+;ehlo_identity = dashboard.example.com
+
+[emails]
+;welcome_email_on_sign_up = false
+
+#################################### Logging ##########################
+[log]
+# Either "console", "file", "syslog". Default is console and  file
+# Use space to separate multiple modes, e.g. "console file"
+mode = console
+
+# Either "debug", "info", "warn", "error", "critical", default is "info"
+;level = info
+
+# optional settings to set different levels for specific loggers. Ex filters = sqlstore:debug
+;filters =
+
+# For "console" mode only
+[log.console]
+level = info
+
+# log line format, valid options are text, console and json
+format = text
+
+#################################### Alerting ############################
+[alerting]
+# Disable alerting engine & UI features
+enabled = false
+
+#################################### Explore #############################
+[explore]
+# Enable the Explore section
+;enabled = false
+
+#################################### Internal Grafana Metrics ##########################
+# Metrics available at HTTP API Url /metrics
+[metrics]
+# Disable / Enable internal metrics
+enabled = false
+
+# Publish interval
+;interval_seconds  = 10
+
+
+
+# Send internal metrics to Graphite
+[metrics.graphite]
+# Enable by setting the address setting (ex localhost:2003)
+;address =
+;prefix = prod.grafana.%(instance_name)s.
+
+#################################### Distributed tracing ############
+[tracing.jaeger]
+# Enable by setting the address sending traces to jaeger (ex localhost:6831)
+;address = localhost:6831
+# Tag that will always be included in when creating new spans. ex (tag1:value1,tag2:value2)
+;always_included_tag = tag1:value1
+# Type specifies the type of the sampler: const, probabilistic, rateLimiting, or remote
+;sampler_type = const
+# jaeger samplerconfig param
+# for "const" sampler, 0 or 1 for always false/true respectively
+# for "probabilistic" sampler, a probability between 0 and 1
+# for "rateLimiting" sampler, the number of spans per second
+# for "remote" sampler, param is the same as for "probabilistic"
+# and indicates the initial sampling rate before the actual one
+# is received from the mothership
+;sampler_param = 1
+
+#################################### Grafana.com integration  ##########################
+# Url used to import dashboards directly from Grafana.com
+[grafana_com]
+;url = https://grafana.com
+
+#################################### External image storage ##########################
+[external_image_storage]
+# Used for uploading images to public servers so they can be included in slack/email messages.
+# you can choose between (s3, webdav, gcs, azure_blob, local)
+;provider =
+
+[external_image_storage.s3]
+;bucket =
+;region =
+;path =
+;access_key =
+;secret_key =
+
+[external_image_storage.webdav]
+;url =
+;public_url =
+;username =
+;password =
+
+[external_image_storage.gcs]
+;key_file =
+;bucket =
+;path =
+
+[external_image_storage.azure_blob]
+;account_name =
+;account_key =
+;container_name =
+
+[external_image_storage.local]
+# does not require any configuration
+
+[rendering]
+# Options to configure external image rendering server like https://github.com/grafana/grafana-image-renderer
+;server_url =
+;callback_url =
+
+[enterprise]
+# Path to a valid Grafana Enterprise license.jwt file
+;license_path =
+
- change mode from '' to '0644'
- change owner from '' to 'gitlab-prometheus'
* file[/var/opt/gitlab/grafana/provisioning/dashboards/gitlab_dashboards.yml] action create
- create new file /var/opt/gitlab/grafana/provisioning/dashboards/gitlab_dashboards.yml
- update content in file /var/opt/gitlab/grafana/provisioning/dashboards/gitlab_dashboards.yml from none to aa31a1
--- /var/opt/gitlab/grafana/provisioning/dashboards/gitlab_dashboards.yml    2019-12-14 17:29:32.067204000 +0000
+++ /var/opt/gitlab/grafana/provisioning/dashboards/.chef-gitlab_dashboards20191214-12-5hd5r7.yml    2019-12-14 17:29:32.067204000 +0000
@@ -1 +1,12 @@
+---
+apiVersion: 1
+providers:
+- name: GitLab Omnibus
+  orgId: 1
+  folder: GitLab Omnibus
+  type: file
+  disableDeletion: true
+  updateIntervalSeconds: 600
+  options:
+    path: "/opt/gitlab/embedded/service/grafana-dashboards"
- change mode from '' to '0644'
- change owner from '' to 'gitlab-prometheus'
* file[/var/opt/gitlab/grafana/provisioning/datasources/gitlab_datasources.yml] action create
- create new file /var/opt/gitlab/grafana/provisioning/datasources/gitlab_datasources.yml
- update content in file /var/opt/gitlab/grafana/provisioning/datasources/gitlab_datasources.yml from none to 2041c0
--- /var/opt/gitlab/grafana/provisioning/datasources/gitlab_datasources.yml    2019-12-14 17:29:32.077204000 +0000
+++ /var/opt/gitlab/grafana/provisioning/datasources/.chef-gitlab_datasources20191214-12-1l57n5z.yml    2019-12-14 17:29:32.077204000 +0000
@@ -1 +1,9 @@
+---
+apiVersion: 1
+datasources:
+- name: GitLab Omnibus
+  type: prometheus
+  access: proxy
+  url: http://localhost:9090
+  isDefault: true
- change mode from '' to '0644'
- change owner from '' to 'gitlab-prometheus'
Recipe: <Dynamically Defined Resource>
* service[grafana] action nothing (skipped due to action :nothing)
Recipe: monitoring::grafana
* runit_service[grafana] action enable
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana] action create
- create new directory /opt/gitlab/sv/grafana
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/opt/gitlab/sv/grafana/run] action create
- create new file /opt/gitlab/sv/grafana/run
- update content in file /opt/gitlab/sv/grafana/run from none to b54d7b
--- /opt/gitlab/sv/grafana/run    2019-12-14 17:29:32.897204000 +0000
+++ /opt/gitlab/sv/grafana/.chef-run20191214-12-1irjiva    2019-12-14 17:29:32.897204000 +0000
@@ -1 +1,12 @@
+#!/bin/sh
+exec 2>&1
+
+
+cd '/var/opt/gitlab/grafana'
+
+umask 077
+exec chpst -P -e /opt/gitlab/etc/grafana/env \
+  -U gitlab-prometheus:gitlab-prometheus \
+  -u gitlab-prometheus:gitlab-prometheus \
+  /opt/gitlab/embedded/bin/grafana-server -config '/var/opt/gitlab/grafana/grafana.ini'
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* directory[/opt/gitlab/sv/grafana/log] action create
- create new directory /opt/gitlab/sv/grafana/log
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* directory[/opt/gitlab/sv/grafana/log/main] action create
- create new directory /opt/gitlab/sv/grafana/log/main
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/opt/gitlab/sv/grafana/log/run] action create
- create new file /opt/gitlab/sv/grafana/log/run
- update content in file /opt/gitlab/sv/grafana/log/run from none to 49180c
--- /opt/gitlab/sv/grafana/log/run    2019-12-14 17:29:33.097204000 +0000
+++ /opt/gitlab/sv/grafana/log/.chef-run20191214-12-1s3kmvz    2019-12-14 17:29:33.097204000 +0000
@@ -1 +1,3 @@
+#!/bin/sh
+exec svlogd -tt /var/log/gitlab/grafana
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* template[/var/log/gitlab/grafana/config] action create
- create new file /var/log/gitlab/grafana/config
- update content in file /var/log/gitlab/grafana/config from none to 623c00
--- /var/log/gitlab/grafana/config    2019-12-14 17:29:33.227204000 +0000
+++ /var/log/gitlab/grafana/.chef-config20191214-12-ywo4s5    2019-12-14 17:29:33.227204000 +0000
@@ -1 +1,7 @@
+s209715200
+n30
+t86400
+!gzip
+
+
- change mode from '' to '0644'
- change owner from '' to 'root'
- change group from '' to 'root'
* ruby_block[verify_chown_persisted_on_grafana] action create
- execute the ruby block verify_chown_persisted_on_grafana
* ruby_block[verify_chown_persisted_on_grafana] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana/env] action create
- create new directory /opt/gitlab/sv/grafana/env
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* ruby_block[Delete unmanaged env files for grafana service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/grafana/control] action create
- create new directory /opt/gitlab/sv/grafana/control
- change mode from '' to '0755'
- change owner from '' to 'root'
- change group from '' to 'root'
* link[/opt/gitlab/init/grafana] action create
- create symlink at /opt/gitlab/init/grafana to /opt/gitlab/embedded/bin/sv
* file[/opt/gitlab/sv/grafana/down] action delete (up to date)
* ruby_block[restart_service] action run (skipped due to only_if)
* ruby_block[restart_log_service] action create
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana] action create (up to date)
* template[/opt/gitlab/sv/grafana/run] action create (up to date)
* directory[/opt/gitlab/sv/grafana/log] action create (up to date)
* directory[/opt/gitlab/sv/grafana/log/main] action create (up to date)
* template[/opt/gitlab/sv/grafana/log/run] action create (up to date)
* template[/var/log/gitlab/grafana/config] action create (up to date)
* ruby_block[verify_chown_persisted_on_grafana] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana/env] action create (up to date)
* ruby_block[Delete unmanaged env files for grafana service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/grafana/control] action create (up to date)
* link[/opt/gitlab/init/grafana] action create (up to date)
* file[/opt/gitlab/sv/grafana/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/grafana] action create
- create symlink at /opt/gitlab/service/grafana to /opt/gitlab/sv/grafana
* ruby_block[wait for grafana service socket] action run
- execute the ruby block wait for grafana service socket
- execute the ruby block restart_log_service
* ruby_block[reload_log_service] action create
* ruby_block[restart_service] action nothing (skipped due to action :nothing)
* ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
* ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana] action create (up to date)
* template[/opt/gitlab/sv/grafana/run] action create (up to date)
* directory[/opt/gitlab/sv/grafana/log] action create (up to date)
* directory[/opt/gitlab/sv/grafana/log/main] action create (up to date)
* template[/opt/gitlab/sv/grafana/log/run] action create (up to date)
* template[/var/log/gitlab/grafana/config] action create (up to date)
* ruby_block[verify_chown_persisted_on_grafana] action nothing (skipped due to action :nothing)
* directory[/opt/gitlab/sv/grafana/env] action create (up to date)
* ruby_block[Delete unmanaged env files for grafana service] action run (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/check] action create (skipped due to only_if)
* template[/opt/gitlab/sv/grafana/finish] action create (skipped due to only_if)
* directory[/opt/gitlab/sv/grafana/control] action create (up to date)
* link[/opt/gitlab/init/grafana] action create (up to date)
* file[/opt/gitlab/sv/grafana/down] action delete (up to date)
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/grafana] action create (up to date)
* ruby_block[wait for grafana service socket] action run (skipped due to not_if)
- execute the ruby block reload_log_service
* directory[/opt/gitlab/service] action create (up to date)
* link[/opt/gitlab/service/grafana] action create (up to date)
* ruby_block[wait for grafana service socket] action run (skipped due to not_if)

* execute[/opt/gitlab/bin/gitlab-ctl start grafana] action run
[execute] ok: run: grafana: (pid 1398) 13s
- execute /opt/gitlab/bin/gitlab-ctl start grafana
Recipe: gitlab::gitlab-rails
* execute[clear the gitlab-rails cache] action run
- execute /opt/gitlab/bin/gitlab-rake cache:clear
Recipe: <Dynamically Defined Resource>
* service[gitaly] action restart
- restart service service[gitaly]
Recipe: gitaly::enable
* runit_service[gitaly] action hup
- send hup to runit_service[gitaly]
Recipe: <Dynamically Defined Resource>
* service[gitlab-workhorse] action restart
- restart service service[gitlab-workhorse]
* service[node-exporter] action restart
- restart service service[node-exporter]
* service[gitlab-exporter] action restart
- restart service service[gitlab-exporter]
* service[redis-exporter] action restart
- restart service service[redis-exporter]
* service[prometheus] action restart
- restart service service[prometheus]
Recipe: monitoring::prometheus
* execute[reload prometheus] action run
- execute /opt/gitlab/bin/gitlab-ctl hup prometheus
Recipe: <Dynamically Defined Resource>
* service[alertmanager] action restart
- restart service service[alertmanager]
* service[postgres-exporter] action restart
- restart service service[postgres-exporter]
* service[grafana] action restart
- restart service service[grafana]

Running handlers:
Running handlers complete
Chef Client finished, 539/1443 resources updated in 07 minutes 14 seconds
gitlab Reconfigured!
il-j2% cat Dockerfile
FROM ubuntu:16.04

MAINTAINER apearl@student.21-school.ru

RUN apt-get update && \
apt-get upgrade -y && \
apt-get install -y ca-certificates openssh-server wget postfix

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && chmod 777 script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce

RUN apt update && apt install -y tzdata && \
apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

http://localhost:8080/


docker build -t ex03 .
docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
