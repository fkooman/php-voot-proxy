# Introduction
This is a VOOT proxy service providing an OAuth 2.0 protected VOOT API
aggregating data from various external group providers.

# Configuration
To install the required dependencies run:

    $ sh docs/install_dependencies.sh

To set file permissions and setup the configuration file run:

    $ sh docs/configure.sh

To initialize the database run:

    $ php docs/initProxyDatabase.php

Now you can modify the `tokenEndpoint` option in `config/proxy.ini` to point to
your `php-oauth` installation's token endpoint.

Don't forget to add the Apache configuration snippet that was shown as output
of the `configure.sh` script to your Apache configuration.

# Adding some Group Providers
There is no API available yet, so for now just use your SQL tools to add group
providers:

    INSERT INTO "ExternalGroupProviders" 
    VALUES(
        'foo',                                  ## id
        'Foo',                                  ## name
        'CS Group Provider',                    ## description
        'http://localhost/php-voot/api.php',    ## endpoint
        'foo',                                  ## username
        'bar',                                  ## password
        NULL                                    ## filter
    );
    
# VOOT / OpenSocial API support
The following endpoint is available to retrieve the list of groups you are a
member of:

    /groups/@me

The following endpoint is available to retrieve the list of people part of 
the group specified in `<GROUP_ID>`:

    /people/@me/<GROUP_ID>

The `<GROUP_ID>` can be obtained through the `/groups/@me` call which return
the groups the user is a member of and the `id`.

The following parameters can be used to "tweak" the results coming from the 
API:

* `startIndex` - the index at which to start with returning groups/people 
  (default is `0`), this is useful to implement "pagination";
* `count` - the number of groups/people to return (default is no limit on
  the number of results), this is useful to implement "pagination";
* `sortBy` - the element to sort by. For groups only sorting by `title` is
  supported. For people sorting by `id` and `displayName` is supported 
  (default is no sorting at all);
* `sortOrder` - the order, can be either `ascending` or `descending` 
  (default is ascending, but `sortBy` has to be set to a supported value
  as well).

An example:

    /groups/@me/urn:foo:mygroup?startIndex=5&count=5&sortBy=displayName&sortOrder=descending

