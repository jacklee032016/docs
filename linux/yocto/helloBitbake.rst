
Hello World for bitbake
###################################


BitBake
======================
::

  git clone git://git.openembedded.org/bitbake
  bitbake -h
	

Define a project of bitbake
==============================

**BBPATH**
------------
root directory of bitbake project 

**conf/bitbake.conf**
---------------------------
Global variable for meta data and recipe files in this bb project
::

  PN  = "${@bb.parse.BBHandler.vars_from_file(d.getVar('FILE', False),d)[0] or 'defaultpkgname'}"
                
  TMPDIR  = "${TOPDIR}/tmp"
  CACHE   = "${TMPDIR}/cache"
  STAMP   = "${TMPDIR}/${PN}/stamps"
  T       = "${TMPDIR}/${PN}/work"
  B       = "${TMPDIR}/${PN}"

**conf/bblayers.conf**
--------------------------
Layers in this project
::

  BBLAYERS ?= " \
       /root/yocto/helloBitbake \
       "

minimally required class, **classes/base.bbclass**
---------------------------------------------------------
class file defines code and functions, base.bbclass is inherited by every class
::

 addtask build


Layers and Recipes
=======================
Layer directory and its configuration file
--------------------------------------------
 **myLayer/conf/layer.conf**
::

  BBPATH .= ":${LAYERDIR}"

  BBFILES += "${LAYERDIR}/*.bb"

  BBFILE_COLLECTIONS += "myLayer"
  BBFILE_PATTERN_mylayer := "^${LAYERDIR_RE}/"
 

recipe file in this layer
--------------------------------
 **myLayer/printHello.bb**
::

  DESCRIPTION = "Prints Hello World"
  PN = 'printhello'
  PV = '1'

  python do_build() {
     bb.plain("********************");
     bb.plain("*                  *");
     bb.plain("*  Hello, World!   *");
     bb.plain("*                  *");
     bb.plain("********************");
  }
          


Execute
===================
::

   bitbake printhello
   
   bitbake -c clean printhello: remove cache
   
