#### pkleadocker


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
By default, the tag name is empty,
Add a tag:
```
sudo docker tag 1234 imagename:tagname
```
or define it when building (syntax: imagename:tagname, by default, tagname=latest)
```
sudo docker build -t imagename .
```
