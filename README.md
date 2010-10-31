# Universal Character Set Detector (UCSD)

A library exposing a C interface and dependency-free interface to the Mozilla C++ UCSD library.

Pulls together:

  * A NSPR emulation library (see `nspr-emu/README.md`)
  * Code written by Colin Snover to provide a command line interface to the library
  * The UCSD library itself from the Mozilla seamonkey source tree

## Building

We have a build system based on CMake, so you will need that installed. That done, simply do this incantation:

    cmake .
    make
    sudo make install

This will install the header file `charsetdetect.h` and the UCSD shared library, which you should link against in your compiler.

## API documentation

The library provides an opaque type of character set detectors:

    typedef void* csd_t;

The first thing a client should do is create one of these:

    csd_t csd_open(void);

A `csd_t` created in this fashion must be freed by `csd_close`. If creation fails, `csd_open` returns `(csd_t)-1`.

Now you need to feed some data to the detector:

    int csd_consider(csd_t csd, const char *data, int length);

The meaning of the return code is as follows:

  * Returns 0 if more data is needed to come to a conclusion
  * Returns a positive number if enough data has been received to detect the character set
  * Returns a negative number if there is an error

Finally, close the detector to find out what the character set is:

    const char *csd_close(csd_t csd);

The detected character set name is returned as an ASCII string. This function returns `NULL` if detection failed because there was not
enough data. It is safe to call `csd_close` at any point from creation by `csd_open` to the first call of `csd_close` on that character
set detector.

## Licensing

The files `libcharsetdetect.{cpp,h}` are (c) 2010 Colin Snover and released under an MIT license.

The UCSD is (c) mozilla.org and tri-licensed under MPL 1.1/GPL 2.0/LGPL 2.1.

We incorporate header files from the NSPR emulation library, which is LGPL licensed.

Thus the resulting artifact is LGPL licensed (I think).