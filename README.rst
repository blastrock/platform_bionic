bionic for flix
===============

This is the libc implementation you need to compile programs for the flix
kernel.

How to compile
--------------

The project build system was changed to CMake. You need to compile bionic and
install it in a sysroot prefix so that other programs can be compiled against
this sysroot. The libraries will be linked statically as flix does not support
dynamic linking.

To compile::

    $ mkdir build
    $ cd build
    $ cmake .. -DCMAKE_INSTALL_PREFIX=<your sysroot>
    $ make
    $ make install
