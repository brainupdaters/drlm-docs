Building GRUB2 for diferent platfoms
====================================

Since DRLM version 2, we moved to GRUB2 to provide the netboot images to start
ReaR recovery images from network. This movement was the first step to provide
support for mulitple platforms for GNU/Linux because GRUB2 supports multiple
architerctures.

At this time DRLM built packages include all documented platforms in this guide.

Prepare your build host
-----------------------

.. note::
  This document describes the process of building DRLM GRUB2 netboot images
  for diferent platforms with a debian machine. The process should be the
  same on other distros, just adjusting package dependecies for target distro
  and install them with the package management tools provided by each distro
  should work without problems.

**Install required packages**
::
  $ apt-get install bison libopts25 libselinux1-dev autogen \
  m4 autoconf help2man libopts25-dev flex libfont-freetype-perl \
  automake autotools-dev libfreetype6-dev texinfo

**Download GRUB2 sources**
::
  $ cd /tmp

  $ wget http://alpha.gnu.org/gnu/grub/grub-2.02~beta3.tar.gz

  $ tar -xzvf grub-2.02~beta3.tar.gz

  $ cd grub-2.02~beta3

Start build process
-------------------

.. warning::
  All documented grub2 image builds are included in drlm packages, this document
  will be a kind of guide for troubleshooting and testing on new GRUB2 versions
  and also a guide to, contributors of future drlm grub2 images, on new supported
  platforms to the project.

**Provide DRLM branded GRUB2 build**
::
  $ vi grub-core/normal/main.c

  .. replace:
  msg_formatted = grub_xasprintf (_("GNU GRUB  version %s"), PACKAGE_VERSION);

  .. with:
  msg_formatted = grub_xasprintf (_("DRLM Boot Manager (GNU GRUB2)"), PACKAGE_VERSION);

Prepare your build environment:

::
  $ ./autogen.sh

Proceed with configuration and build for each platform needed:

**For i386-pc:**
::
  $ ./configure --disable-werror
  $ make && make install

  $ /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/i386-pc --net-directory=/tmp
  Netboot directory for i386-pc created. Configure your DHCP server to point to /tmp/boot/grub/i386-pc/core.0


**For 32-bit EFI:**
::
  $ ./configure --with-platform=efi --target=i386 --disable-werror
  $ make && make install

  $ /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/i386-efi --net-directory=/tmp
  Netboot directory for i386-efi created. Configure your DHCP server to point to /tmp/boot/grub/i386-efi/core.efi


**For 64-bit (U)EFI:**
::
  $ ./configure --with-platform=efi --target=x86_64 --disable-werror
  $ make && make install

  $ /usr/local/bin/grub-mknetdir -d /usr/local/lib/grub/x86_64-efi --net-directory=/tmp
  Netboot directory for x86_64-efi created. Configure your DHCP server to point to /tmp/boot/grub/x86_64-efi/core.efi

**Create a tarball with tergeted platform netboot image**
::
  $ tar -cvzf drlm_grub2_<target>-<platform>.tar.gz

.. note::
  This gzipped tarball can be extracted to DRLM $STORDIR on your DRLM server, for
  testing purposes or to provide support to new platforms not yet provided by
  DRLM package builds.
