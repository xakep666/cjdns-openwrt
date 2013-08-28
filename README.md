This is an OpenWRT feed for the [cjdns] project.

## What is OpenWRT?

OpenWRT is a replacement firmware for home and small office routers that gives great flexibility. It is based on a linux kernel and allows for outside software such as cjdns to run and modify the network stack. We will be using it as a base station to anchor the mesh network we're creating.

## Configuring and Building OpenWRT

Before beginning, you need to obtain a clean version of the OpenWRT source code on the computer you will be compiling on.

    $ git clone git://git.openwrt.org/openwrt.git

OpenWRT has its own package management system within the build tools. Before you build the openwrt firmware you're going to be using, we are going to use this package system to add cjdns to the firmware build process. In the directory where you cloned openwrt there should be a document called "feeds.conf.default." We want to include the default feed lists, so do:

    $ cp feeds.conf.default feeds.conf

Then we want to edit feeds.conf:

    $ vim feeds.conf

And add the following line to the file:

    src-git cjdns git://github.com/cjdelisle/cjdns-openwrt.git

Next use that package system to incorporate the cjdns source:

    $ ./scripts/feeds update -a
    $ ./scripts/feeds install cjdns

Then configure for your system:

    # make menuconfig

Select your system type and the options you want and turn on the following option:

    Network ---> Routing and Redirection ---> [*] cjdns

Then save and close the configuration menu, and allow OpenWRT to resolve dependencies:

    # make defconfig

And finally build:

    # make

If you have a multicore processor, you can build faster using `-j`,
however the OpenWRT build process is not highly parallelized so your milage may vary.

    # make -j 4

Upload the compiled firmware (which will be in the bin/ directory) to your desired router through any of the [avilable methods](http://wiki.openwrt.org/doc/howto/generic.flashing).

To update the version of cjdns:

    # rm ./dl/cjdns-*
    # ./scripts/feeds update cjdns
    # make