Introduction
============

This script is a tool to test a Gentoo ebuild and its
dependencies. The idea is that the package is emerged in a clean (and
current) stage3 Docker container.

.. image:: https://travis-ci.org/waebbl/ebuildtester.svg?branch=master
    :target: https://travis-ci.org/waebbl/ebuildtester


Requirements
------------

You require `Docker <https://wiki.gentoo.org/wiki/Docker>`_ and `FUSE
<https://wiki.gentoo.org/wiki/Filesystem_in_Userspace>`_. Docker must be
configured to use the `devicemapper
<https://docs.docker.com/storage/storagedriver/device-mapper-driver/>`_
storage driver.  This can be achieved with the following inside
``/etc/docker/daemon.json``:

.. code-block:: javascript

   {
     "storage-driver": "devicemapper"
   }

Usage
-----

We are going to assume that the user has a local git clone of the portage tree in

.. code-block:: bash

   /usr/local/git/gentoo

We have added a new ebuild and would like to verify that the build
dependencies are all correct. We can build the package (ATOM) with:

.. code-block:: bash

   ebuildtester --portage-dir /usr/local/git/gentoo \
     --atom ATOM \
     --use USE1 USE2

where we have specified two USE flags, USE1 and USE2. The
`ebuildtester` command will now create a docker container and start
installing the ATOM. All specified dependencies will be installed as
well.


Command line arguments
----------------------

The command understands the following command line arguments:

.. code-block:: bash

    usage: ebuildtester [-h] [--version] [--atom ATOM [ATOM ...]] [--live-ebuild]
                             [--manual] --portage-dir PORTAGE_DIR
                             [--overlay-dir OVERLAY_DIR] [--update {yes,true,no,false}]
                             [--threads N] [--use USE [USE ...]]
                             [--global-use GLOBAL_USE [GLOBAL_USE ...]] [--unmask ATOM]
                             [--unstable] [--gcc-version VER] [--rm] [--with-X]
                             [--with-vnc]
                             [--profile {default/linux/amd64/17.0,default/linux/amd64/17.0/systemd}]

    A dockerized approach to test a Gentoo package within a clean stage3.

    optional arguments:
      -h, --help            show this help message and exit
      --version             show program's version number and exit
      --atom ATOM [ATOM ...]
                            The package atom(s) to install
      --live-ebuild         Unmask the live ebuild of the atom
      --manual              Install package manually
      --portage-dir PORTAGE_DIR
                            The local portage directory
      --overlay-dir OVERLAY_DIR
                            Add overlay dir (can be used multiple times)
      --update {yes,true,no,false}
                            Update container before installing atom
      --threads N           Use N (default 8) threads to build packages
      --use USE [USE ...]   The use flags for the atom
      --global-use GLOBAL_USE [GLOBAL_USE ...]
                            Set global USE flag
      --unmask ATOM         Unmask atom (can be used multiple times)
      --unstable            Globally 'unstable' system, i.e. ~amd64
      --gcc-version VER     Use gcc version VER
      --rm                  Remove container after session is done
      --with-X              Globally enable the X USE flag
      --with-vnc            Install VNC server to test graphical applications
      --profile {default/linux/amd64/17.0,default/linux/amd64/17.0/systemd}
