About ZooDind
=============

ZooDinD means Zoobab's Docker-in-Docker.

It is based on Alpine Linux, Docker and/or SSH.

It is useful to debug different versions of the Docker Daemon.

Usage
=====

## Usage with Docker exec

You can also launch it without SSH:

```
$ docker run --privileged --name zoodind zoobab/zoodind
time="2017-08-17T13:47:39.197079256Z" level=info msg="libcontainerd: new containerd process, pid: 18"
time="2017-08-17T13:47:40.340923831Z" level=info msg="Graph migration to content-addressability took 0.00 seconds"
time="2017-08-17T13:47:40.341737960Z" level=warning msg="Your kernel does not support swap memory limit"
time="2017-08-17T13:47:40.341914513Z" level=warning msg="Your kernel does not support cgroup rt period"
time="2017-08-17T13:47:40.341971179Z" level=warning msg="Your kernel does not support cgroup rt runtime"
time="2017-08-17T13:47:40.343235779Z" level=info msg="Loading containers: start."
time="2017-08-17T13:47:40.346638308Z" level=warning msg="Running modprobe bridge br_netfilter failed with message: modprobe: can't change directory to '/lib/modules': No such file or directory\n, error: exit status 1"
time="2017-08-17T13:47:40.349276885Z" level=warning msg="Running modprobe nf_nat failed with message: `modprobe: can't change directory to '/lib/modules': No such file or directory`, error: exit status 1"
time="2017-08-17T13:47:40.351867838Z" level=warning msg="Running modprobe xt_conntrack failed with message: `modprobe: can't change directory to '/lib/modules': No such file or directory`, error: exit status 1"
time="2017-08-17T13:47:40.530985786Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.18.0.0/16. Daemon option --bip can be used to set a preferred IP address"
time="2017-08-17T13:47:40.634905127Z" level=info msg="Loading containers: done."
time="2017-08-17T13:47:40.686731734Z" level=info msg="Daemon has completed initialization"
time="2017-08-17T13:47:40.686820051Z" level=info msg="Docker daemon" commit=v17.05.0-ce graphdriver=overlay2 version=17.05.0-ce
time="2017-08-17T13:47:40.704376372Z" level=info msg="API listen on /var/run/docker.sock"
```

If you want to run a specific version, ie "1.12.6-alpine", or "17.05.0-ce-alpine", or "1.11.2-alpine":

```
$ docker run --privileged --name zoodind zoobab/zoodind:1.12.6-alpine
time="2017-08-18T08:04:18.591313245Z" level=info msg="libcontainerd: new containerd process, pid: 17"
time="2017-08-18T08:04:19.611850956Z" level=error msg="Failed to built-in GetDriver graph aufs /var/lib/docker"
time="2017-08-18T08:04:19.612371623Z" level=error msg="Failed to built-in GetDriver graph devicemapper /var/lib/docker"
time="2017-08-18T08:04:20.191656871Z" level=info msg="Graph migration to content-addressability took 0.00 seconds"
time="2017-08-18T08:04:20.192125880Z" level=warning msg="Your kernel does not support swap memory limit."
time="2017-08-18T08:04:20.193349882Z" level=info msg="Loading containers: start."
time="2017-08-18T08:04:20.197104561Z" level=warning msg="Running modprobe bridge br_netfilter failed with message: modprobe: can't change directory to '/lib/modules': No such file or directory\n, error: exit status 1"
time="2017-08-18T08:04:20.198875736Z" level=warning msg="Running modprobe nf_nat failed with message: `modprobe: can't change directory to '/lib/modules': No such file or directory`, error: exit status 1"
time="2017-08-18T08:04:20.201023739Z" level=warning msg="Running modprobe xt_conntrack failed with message: `modprobe: can't change directory to '/lib/modules': No such file or directory`, error: exit status 1"
time="2017-08-18T08:04:20.334563554Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.18.0.0/16. Daemon option --bip can be used to set a preferred IP address"

time="2017-08-18T08:04:20.411129641Z" level=info msg="Loading containers: done."
time="2017-08-18T08:04:20.411834614Z" level=info msg="Daemon has completed initialization"
time="2017-08-18T08:04:20.411937423Z" level=info msg="Docker daemon" commit=v1.12.6 graphdriver=overlay version=1.12.6
time="2017-08-18T08:04:20.420797765Z" level=info msg="API listen on /var/run/docker.sock"
```

Then use docker exec:

```
$ docker exec zoodind docker version
Client:
 Version:      17.05.0-ce
 API version:  1.29
 Go version:   go1.8.1
 Git commit:   v17.05.0-ce
 Built:        Tue May 16 10:10:15 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.05.0-ce
 API version:  1.29 (minimum version 1.12)
 Go version:   go1.8.1
 Git commit:   v17.05.0-ce
 Built:        Tue May 16 10:10:15 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

## Usage with SSH

Some basic commands on how to use it:

```
$ cd 17.05.0-ce-alpine
$ ./run.sh
7205cedb9aa6cd2d72410cf08f93035b895e60b063886d4baad3c12887ce7c60
$ ./testssh.sh
OK, SSH+Docker test succeeded
$ ssh -q -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i id_rsa root@127.0.0.1 -p1705 docker version
Client:
 Version:      17.05.0-ce
 API version:  1.29
 Go version:   go1.8.1
 Git commit:   v17.05.0-ce
 Built:        Tue May 16 10:10:15 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.05.0-ce
 API version:  1.29 (minimum version 1.12)
 Go version:   go1.8.1
 Git commit:   v17.05.0-ce
 Built:        Tue May 16 10:10:15 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

Todo
====

* ~~add more docker versions: 1.12.6, 1.11.2~~
* add ubuntu based images
* ~~ssh dotconfig~~
* use a skel directory to make templates
* commit suicide if the docker daemon is not running after a while
* be able to run as a oneliner: $ docker run -it zoodind:1.12.6 docker run -it alpine /bin/sh

Problems
========

* Docker Inc has removed all debs before v17.*: https://twitter.com/zoobab/status/898499634674507776
