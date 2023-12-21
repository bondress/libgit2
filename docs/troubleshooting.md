Troubleshooting libgit2 Problems
================================

CMake Failures
--------------

* **`Asked for OpenSSL TLS backend, but it wasn't found`**
  CMake cannot find your SSL/TLS libraries.  By default, libgit2 always
  builds with HTTPS support, and you are encouraged to install the
  OpenSSL libraries for your system (eg, `apt-get install libssl-dev`).

  For development, if you simply want to disable HTTPS support entirely,
  pass the `-DUSE_HTTPS=OFF` argument to `cmake` when configuring it.

**What To Fix?**

Hi I installed and build the library on Ubuntu Linux and had some issues, I want to add their solutions to documentation

Could NOT find PkgConfig (missing: PKG_CONFIG_EXECUTABLE) 
-- Could NOT find PkgConfig (missing: PKG_CONFIG_EXECUTABLE) 
-- Could NOT find GSSAPI (missing: GSSAPI_LIBRARIES GSSAPI_INCLUDE_DIR) 
-- Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR (missing: OPE

**Solution**
This error is raised because the pkg-config utility is not available on your system.

Using PkgConfig with CMake is not a truly cross-platform solution, as Windows does not come with the pkg-config utility installed. (The PCL developers should instead use find_package() in their CMake. Perhaps, this is worth opening up a bug report on their Github.) On Linux, this is an easy fix; you can install pkg-config like this:

sudo apt-get install pkg-config

Source: https://stackoverflow.com/questions/33380020/errorcould-not-find-pkgconfig-missing-pkg-config-executable

Summary, for the impatient
To fix this error "-- Could NOT find GSSAPI (missing: GSSAPI_LIBRARIES GSSAPI_INCLUDE_DIR)", run these commands:

sudo ln -s /usr/bin/krb5-config.mit /usr/bin/krb5-config
sudo ln -s /usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2 /usr/lib/libgssapi_krb5.so
sudo apt-get install python-pip libkrb5-dev
sudo pip install gssapi
Source: https://stackoverflow.com/questions/30896343/how-to-install-gssapi-python-module

To fix this error "-- Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY OPENSSL_INCLUDE_DIR)", run this command:

sudo apt install libssl-dev

After you install it, you can now run your compilation which caused the error.

Source: https://www.debugpoint.com/could-not-find-openssl/
