## Folder Sharing

Boot2Docker is essentially a remote Docker engine with a read only filesystem (other than Docker images, containers and volumes). The most scalable and portable way to share disk space between your local desktop and a Docker container is by creating a volume container and then sharing that to where it's needed.  

One well tested approach is to use a file sharing container like `svendowideit/samba`:

```console
# Make a volume container (only need to do this once)
$ docker run -v /data --name my-data busybox true
# Share it using Samba (Windows file sharing)
$ docker run --rm -v /usr/local/bin/docker:/docker -v
/var/run/docker.sock:/docker.sock svendowideit/samba my-data
# Next line only for Mac OS X, find out the IP address of your Boot2Docker host
$ boot2docker ip
192.168.59.103
```

Connect to the shared folder using Finder (OS X):

    Connect to cifs://192.168.59.103/data

Once mounted, it will appear as "/Volumes/data".

Or on Windows, use Explorer to Connect to:

    \\192.168.59.103\data

![connect with explorer](explorer2.png)

You can then use your data container from any container you like:

```console
$ docker run -it --volumes-from my-data info490
```

or in our case,

```console
$ docker run -d -p 8888:8888 -e "PASSWORD=YourPassword" --volumes-from my-data info490/base
```

You will find the "data" volume mounted as "/data" in that container. Note that "my-data" is the name of volume container, this is shared via the "network" by the "samba" container that refers to it by name. So, in this example, if you were on OS-X you now have /Volumes/data and /data in container being shared. You can change the paths as needed.

### Reference

- [Boot2Docker documentation](https://github.com/boot2docker/boot2docker).
