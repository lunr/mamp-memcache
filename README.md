# Installing Memcache Extension for MAMP

This _README_ document outlines steps to install the `memcache` PHP extension for MAMP 3.4 and 3.5.

These instructions are broken into two main sections, the Basic installation and the Advanced installation.

The Basic installation goes through how to install the pre-built extensions included in this repository to specific versions of PHP included with MAMP.

The Advanced installation explains how to download and build the extension from source code so you can install the extension for any version of PHP included with MAMP.

## Basic Installation

The basic installation assumes you are running MAMP 3.4 or 3.5 and are using one of the installed versions of PHP including PHP 5.4.42, PHP 5.5.26, or PHP 5.6.10.

### 1) Locate the appropriate extension version

First, find the appropriate folder at the root of this repository that matches your version number. Inside of the directory should be a file named `memcache.so`.

### 2) Copy extension into PHP extensions folder

Once you've found the appropriate extension version, copy it to the extensions folder for the version of PHP you are using.

The path of the exension folder follows the following format:

    /Applications/MAMP/bin/php/php[VERSION]/lib/phpextensions/no-debug-non-zts-[DATE]

For example, for PHP 5.6.10, the extensions folder is located here:

    /Applications/MAMP/bin/php/php5.6.10/lib/php/extensions/no-debug-non-zts-20131226/

Copy the appropirate `memcache.so` file into the extensions directory:

    cp memcache.so /Applications/MAMP/bin/php/php5.6.10/lib/php/extensions/no-debug-non-zts-20131226/

### 3) Add extension to PHP configuration

Finally, you will need to add the extension configuration line to the appropriate `php.ini` file.

You can edit the `php.ini` file for your version by opening MAMP, and from the menu bar, navigate to `MAMP -> File -> Edit Template -> PHP`. Select the version you are using and MAMP will open a text editor for the `php.ini` file.

Add the following line to the end of the file, or optionally near other `extension=` configuration lines:

    extension=memcache.so

Then, restart MAMP, and confirm the `memcache` extension is installed by checking `phpinfo()` for the word `memcache`.

## Advanced Installation

Follow these instructions if you want to compile it yourself and not use the prebuilt extensions.

**These instructions are for building on PHP 5.5.26 (MAMP 3.5)**

### 1) Make sure you have XCode command line tools installed.

We are going to need a C compiler and other libraries to upgrade cURL. So fire up a Terminal which you will continue to use for each step in this documentation.

    xcode-select --install

### 2) Install the source for PHP

You will need to use PHP to build the extension which requires have the source to the PHP version you are building against. You can download the source at http://php.net/downloads.php.

Once you've downloaded and extracted the tarball, assuming you're building for PHP 5.5.26 (MAMP 3.5), run the following commands to move the source and configure PHP:

    mkdir /Applications/MAMP/bin/php/php5.5.26/include
    mv ~/Downloads/php-5.5.26 /Applications/MAMP/bin/php/php5.5.26/include/php
    cd /Applications/MAMP/bin/php/php5.5.26/include && ./configure

### 3) Compile `memcache` from PECL

Download the `memcache` source from the PECL website:

     wget http://pecl.php.net/get/memcache-2.2.7.tgz

Extract the source into a working directory:

    tar xzvf memcache-2.2.7.tgz
    cd memcache-2.2.7.tgz

Add the MAMP PHP `bin` folder to your path:

    export PATH="/Applications/MAMP/bin/php/php5.5.26/bin:$PATH"

Configure the API version for PHP:

    /Applications/MAMP/bin/php/php5.5.26/bin/phpize

Setup compiler configuration and build:

    CFLAGS='-O3 -fno-common -arch i386 -arch x86_64' LDFLAGS='-O3 -arch i386 -arch x86_64' CXXFLAGS='-O3 -fno-common -arch i386 -arch x86_64' ./configure --prefix=/Applications/MAMP/Library && make

### 4) Install `memcache` extension

You should now have a `memcache.so` extension file in the `modules` directory of your source code working directory. Install the extension into PHP's extensions directory:

    cp modules/memcache.so /Applications/MAMP/bin/php/php5.5.26/lib/php/extensions/no-debug-non-zts-20121212/

Finally, open the `php.ini` file for PHP 5.5.26, and add the following line to it, optionally near the other `extension=` lines:

    extension=memcache.so

For MAMP, you can edit the INI file by going to `MAMP -> File -> Edit Template -> PHP -> PHP 5.5.26 php.ini` in the menu bar of Mac OS X.

### References

* http://phantombear.net/mamp-pro-3-pecl-extensions-working-memcache/
* https://drewish.com/2010/07/20/mamp-memcache-drilling-a-screw-in-my-eye/
