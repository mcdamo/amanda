# Install FreeBSD

Instructions for building Amanda 3.5.1 on FreeBSD (also FreeNAS)

## Dependencies
Install runtime dependencies
```
pkg install p5-json p5-encode
```

Install build-dependenices, optional _libxslt_ and _docbook_ required for **manpage**. Tip: to also build [amplot](https://wiki.zmanda.com/man/amplot.8.html) add _gnuplot_ package.
```
pkg install automake autoconf bison flex glib swig pkgconf gtar gzip gmake \
   openssl-devel libxslt docbook-xsl docbook-xml
```

## Build
### Autogen
Generate make scripts:
```
./autogen
```

### Configure
Trouble building some NDMP dependencies, so disable it:
```
./configure --with-user=amanda --with-group=amanda --enable-manpage-build --disable-ndmp-device
```

_make_ fails with this error:
```
In file included from /usr/include/rpc/rpc.h:45:
../gnulib/sys/socket.h:68:3: error: "Please include config.h first."
```
Modify `gnulib/sys/socket.h:68` to workaround the above error:
```c
#ifndef _GL_INLINE_HEADER_BEGIN
 # include "config.h"
#endif
```

### Make
Use _gmake_ for compatibility:
```
gmake
```

```
gmake install
```

## Upgrading
When upgrading from a previous source build be sure to clean the old install _before_ rebuilding, else the `make install` step can fail while running perl tests.

This can be done with the provided scripts
```
gmake uninstall
```

Alternatively simply remove the previous perl libraries
```
rm -rf /usr/local/lib/perl5/site_perl/auto/Amanda
```
