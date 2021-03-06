[//]: # " Copyright (c) 2015, 2018 Oracle and/or its affiliates. All rights reserved. "
[//]: # " "
[//]: # " This program and the accompanying materials are made available under the "
[//]: # " terms of the Eclipse Distribution License v. 1.0, which is available at "
[//]: # " http://www.eclipse.org/org/documents/edl-v10.php. "
[//]: # " "
[//]: # " SPDX-License-Identifier: BSD-3-Clause "

Bookmark Example
================

This example demonstrates how to use JPA in the backend. The example is
based on bookmark example from the [RESTful Web
Services](http://www.oreilly.com/catalog/9780596529260/) book, which was
inspired by the [http://del.icio.us/](http://del.icio.us/v1) web
service.

A bookmark web application is presented that is capable of maintaining
users and their bookmarks.

Contents
--------

The mapping of the URI path space is presented in the following table:

URI path                                | Resource class      | HTTP methods                                          | Notes
--------------------------------------- | ------------------- | ----------------------------------------------------- | --------------------------------------------------------
**_/users_**                            |  UsersResource      |  GET                                                  |  Returns a list of users.
**_/users/{userid}_**                   |  UserResource       |  GET, PUT (used also for creating new users), DELETE  |  Returns user details
**_/users/{userid}/bookmarks_**         |  BookmarksResource  |  GET, POST                                            |  Returns a list of bookmarks for actual user resource.
**_/users/{userid}/bookmarks/{bmid}_**  |  BookmarkResource   |  GET, PUT, DELETE                                     |  Returns bookmark uri and a long and short description.

Running the Example
-------------------

Bookmark example runs on Glassfish 3.x application servers
([https://javaee.github.io/glassfish/](https://javaee.github.io/glassfish/)) and needs a
running JavaDB
(<http://www.oracle.com/technetwork/java/javadb/overview/index.html>)
instance for underlying data (it comes along with GlassFish).

Presuming, you have installed Glassfish 3.1 `AS_HOME` variable should
point to your glassfish installation directory.

#### Building And Starting The Bookmark Service

If `.asadminpass` file is missing at your home directory, you will need
to run the following command to get it created

    $AS_HOME/bin/asadmin start-domain
    $AS_HOME/bin/asadmin login

You then build and run the example by

    mvn package
    $AS_HOME/bin/asadmin start-domain
    $AS_HOME/bin/asadmin start-database
    $AS_HOME/bin/asadmin create-jdbc-connection-pool \
             --datasourceclassname org.apache.derby.jdbc.ClientDataSource \
             --restype javax.sql.DataSource \
             --property "portnumber=1527:password=REST:user=REST:serverName=localhost:databaseName=BookmarkDB:connectionAttributes=;create\=true" bookmarkPool
    $AS_HOME/bin/asadmin create-jdbc-resource --connectionpoolid bookmarkPool jdbc/bookmarkSample
    $AS_HOME/bin/asadmin deploy target/bookmark.war

**Notice:** <span style="color: #f00">On Windows</span>: you might need
to replace asadmin with asadmin.bat. Also, because it seems there are
some issues with copy-pasting backslashes, you will also need to use the
following one-liner instead of the above mentioned complex command:

    asadmin create-jdbc-connection-pool --datasourceclassname org.apache.derby.jdbc.ClientDataSource --restype javax.sql.DataSource --property "portnumber=1527:password=REST:user=REST:serverName=localhost:databaseName=BookmarkDB:connectionAttributes=;create\=true" bookmarkPool

and make sure you do not miss the backslash (\\) in `create\=true` part.

#### Test Client Running

    mvn test -Djersey.config.test.container.factory=org.glassfish.jersey.test.external.ExternalTestContainerFactory -Djersey.config.test.logging.enable=true -Djersey.config.test.logging.dumpEntity=true -Djersey.config.test.container.port=8080 -Ptest

Links
-----

<http://java.net/projects/bpcatalog/sources/svn/content/trunk/ee5/docs/persistence/webonlyapp.html?raw=true>