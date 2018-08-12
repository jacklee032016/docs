
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Don't link unused symbols in binary
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Aug.10th, 2018

**gcc compiling options:**

::

 -fdata-sections -ffunction-sections 
 
keep the data and functions in separate sections, so they (data and functions) can be discarded if unused.

**ld linking options:**

::

 --gc-sections, --no-gc-sections

Enable garbage collection of unused input sections

 
