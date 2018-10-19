
FAQ in Fedora
########################
09.09,2018

Fedora 64bit Configuration
=============================

Enlarge size of root file system of Fedora
--------------------------------------------

Enlarge size of Virtual HD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

  VBoxManager modifyhd MyLinux.vdi --resize 153600 


Enlarge parition of root file system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Create virtual machine booting with gparted-live-0.32.0-1-i686.iso;
* Resize partition in virtual machine;


Logical Volume for root file system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Enter into Fedora, change logical volume dynamically

::

   pvresize /dev/sda2
      resumse physical volume

   lvextend --extents +100%Free /dev/fedora/root 
      Add space to logical volume
        /dev/fedora/root is symbol link of /dev/dm-0
        File system mount as /dev/mapper/fedora-root, which is also symbol link of ./dev/dm-0

   resize2fs /dev/fedora/root



GNOME Shell extension
-----------------------------

::

  dnf install gnome-tweak-tool

	dnf search gnome | grep shell

	dnf install gnome-shell-extension-openweather.noarch
	




Enable sudoers for user 'lzj'
-------------------------------
Command line:
::

  visudo  or vi /etc/sudoers
  
Add one line: allow user in group 'lzj' to run all commands
::

  %lzj   ALL=(ALL)   ALL


