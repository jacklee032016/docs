
FAQ in Fedora
########################
09.09,2018


Enable sudoers for user 'lzj'
-------------------------------
Command line:
::

  visudo  or vi /etc/sudoers
  
Add one line: allow user in group 'lzj' to run all commands
::

  %lzj   ALL=(ALL)   ALL


