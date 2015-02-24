## An easier way to use the shared folder

The Boot2Docker automatically mounts the `Users` directory of your **host**
  machine onto the Boot2Docker VM. 

If you are using Windows, at the Boot2Docker prompt, do

```console
$ ls /c/Users
```

or if you are using a Mac, do

```console
$ ls /Users
```

You should see your users directories (your username) of the *host* machine. Make a sub-directory:

```console
$ cd /c/Users/Default
$ mkdir info490
```

(or `/Users/<username>` for Mac; obviously you should replace `Default`
  in the above command with your username)

Back up important files in the notebook server container that is currently
  running. Stop and remove the container using `docker stop` and `docker rm`.
  Now, use the `-v` option to mount this volume onto the IPython notebook
  server container:

```console
$ docker run -d -p 8888:8888 -e "PASSWORD=YourPassword" -v /c/Users/Default/info490:/notebooks/data lcdm/info490
```

Now you should be able to share files between the `c:\Users\Default\info490`
  in Windows with the `/notebooks/data` directory
  in your notebook server container.
  Open a web browser in Windows/Mac, access the notebook server,
  and check if a sub-directory named `data` exists.
  Also, go to the corresponding directory in Windows/Mac and
  check if there is a directory named `info490`.
  Try transferring files via the shared folder.
