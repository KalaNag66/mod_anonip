mod_anonip
==========

A simple Apache2 module to anonymize the IP address of any request by a specific number of bytes.

The original version of *mod_anonip* is based on [*mod_removeip*](http://www.wirspeichernnicht.de/content/view/14/24/),
but *mod_anonip* just anonymizes the IP-address by removing a specified number of bytes instead of removing the IP-address completely. This - unlike *mod_removeip* - allows you to preserve the privacy of your visitors, while still being able to log the origin country of a request or to serve website content based on GeoIP.

Installation
============

First you have to clone the repository and compile the module:

    $ git clone https://github.com/flowyapps/mod_anonip
    $ cd mod_anonip/module
    $ make anonip

You might have to install some kind of Apache development package (`libapache2-prefork-dev` or `apache2-threaded-dev` on Ubuntu) to have all tools required for compilation.

After the compilation has finished, you can install and activate the module:

    # make install
    # cd ../conf
    # cp * /etc/apache2/mods-available/
    # a2enmod anonip

After enabling the module you have to restart Apache (`service apache2 restart` on Ubuntu).

Configuration
=============

The module comes with one configuration option, which defines how many bytes should be masked:

    AnonipMask = 2
    
The default value is `0`, so `mod_anonip` will do nothing if no configuration exists. The configuration files shipped with the module set `AnonipMask = 2`. Values between 0 and 4 are valid with the following results:

    AnonipMask    Anonymized address
    0             192.168.1.1
    1             192.168.1.0
    2             192.168.0.0
    3             192.0.0.0
    4             0.0.0.0
    [other]       192.168.1.1

As long, as you mask at least 2 bytes of the IP address, your website will be in accordance with the privacy recommendations of the [Independent Centre for Privacy Protection in Germany (ULD)](https://www.datenschutzzentrum.de).
