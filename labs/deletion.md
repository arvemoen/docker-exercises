# Throw your container away

As containers are just a thin base layer on top of the host kernel, it is really fast to spin up a new instance if you crashed your old one.

Let's try to run an alpine container and delete the file system.

Spin up the container with `docker run -ti alpine`

list all the folders on the root level to see the whole distribution:
```
# ls /

bin    etc    lib    mnt    root   sbin   sys    usr
dev    home   media  proc   run    srv    tmp    var
```

Now, delete the whole file system with `rm -rf /`

> **Warning:** Do never try to run this command as a super user in your own environment, as it will delete *everything* on your computer.

Try to navigate around to see how much of the OS is gone

```
# ls
/bin/sh: ls: not found

# whoami
sh: whoami: not found

# date
/bin/sh: date: not found
```

Exit out by `Ctrl+D` and create a new instance of the Alpine image and look a bit around:

```
$ docker run -ti alpine
# ls /
bin    etc    lib    mnt    root   sbin   sys    usr
dev    home   media  proc   run    srv    tmp    var
```

Try to perform the same tasks as displayed above to see that you have a fresh new instance ready to go.

### Cleaning up containers you do not use anymore

Containers are still persisted, even though they are stopped.
If you want to delete them from your server you need to use the `docker rm` command.
`docker rm` can take either the `CONTAINER ID` or `NAME` as seen above. Try to remove the `hello-world` container:
```
sofus@Praq-Sof:~/git/docker-exercises$ docker ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS                      PORTS                                                          NAMES
6a9246ff53cb        hello-world               "/hello"                 18 seconds ago      Exited (0) 16 seconds ago                                                                  ecstatic_cray

sofus@Praq-Sof:~/git/docker-exercises$ docker rm ecstatic_cray
ecstatic_cray
```

The container is now gone when you execute a `ps -a` command.

Tip: As with Git, you can use any unique part of the container ID to refer to it.

### Deleting images
You deleted the container instance above, but not the image of hello-world itself. And as you are now on the verge to become a docker expert, you do not need the hello-world image anymore so let us delete it.

First off, list all the images you have downloaded to your computer:

```
sofus@praq-sal:~$ docker images
REPOSITORY                              TAG                   IMAGE ID            CREATED             SIZE
alpine                                  latest                053cde6e8953        9 days ago          3.97MB
hello-world                             latest                48b5124b2768        10 months ago       1.84kB
```

Here you can see the images downloaded as well as their size.
To remove the hello-world image use the `docker rmi` command together with the id of the docker image.

```
sofus@praq-sal:~$ docker rmi 48b5124b2768
Untagged: hello-world:latest
Untagged: hello-world@sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Deleted: sha256:48b5124b2768d2b917edcb640435044a97967015485e812545546cbed5cf0233
Deleted: sha256:98c944e98de8d35097100ff70a31083ec57704be0991a92c51700465e4544d08
```

What docker did here was to `untag` the image removing the references to the sha of the image. After the image has no references, it deletes the two layers the image itself is comprised of.

**Summary**

You have now seen the swiftness of creating a new container from an image, trash it, and create a new one on top of it.
You have learned to use `rm` for deleting containers, `rmi` for images, `images` for listing the images and `ps -a` to look at all the containers on your host. 