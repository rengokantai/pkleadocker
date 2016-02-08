#### pkleadocker
check docker info
```
sudo docker -D info
```

pull images from web
```
sudo docker pull busybox
```
list all images
```
sudo docker images
```

search images:
```
sudo docker search mysql
```

press ctrl+p and ctrl+q to detach a container,  
and run 
```
sudo docker ps
```
to list all running docker ps  
To attach
```
sudo docker attach NAME
```

stop and start
```
sudo docker stop c437777
```
to see all ps, including stopped
```
sudo docker ps -a
```
```
sudo docker start 437777 (must attach to run)
```

after log in,  create a file, then diff command
```
docker diff 437777
```

pause and unpause
```
sudo docker run -i -t ubuntu:14.04 /bin/bash
sudo docker pause c437777
```
But when executing 
```
sudo docker ps -a
```
We may see some stopped containers.
By default, the container will not be erased after stopping, to remove it, use --rm
```
sudo docker run -i -t --rm ubuntu:14.04 /bin/bash
```
Or, we can explicitly kill by
```
sudo docker rm 1232423a
```
Same as
```
sudo docker rm 'sudo docker ps -aq --no-trunc'
```

run docker as deamon
```
docker run -d ubuntu:14.04
```

show logs
```
docker logs 43777
```

-Dockerfile
basic:if Dockerfile is in current dict,
```
sudo docker build .
```

It will show 'Successfully built 12324'
Then
```
sudo docker run 1234
```

some Dockerfile commands:
basic: FROM, MAINTAINER  
CMD-> run when new container starts  
RUN->run at build time  
ENTRYPOINT->assign commands. can be overriden  
Other instructions: USER(login user), EXPOSE (port number),COPY ,ADD ,WORKDIR 
Rarely use: ONBUILD, VOLUME
```
sudo docker run --entrypoint="/bin/sh" -it yd
```
show biuld history
```
sudo docker history yd
```

By default, the tag name is empty,
Add a tag:
```
sudo docker tag 1234 imagename:tagname
```
or define it when building (syntax: imagename:tagname, by default, tagname=latest)
```
sudo docker build -t imagename .
```


inspect tar file:
```
tar tf file.tar
```


- Pushing images to the Docker Hub
```
sudo docker run -t --name="yournewname" -t ubuntu /bin/bash
```
Then make some changes in container:
```
mkdir newdolder
```
Commit:
```
sudo docker commit -m="message" yournewname yourdockeraccount/imagename
```
push:
```
sudo docker push yourdockeraccount/imagename
sudo docker login
```
Delete:
```
sudo docker rmi yourdockeraccount/imagename
```

Because we cannot find our newly created image locally, we need to pull from remote
```
sudo docker run -i -name="yournewname" -t yourdockeraccount/imagename
```

OR, we can build image from dockerfile
```
sudo docker build -t="yourdockeraccount/imagename" .
sudo docker run -t --name="yournewname" -t yourdockeraccount/imagename
sudo docker push yourdockeraccount/imagename
sudo docker login
```

#### cp6
to check ip of container:
```
docker inspect <idofcontainer>
```

Ubuntu install apache2 (in centos is httpd), in Dockerfile
```
RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOG_DIR /var/log/apache2
```
set entrypoint.(must run in foreground)
```
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```
build and run
```
sudo docker build -t apache2 .
sudo docker run -d apache2
sudo docker logs 43777
```

inspect ip
```
sudo docker inspect --format='{{.NetworkSettings.IPAddress}}' 43777
```
get ip and connect
```
wget -qO- 172.17.0.1
```

-p parameter. syntax: localip:containerip
```
sudo docker run -d -p 180:80 yd //not a good solution, we can use `sudo docker run -d -p 80 yd` to auto-generate ip
```
check use iptables
```
sudo iptables -t nat -L -n
```

Or more specific
```
ubuntu@ip-172-31-56-213:~$ sudo docker inspect --format='{{.Config.ExposedPorts}}' 43777
```

auto generate using assigned ip
```
sudo docker run -d -p 198.51.100.73::80 yd (auto-generate)
sudo docker run -d -p 198.51.100.73:80:80 yd
```
if we add EXPOSE instruction in Dockerfile, we use
```
sudo docker run -d -P yd (auto binding ip)
```


#### cp7
- data volume
First add volume in dockerfile:
```
VOLUME /mountvolume
```
Build:
```
sudo docker build -t image-name
```
inspect the image:
```
sudo docker inspect image-name
```
run:
```
sudo docker run --rm -it image-name
mount
```
inspect the container:
```
sudo docker inspect 123456
```

Or, we can explicitly appoint mount point
```
sudo docker run –v /MountPoint -it ubuntu:14.04
```

Delete a mount volume of a container
```
sudo docker rm -fv f7777
```

create mounting point
```
sudo docker run -v /tmp/hostdir:/MountPoint -it ubuntu:14.04  //host(monutingpoint:container)
```
use data from other containers
```
sudo docker run -lt --name="container1" -v /mount imagename
```
another:
```
sudo docker run –it --volumes-from container1 imagename
```

Never use a data volume as storage during the build process. Ex
```
FROM ubuntu:14.04
VOLUME /MountPointDemo
RUN date > /MountPointDemo/date.txt
RUN cat /MountPointDemo/date.txt
```
This will render an error.


- cp9
```
sudo gpasswd -a jenkins docker
```


- cp10

show pid of a container
```
sudo docker inspect --format "{{ .State.Pid }}" 1234
```
And get pid of container.
```
ps -ef
```
And get environment
```
sudo cat -v /proc/2543/environ
sudo ls /proc/2543/ns
```
get all mount points of a container,log in to container,
```
cat /proc/mounts
```


show memory info of container
```
/sys/fs/cgroup/memory/docker/
```
