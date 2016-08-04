# Efreet

This is a collection (sparse so far) of Dockerfiles for building a containerized
[GENIE](http://genie.hepforge.org) that runs inside
[Docker](https://www.docker.com). It is, in some senses, putting the Genie
_back in_ the bottle.

Docker is very powerful and well-documented, but there is no need (hopefully) for
the casual GENIE enthusiast to read all of the documentaion before using `Efreet`
to run GENIE without having to build anything. This 'README' should be sufficient,
and if it isn't, open an issue, etc.

Note that the purpose of the Docker container is not to provide a fully-featured
VM to work inside of. Use it to create an easy way to run GENIE and export
new files created by the generator for analysis in your 'native' environment.

## Linux

Not tested! In principle, you just install `docker` and then you can `pull` the
`genie` images, etc. 

## OSX

If you haven't already, install [VirtualBox](https://www.virtualbox.org). Next, 
install [docker-machine](https://docs.docker.com/machine/). Then _source_
(do not execute) the script `start-docker-osx.sh` to start a `docker`
daemon. (Note: if you are a regular Docker user, feel free to start
`docker-machine` in your usual way. There is nothing magical about this script.)

You may use a Dockerfile to build GENIE (and, in fact, if you want to add new
applications, you should add them to the Dockerfile and build a new image), but
you may also pull an image from
[DockerHub](https://hub.docker.com/r/gnperdue/genie/).

    $ docker pull gnperdue/genie:2.10.10

It will take a few minutes to download the image. Once it has downloaded, run
it with:

    $ docker run -t -i gnperdue/genie:2.10.10 /bin/bash

This will provide a linux prompt. Go to 

    # cd /root/lamp/
    # . environment_setup.sh
    # gevgen -n 1 -p 14 -t 1000060120 \
      -e 1  -r 101 \
      --seed 2989819 --cross-sections /root/lamp/data/gxspl-small.xml \
      --message-thresholds Messenger.xml --event-generator-list CCQE

In the `/root/lamp/genie_runs` directory you can find scripts for running GENIE
(not required) and in `/root/lamp/data/gxspl-small.xml` you have a small, starter
cross section file.

If you run GENIE in this way, you will lose all of your changes and new files
when you `exit`. In order to persist your files, it is a good idea to run with
a local mount:

    $ docker run -t -i -v $PWD:/root/mygeniefiles gnperdue/genie:2.10.10 /bin/bash

Now `/root/mygeniefiles` inside the container will hold the contents of `$PWD` and
if you move a file you produce with GENIE to `/root/mygeniefiles` it will persist
after you exit Docker. (You can name the directory whatever you like.)

There is a nicer, newer way of doing this with Docker volumes that we haven't
researched yet.

When you are done, you need to stop the `docker-machine`. If you used the start-up
script included here, issue the command:

    $ docker-machine stop idreamofgenie

## Windows

Not tested! But this should, _in principle_ be the easiest way to run GENIE
"natively" on a Windows machine.

## Notes to self...

Example of how to update for a new version of GENIE:

    $ . start-docker-osx.sh
    // go to Dockerfile location, here for Ubuntu
    $ cd dockerfiles/ubuntu_14_04/
    // build with a tag (Dockerfile is local)
    $ docker build -t gnperdue/genie:2.10.8 .
    ...
    // see what we got
    $ docker images
    $ docker push gnperdue/genie:2.10.8
    // get rid of old "latest" and rebuild
    // use correct hash from `docker images`
    $ docker rmi -f 42a44
    $ docker build -t gnperdue/genie:latest .
    $ docker push gnperdue/genie:latest
