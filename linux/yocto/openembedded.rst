OpenEmbedded Usage
#############################

Preparation
==========================
Install and update softwares in Fedora
-------------------------------------------
::

 sudo dnf install gawk make wget tar bzip2 gzip python3 unzip perl patch \
    diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath \
    ccache perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue perl-bignum socat \
    python3-pexpect findutils which file cpio python python3-pip xz


Download bitbake and openembedded-core
-------------------------------------------
::

  git clone git://git.openembedded.org/bitbake
  git clone git://github.com/openembedded/openembedded-core


Run openembedded
---------------------------
::

 BITBAKEDIR=/opt/bitbake source openembedded-core-master/oe-init-build-env 


Build Image
===========================

Configuration
----------------------
It create **`build`** directory and its subdirectories, copy some default configuration and layer;

Only one layer is defined in **$(OPENEMBEDDED)/meta**;

Working directory is build/tmp-glibc

Modify conf/local.conf (this file is a copy of **$(OPENEMBEDDED)conf/local.conf.sample**

::

 Common targets are:
    core-image-minimal
    core-image-sato
    meta-toolchain
    meta-ide-support


Build
----------------
::

 bitbake core-image-minimal

 
 Parsing recipes: 100% |###############################################################################################################| Time: 0:03:26
 Parsing of 789 .bb files complete (0 cached, 789 parsed). 1259 targets, 66 skipped, 0 masked, 0 errors.
 NOTE: Resolving any missing task queue dependencies

 Build Configuration:
 BB_VERSION           = "1.39.1"
 BUILD_SYS            = "i686-linux"
 NATIVELSBSTRING      = "fedora-25"
 TARGET_SYS           = "i586-oe-linux"
 MACHINE              = "qemux86"
 DISTRO               = "nodistro"
 DISTRO_VERSION       = "nodistro.0"
 TUNE_FEATURES        = "m32 i586"
 TARGET_FPU           = ""
 meta                 = "<unknown>:<unknown>"

 Initialising tasks: 100% |############################################################################################################| Time: 0:00:06
 Sstate summary: Wanted 470 Found 0 Missed 470 Current 0 (0% match, 0% complete)
 NOTE: Executing SetScene Tasks
 NOTE: Executing RunQueue Tasks

 Initialising tasks: 100% |############################################################################################################| Time: 0:00:06
 Sstate summary: Wanted 470 Found 0 Missed 470 Current 0 (0% match, 0% complete)
 NOTE: Tasks Summary: Attempted 1631 tasks of which 5 didn't need to be rerun and all succeeded.


Check build result:
---------------------------------------
  cache:
  downloads:
  sstate-cache:
  tmp-glibc: 
     buildstats:
     cache:
     deploy:
     hosttools:
     log:
     pkgdata:
     sstate-control:
     stamps:
     sysroots:
     sysroots-compoments:
     work:
     work-shared:
  


Run without environment of build
==================================

**PATH**: runqemu with find other scripts such as runqemu-if-up, etc.
::

  export PATH=$OEROOT/scripts

**Create TAP NIC**:
Create TAP network device for non-root user,

Run runqemu-gen-tapdevs to manually create.
::

  sudo runqemu-gen-tapdevs 1000 1000 4 tmp-glibc/sysroots-components/i686/qemu-helper-native/usr/bin
  
  Where 1000/1000 is user/group Id, 4: number of tap devices; 
  `tunctl` is in that path;
  /dev/net/dev must be usable by user 'lzj';

  
**Run kernel and root filesystem with qemu**

::

../openembedded-core-master/scripts/runqemu \
  tmp-glibc/deploy/images/qemux86/bzImage \
	tmp-glibc/deploy/images/qemux86/core-image-minimal-qemux86-20180909104321.rootfs.ext4 

  runqemu - INFO - Running \
    /home/lzj/oe/build/tmp-glibc/work/i686-linux/qemu-helper-native/1.0-r1/recipe-sysroot-native/usr/bin/qemu-system-i386  \
      -drive file=/home/lzj/oe/build/tmp-glibc/deploy/images/qemux86/core-image-minimal-qemux86-20180909104321.rootfs.ext4,if=virtio,format=raw -vga vmware -show-cursor -usb -device usb-tablet -device virtio-rng-pci   -cpu pentium2 -m 256 -serial mon:vc -serial null -kernel /home/lzj/oe/build/tmp-glibc/deploy/images/qemux86/bzImage -append 'root=/dev/vda rw highres=off  mem=256M vga=0 uvesafb.mode_option=640x480-32 oprofile.timer=1 uvesafb.task_timeout=-1 '

