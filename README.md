#Broken:
This is not expected to work with any reasonably recent version of cjdns
because there is no automated testing of it. If you would like to help develop
an automated test kit, please stop by the IRC channel `#cjdns` on efnet.

This is an OpenWRT feed for the [cjdns] project.

To install cjdns on OpenWRT:

    # cd ~
    # svn co svn://svn.openwrt.org/openwrt/trunk/ openwrt
    # cd openwrt
    # cp ./feeds.conf.default ./feeds.conf
    # echo 'src-git cjdns git://github.com/cjdelisle/cjdns-openwrt.git' >> ./feeds.conf
    # ./scripts/feeds update -a
    # ./scripts/feeds install cjdns

Then configure for your system:

    # make menuconfig

Select your system type and the options you want and choose:

    Network ---> Routing and Redirection ---> [*] cjdns

Then save and close the configuration menu, then allow OpenWRT to resolve dependencies:

    # make defconfig

Then build:

    # make

If you have a multicore processor, you can build faster using `-j`,
however the OpenWRT build process is not highly parallelized so your milage may vary.

    # make -j 4

To update the version of cjdns:

    # rm ./dl/cjdns-*
    # ./scripts/feeds update cjdns
    # make
