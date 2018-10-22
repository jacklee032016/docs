
GIT and Branch
#########################
10.22, 2018


What is Branch
===========================
Branch is used to develop new function or fix bugs when a matural version (``master``) is usable;

Default, ``master`` is branch for code base; ``master`` is default name or top level of local repository;

Other Branches are created, and developed, then merged into ``master``; ``master`` also can be merged into other branches;


Create and Use Branch
===========================
Create and use 

::

  git checkout -b BRANCH_NAME : create and use BRANCH_NAME


Change working space to one branch

::

  git checkout BRANCH_NAME|master : use BRANCH_NAME or ``master``
 
  git branch : list branches


Remote for Branches
==============================

For new local Repository
---------------------------
For a new repository in local repository:

::

   git remote add origin http://**/*/*.git : ``origin`` is remote name;
   git push -u origin master : push master(local and source) into origin(remote and destination)

Remote
-----------

::

   git remote add RemoteName BranchName
   git push RemoteName URL : 
   
   git fetch RemoteName : update branch from remote 

Merge Branch
===========================
Merge one branch (which is not current working space) into current working space

Merge branch into master
----------------------------
Enter into master working space and merge:

::

   git checkout master : 
   git merge iss53
   
Merge ``master`` into branch
-------------------------------
Current in ``hotfix`` working space:

::

   git merge master : merge master into current branch's working space of ``hotfix``;


Delete Branch
===================

::

    git branch -d|-D BranchName
    git push origin BranchName : delete branch on github;

