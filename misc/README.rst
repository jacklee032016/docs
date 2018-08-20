

__VAR_ARGS__ and ##_VAR_ARGS__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
08.18, 2018

__VAR_ARGS__ represents variable parameters in function like ::

  define eprint(fmt, ...)   print(fmt, __VAR_ARGS__)
  
In this case, if no more parameters are used after fmt, then an odd comma is there, and fails.

So GCC add new extension of ##__VAR_ARGS__, which can remove the last comma when optional parameters are not used.


Macros:

 * `##` is token paste(concatenation), which concatenate 2 token together;
 * `#`: create a string from symbol;

 
 -fexceptions  -g -O2 -fstack-protector 

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

 
