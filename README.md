# RPi Repo Manifests for the Yocto Project Build System

This repository provides Repo manifests to setup the Yocto build system for based on page https://jumpnowtek.com/rpi/Raspberry-Pi-Systems-with-Yocto.html

The Yocto Project allows the creation of custom linux distributions for embedded
systems.  It is a collection of git repositories known as *layers* each of which provides *recipes* to build
software packages as well as configuration information.

Repo is a tool that enables the management of many git repositories given a single *manifest* file.  
Tell repo to fetch a manifest from this repository and 
it will fetch the git repositories specified in the manifest and, by doing so,
setup a Yocto Project build environment for you!

Getting Started
---------------
**1.  Install Repo.**

##### Manually
Download the Repo script:

    $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > repo

Make it executable:

    $ chmod a+x repo

Move it on to your system path:

    $ sudo mv repo /usr/local/bin/

##### Debian / Ubuntu
Use apt manager
    
    $ apt install repo


If it is correctly installed, you should see a Usage message when invoked
with the help flag.

    $ repo --help

**2.  Initialize a Repo client.**

Tell Repo where to find the manifest:

    $ repo init -u https://github.com/tomaskrcka/repo-rpi-qt5.git -b <branch>

A successful initialization will end with a message stating that Repo is
initialized in your working directory. Your directory should now
contain a .repo directory where repo control files such as the manifest are
stored but you should not need to touch this directory.

***
**Note:**
You can use the **-b** switch to specify the branch of the repository
to use.

To test out the stable version, type:

    $ repo init -u https://github.com/tomaskrcka/repo-rpi-qt5.git -b warrior
    $ repo sync
    
To learn more about repo, look at [Repo Command Reference](https://source.android.com/source/using-repo "Using repo")
***

**3.  Fetch all the repositories:**

    $ repo sync

**4.  Initialize the Yocto Project Build Environment.**

    $ source poky-warrior/oe-init-build-env ~/rpi/build
    

This copies default configuration information into the **rpi/build/conf**
directory and sets up some environment variables for the build system.  This configuration
directory is not under revision control;

**5.  Build an image:**

This process downloads several gigabytes of source code and then proceeds to
do an awful lot of compilation so make sure you have plenty of space (25GB minimum), 
and expect a day or so of build time depending on your network
connection.  Don't worry---it is just the first build that takes a while.

    $ bitbake qt5-image

If everything goes well, you should have a compressed root filesystem tarball as well as kernel and bootloader binaries available in your
**rpi/build/tmp/deploy/images/raspberrypi3** directory.  
If you run into problems, the most likely candidate is missing software packages.  Check out
[The Build Host Packages](http://www.yoctoproject.org/docs/current/yocto-project-qs/yocto-project-qs.html#resources "Yocto quick start guide")
for the list of required packages for operating system. Also, take
a look to be sure your operating system is supported:
[Distribution Supported List](https://wiki.yoctoproject.org/wiki/Distribution_Support "Yocto wiki")

**6. Populate a SDK (optional)**

If you need a current SDK from the build you can use the following command.

    $  bitbake qt5-image -c populate_sdk