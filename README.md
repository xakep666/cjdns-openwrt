This is an OpenWRT feed for the [cjdns] project.

## What is OpenWRT?

OpenWRT is a replacement firmware for home and small office routers that gives great flexibility. It is based on a linux kernel and allows for outside software such as cjdns to run and modify the network stack. We will be using it as a base station to anchor the mesh network we're creating.

## What is batman-adv?

Batman-adv is a wireless mesh network protocol that forms a [layer 2](http://en.wikipedia.org/wiki/OSI_model) network on top of normal 802.11 wifi. Essentially it makes it appear as if an entire wireless mesh is on the same logical network, even if two particular radios could not have reached each other on their own (ie - they require the nodes between them to relay the connection in order to communicate). Batman-adv and cjdns complement each other well, as batman-adv provides the illusion of a physical link between far-flung wireless nodes, and cjdns creates the logical layer above that illusion. This tutorial will include information on including batman-adv, but it is optional. cjdns will work without it.

## Configuring and Building OpenWRT

Before beginning, you need to obtain a clean version of the OpenWRT source code on the computer you will be compiling on.

    $ git clone git://git.openwrt.org/openwrt.git

OpenWRT has its own package management system within the build tools. Before you build the openwrt firmware you're going to be using, we are going to use this package system to add batman-adv and cjdns to the firmware build process. In the directory where you cloned openwrt there should be a document called "feeds.conf.default." We want to include the default feed lists, so do:

    $ cp feeds.conf.default feeds.conf

Then we want to edit feeds.conf:

    $ vim feeds.conf

And add the following line to the file:

    src-git cjdns git://github.com/cjdelisle/cjdns-openwrt.git

Next use that package system to install cjdns and (optionally) batman-adv:

    $ ./scripts/feeds update -a
    $ ./scripts/feeds install cjdns
    $ ./scripts/feeds install kmod-batman-adv (if you wish to use batman-adv)

Then configure for your system:

    # make menuconfig

Select your system type and the options you want and choose:

    Network ---> Routing and Redirection ---> [*] cjdns

If you have batman-adv included as well:
    
    Kernel Modules ---> Network Support ---> kmod-batman-adv

Then save and close the configuration menu, then allow OpenWRT to resolve dependencies:

    # make defconfig

Then build:

    # make

If you have a multicore processor, you can build faster using `-j`,
however the OpenWRT build process is not highly parallelized so your milage may vary.

    # make -j 4

Upload the compiled firmware (which will be in the bin/ directory) to your desired router through any of the [avilable methods](http://wiki.openwrt.org/doc/howto/generic.flashing).

If you included batman-adv you will want to configure it as explained in [their wiki](http://www.open-mesh.org/projects/batman-adv/wiki/Building-with-openwrt).

To update the version of cjdns:

    # rm ./dl/cjdns-*
    # ./scripts/feeds update cjdns
    # make
