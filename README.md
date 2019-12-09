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
docker-machine pull hello-world
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


20. Create a local swarm, the Char virtual machine should be its manager.
il-j2% docker swarm init --advertise-addr $(docker-machine ip Char)
Swarm initialized: current node (n6mimodq4rlv0kbpti90pyau6) is now a manager.

To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-2yh43nsm0zrgfl9ce6vzp3m76g9d6yeooo6euqgt32clqau2fy-23xknjm9lvsquziw6n65cewoh 192.168.99.109:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

