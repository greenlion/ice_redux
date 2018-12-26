#ICE_REDUX 1.0.0 - Modified version of Infobright Community Edition 4.0.7 (GPL v2)

This is an updated version of Infobright Community Edition.  The only major 
change from ICE 4.0.7 is that the source and build tools have been updated 
to compile on modern GCC.

Requires bison 2.x.  Modern Linux has bison 3.x, so you will need to 
download and install a copy of 2.x and place it somewhere in your path
before bison 3.x.

Note that originally ICE was built with boost 1.42.0 which no longer 
compiles on modern GCC.  You can use the latest version of boost
instead.  Remeber to set BOOST_ROOT to the path to the boost directory
before running make EDITION=community release.

Original README:

Infobright Installation Using a Source Distribution
===================================================

* Menu:

* Source Installation Overview
* Dealing with Problems Compiling Infobright

You need the following tools to build and install MySQL and Infobright from source:

   * A working gcc compiler (recomended version is 4.2.x).
   * Properly installed autoconf and other gnu tools such as aclocal, autoheader, libtool(1.5.22), automake, 
     autoconf (2.59), autoreconf (2.59), make (3.81), m4 - macro preprocessor(1.4.5), libncurses5, libncurses5-dev, zlib, 
     zlib-devel, perl, bison etc.
   * boost 1.42 or higher (required boost-regex*, boost-program-options*,
     boost_thread*, boost_filesystem*, boost_system*). In Infobright we compile boost using the following steps
     - download boost 1.42 and unpack it
     - cd to unpacked folder
     - ./bootstrap.sh --prefix=/usr/local/boost_1_42_0
     - ./bjam install
     - export BOOST_ROOT=/usr/local/boost_1_42_0
     
Source Installation Overview
----------------------------------

The basic commands that you must execute to compile and install a MySQL and Infobright source
distribution are:

    shell> groupadd mysql
    shell> useradd -g mysql mysql

    # To compile and install MySQL and Infobright server and client tools
    shell> cd infobright-version
    shell> make EDITION=community release
    shell> make EDITION=community install-release

    # Setting config file and brighthouse.ini file.
    shell> cp src/build/pkgmt/my-ib.cnf /etc/
    
    Note: You can customize /etc/my-ib.cnf file by changing port, socket etc.
    If you are compiling on a 32 bit system, you need to rename brighthouse.ini.32bit
    (can be found in package dir /usr/local/infobright/share/mysql/ etc.) to brighthouse.ini. 
    The existing brighthouse.ini is configured with higher memory settings suitable 
    for a 64 bit system.
	
    shell> cd /usr/local/infobright
    shell> bin/mysql_install_db --defaults-file=/etc/my-ib.cnf --user=mysql
    shell> chown -R root  .
    shell> chown -R mysql var cache
    shell> chgrp -R mysql .
    shell> `pwd`/libexec/mysqld --defaults-file=/etc/my-ib.cnf --user=mysql

A more detailed version of the preceding description for installing
MySQL and Infobright from a source distribution follows:

 1. Add a login user and group for `mysqld' to run as:

    shell> groupadd mysql
    shell> useradd -g mysql mysql

    These commands add the `mysql' group and the `mysql' user. The
    syntax for `useradd' and `groupadd' may differ slightly on
    different versions of Unix, or they may have different names such
    as `adduser' and `addgroup'.

    You might want to call the user and group something else instead
    of `mysql'. If so, substitute the appropriate name in the
    following steps.

 2. Pick the directory under which you want to unpack the distribution
    and change location into it.

 3. Unpack the distribution into the current directory:

    shell> gunzip < /PATH/TO/infobright-version-src.tar.gz | tar xvf -

    This command creates a directory named infobright-version-src.

 4. Configure, compile and install MySQL and Infobright source code using the following commands:

    shell> cd infobright-version
    shell> make EDITION=community release
    shell> make EDITION=community install-release

    # Setting config file and brighthouse.ini file.
    shell> cp vendor/mysql/support-files/my-ib.cnf /etc/
    
 5. Change location into the installation directory:

    shell> cd /usr/local/infobright

 6. If you haven't installed MySQL before, you must create the MySQL
    grant tables:

    shell> bin/mysql_install_db --defaults-file=/etc/my-ib.cnf --user=mysql

    If you run the command as `root', you should use the `--user'
    option as shown. The value of the option should be the name of the
    login account that you created in the first step to use for
    running the server. If you run the command while logged in as that
    user, you can omit the `--user' option.

    After using `mysql_install_db' to create the grant tables for
    MySQL, you must restart the server manually. The `mysqld_safe'
    command to do this is shown in a later step.

    shell> bin/mysql_install_db --defaults-file=/etc/my-ib.cnf --user=mysql

 7. Change the ownership of program binaries to `root' and ownership
    of the data directory to the user that you run `mysqld' as.
    Assuming that you are located in the installation directory
    (`/usr/local/infobright'), the commands look like this:

    shell> chown -R root  .
    shell> chown -R mysql var cache
    shell> chgrp -R mysql .

    The first command changes the owner attribute of the files to the
    `root' user. The second changes the owner attribute of the data
    directory to the `mysql' user. The third changes the group
    attribute to the `mysql' group.

  8. After everything has been installed, you should test your distribution.
    To start the server, use the following command:

    shell> cd /usr/local/infobright
    shell> `pwd`/libexec/mysqld --defaults-file=/etc/my-ib.cnf --user=mysql
  
  9. To run the client:
    shell> cd /usr/local/infobright
    shell> bin/mysql --defaults-file=/etc/my-ib.cnf -uroot
    
If that command fails immediately and prints `mysqld ended', you can
find some information in the `HOST_NAME.err' file in the var directory.

Dealing with Problems Compiling Infobright
------------------------------------------

If you use GNU make 3.81, it might generate warning messages 
"-jN forced in submake: disabling jobserver mode". Please ignore such warnings.
Make sure sytem OS and processor are supported by Infobright. Also make sure that
all the packages mentioned on top of the page are properly installed.
