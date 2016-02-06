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
CMD-> run when new container starts  
RUN->run at build time  
ENTRYPOINT->assign commands. can be overriden
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

