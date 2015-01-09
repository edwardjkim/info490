## Folder Sharing

If you are using Boot2Docker on Windows or Mac OS X, you will need to set up
folder sharing to share disk space between your local desktop and a Docker
container is by creating a volume container and then sharing that to where it's
needed. You may postpone this until the end of the first week when you will be
more familiar with the Unix CLI, but keep in mind that you will need to use
this feature at some point.

First, make a volume container by typing (you only need to do this once)

```console
$ docker run -v /data --name my-data busybox true
```

On Windows, type

```console
$ docker run --rm -v /usr/local/bin/docker:/docker -v
/var/run/docker.sock:/docker.sock svendowideit/samba my-data
```

We will connect to the Docker container using the IP address of your
Boot2Docker host, which is 192.168.59.103 by default. On Windows, use Explorer
to connect to

    \\192.168.59.103\data (or _data)

![connect with explorer](explorer2.png)

On Mac OS X, you can find out the IP address of your Boot2Docker host with

```console
$ boot2docker ip
192.168.59.103
```

and connect to the shared folder using Finder (OS X):

    Connect to cifs://192.168.59.103/data

Once mounted, it will appear as "/Volumes/data".

You can then use your data container from any container you like. For
interactive mode, you would type

```console
$ docker run -it --volumes-from my-data info490/base /bin/bash
```

You can make sure that the data container is mounted correctly by typing the following in your container:

```console
root@58c532684e57:/# cd /data
root@58c532684e57:/# touch hello.test
```

and a file named "hello.test" should appear in the "data" folder of your Explorer window.

And to IPython notebook server with the data container, type

```console
$ docker run -d -p 8888:8888 -e "PASSWORD=YourPassword" --volumes-from my-data info490/base
```

You will find the "data" volume mounted as "/data" in that container. Note that "my-data" is the name of volume container, this is shared via the "network" by the "samba" container that refers to it by name. So, in this example, if you were on OS-X you now have /Volumes/data and /data in container being shared. You can change the paths as needed.

### Reference

- [Boot2Docker documentation](https://github.com/boot2docker/boot2docker).
