Building GRUB2 for diferent platfoms
====================================

Since DRLM version 2, we moved to GRUB2 to provide the netboot for 
ReaR recovery images. This was the first step to support mulitple 
platforms for GNU/Linux as GRUB2 supports multiple architerctures.

At this time DRLM officially supports the following platforms:

  - i386-pc
  - i386-efi
  - x86_64-efi
  - powerpc-ieee1275

Prepare your build host
-----------------------

.. note::
  This document describes the process of building DRLM GRUB2 netboot images
  for diferent platforms with a debian machine and SLES12 for PowerPC. The process should be the
  same on other distros, just adjusting the correct package dependecies for target distro
  and install them with the package management tools provided should work without problems.

Install required packages (Debian)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# apt-get install bison libopts25 libselinux1-dev autogen \
  m4 autoconf help2man libopts25-dev flex libfont-freetype-perl \
  automake autotools-dev libfreetype6-dev texinfo autopoint \
  pkg-config unifont liblzma-dev make

Install required packages (SLES12 PowerPC)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# zypper install automake bison device-mapper-devel fdupes flex \
  freetype2-devel fuse-devel dejavu-fonts gnu-unifont help2man xz \
  makeinfo xz-devel python openssl texinfo gettext-tools
  

Download GRUB2 sources
~~~~~~~~~~~~~~~~~~~~~~

::

  ~$ cd /usr/src

  ~# wget http://alpha.gnu.org/gnu/grub/grub-2.04~rc1.tar.gz

  ~# tar -xzvf grub-2.04~rc1.tar.gz

  ~$ cd grub-2.04~rc1

Start build process
-------------------

.. warning::
  All documented grub2 image builds are included in drlm packages, this document
  will be a kind of guide for troubleshooting and testing on new GRUB2 versions
  and also a guide to, contributors of future drlm grub2 images, on new supported
  platforms to the project.

Provide DRLM branded GRUB2 build
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# vi grub-core/normal/main.c

  .. replace:
  msg_formatted = grub_xasprintf (_("GNU GRUB  version %s"), PACKAGE_VERSION);

  .. with:
  msg_formatted = grub_xasprintf (_("DRLM Boot Manager (GNU GRUB2)"), PACKAGE_VERSION);


Prepare your build environment:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# ./autogen.sh


On next steps we will proceed with configuration and build for each platform needed.

For i386-pc:
~~~~~~~~~~~~

::

  ~# ./configure --disable-werror --disable-dependency-tracking
  *******************************************************
  GRUB2 will be compiled with following components:
  Platform: i386-pc
  With devmapper support: No (need libdevmapper header)
  With memory debugging: No
  With disk cache statistics: No
  With boot time statistics: No
  efiemu runtime: Yes
  grub-mkfont: Yes
  grub-mount: No (need FUSE library)
  starfield theme: No (No DejaVu found)
  With libzfs support: No (need zfs library)
  Build-time grub-mkfont: Yes
  With unifont from /usr/share/fonts/X11/misc/unifont.pcf.gz
  With liblzma from -llzma (support for XZ-compressed mips images)
  *******************************************************
  ~# make && make install

  ~# /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/i386-pc --net-directory=/tmp
  Netboot directory for i386-pc created. Configure your DHCP server to point to /tmp/boot/grub/i386-pc/core.0


For 32-bit EFI:
~~~~~~~~~~~~~~~

::

  ~# ./configure --with-platform=efi --target=i386 --disable-werror
  *******************************************************
  GRUB2 will be compiled with following components:
  Platform: i386-efi
  With devmapper support: No (need libdevmapper header)
  With memory debugging: No
  With disk cache statistics: No
  With boot time statistics: No
  efiemu runtime: No (not available on efi)
  grub-mkfont: Yes
  grub-mount: No (need FUSE library)
  starfield theme: No (No DejaVu found)
  With libzfs support: No (need zfs library)
  Build-time grub-mkfont: Yes
  With unifont from /usr/share/fonts/X11/misc/unifont.pcf.gz
  With liblzma from -llzma (support for XZ-compressed mips images)
  *******************************************************
  ~# make && make install

  ~# /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/i386-efi --net-directory=/tmp
  Netboot directory for i386-efi created. Configure your DHCP server to point to /tmp/boot/grub/i386-efi/core.efi


For 64-bit (U)EFI:
~~~~~~~~~~~~~~~~~~

::

  ~# ./configure --with-platform=efi --target=x86_64 --disable-werror
  *******************************************************
  GRUB2 will be compiled with following components:
  Platform: x86_64-efi
  With devmapper support: No (need libdevmapper header)
  With memory debugging: No
  With disk cache statistics: No
  With boot time statistics: No
  efiemu runtime: No (not available on efi)
  grub-mkfont: Yes
  grub-mount: No (need FUSE library)
  starfield theme: No (No DejaVu found)
  With libzfs support: No (need zfs library)
  Build-time grub-mkfont: Yes
  With unifont from /usr/share/fonts/X11/misc/unifont.pcf.gz
  With liblzma from -llzma (support for XZ-compressed mips images)
  *******************************************************
  ~# make && make install

  ~# /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/x86_64-efi --net-directory=/tmp
  Netboot directory for x86_64-efi created. Configure your DHCP server to point to /tmp/boot/grub/x86_64-efi/core.efi


For ieee1275 (PowerPC):
~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# ./configure --with-platform=ieee1275 --target=ppc64le --disable-werror
  ~# make && make install

  ~# /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/powerpc-ieee1275 --net-directory=/tmp
  Netboot directory for powerpc-ieee1275 created. Configure your DHCP server to point to /tmp/boot/grub/powerpc-ieee1275/core.elf


Create a tarball with targeted platform netboot image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~$ cd /tmp
 
  ~# tar -cvzf drlm_grub2_<target>-<platform>.tar.gz boot/

.. note::
  This gzipped tarball can be extracted to DRLM $STORDIR on your DRLM server, for
  testing purposes or to provide support to new platforms not yet provided by
  DRLM package builds.
