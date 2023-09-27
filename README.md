# jobd

## Cppcheck bug

This commit is to show a cppcheck bug that sometimes happens during a parallel cmake build with an addon.


I was able to reproduce the bug on Linux with cppcheck 2.11.1 runing the following commands:
```
mkdir build
cd build
cmake ..
make -j
```

Occasionally cppcheck output will be interrupted with this internal error:
```
:0:0: error: Bailing out from checking since there was an internal error: Internal error during whole program analysis: Failed to execute 'python3 /home/amacgregor/Projects/andy_tools/cppcheck/build_211/bin/addons/runaddon.py /home/amacgregor/Projects/andy_tools/cppcheck/build_211/bin/addons/misra.py --cli --file-list /home/amacgregor/Projects/andy_tools/jobd/cppcheck-addon-ctu-file-list'. Traceback (most recent call last): [internalError]
```

I think it is because there are multiple processes running that create and drestroy the cppcheck-addon-ctu-file-list file
which isn't appended with a pid.

This project is an arbitrary choice because its a multifile C project that isn't misra-compliant
with CMake as the build system and minimal dependencies.

## IMPORTANT NOTICE

Development of this project has been paused indefinitely.
Current efforts are focused on reviving and improving the
original [relaunchd](https://github.com/mheily/relaunchd) project
instead.

## Overview 

jobd is an init system. It is currently under heavy development, and should not be used for anything important. Be especially mindful that there is NO WARRANTY provided with this software.  

## Status

jobd runs on the following platforms:
* FreeBSD
* Linux

See the [release notes](./CHANGELOG.md) for details about
the current release.

## Building and installation

The basic commands to build and install the software are:

        mkdir build
        cd build
        cmake ..
        make
        sudo make install

<!--
## Building under Linux

You can check the current build status by visiting the 
[Travis CI dashboard](https://travis-ci.org/mheily/jobd/builds)

There are a few extra steps when building on Linux:

1. Install mandoc to generate HTML from manpages. Example:

	sudo apt-get install mandoc

## Building under OpenBSD

You will need to build libucl, which means installing GNU Autotools:
```
# pkg_add autoconf-2.69p1 automake-1.15 libtool
```

Run the configure script:
```
./configure
```

Since libucl will try to run autoconf/automake, you will need to provide
the environment variables to make(1):
```
AUTOCONF_VERSION=2.69 AUTOMAKE_VERSION=1.15 make
```


## Building under NetBSD

You will need to build libucl, which means installing GNU Autotools:
```
# pkg_add autoconf automake libtool pkg-config
```

## Building under MacOS

You will need the Homebrew versions of a number of autotools utilities.

Run this:
```
$ brew install autoconf automake libtool shtool pkgconfig
```

## Socket activation

jobd uses a different mechanism for socket activation than the one that
Darwin uses.

TODO -- document this

## Usage

To start launchd, run the following command as root:

	service launchd start

If you want to run jobs in your graphical user session, add the following lines to
your session startup file:

	launchctl load ~/.launchd/agents /usr/local/etc/launchd/agents /usr/local/share/launchd/agents   


## Static Analysis 

Coverity scan reports for jobd are available at:
https://scan.coverity.com/projects/mheily-jobd?tab=overview

When new releases are created, they will be submitted to Coverity
to re-run the static analyzer.

-->

## Testing

There are two main test scripts:

* test/run.sh
* test/pid1.sh

The first script, test/run.sh, will build a copy of the software and run some tests. It is self-contained within
 `test/obj` and will not interfere with any other installation of the software.
 
The second test, test/pid1.sh, runs a FreeBSD image under Vagrant and configures the box to run jobd
as pid #1.

## Contact Information

There is a [mailing list](https://groups.google.com/forum/#!forum/jobd-devel) for questions, comments, or other feedback about the project.
   
<!--
There is also a #jobd IRC channel on FreeNode.
-->

## See also

For an introduction to jobd, please read the [jobd handbook](http://mheily.github.io/jobd/).

There is a [slide deck](https://docs.google.com/presentation/d/1QY1p8H-tmWw4h5mL63nboFefuMxmDcOPMeyqJJqqF4M/edit?usp=sharing) that was presented at the
2016 FreeBSD developer summit at BSDcan.
