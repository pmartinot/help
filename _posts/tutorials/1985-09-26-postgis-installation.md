---
layout: post
template: two-col
title:  "How to install PostGIS"
so_title: "postgis"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   1900-09-26 15:33:13
categories: 
lead: You can install PostGIS automatically or manually
search-tags: ['']
tags: ['Customization', 'Database']
tutorial: true
difficulty: 1
---

<h2>Contents</h2>
<ul class="page-toc">
	<li>
		<a href="#cloud66">Installing with Cloud 66</a>
	</li>
	<li>
		<a href="#manual">Manual installation</a>
	</li>
</ul>

<h2 id="cloud66">Installing with Cloud 66</h2>

Installing through Cloud 66 is as simple as a manifest file entry. Refer to our [Manifest file](http://help.cloud66.com/building-your-stack/getting-started-with-manifest-files) documentation for more information.

In order for your manifest file to be picked up, you need a file called **manifest.yml** to be present within a folder named **.cloud66** that is in turn located in the root of your source code, and checked into your repository.

<pre class="prettyprint">
[source&#95;repo]/.cloud66/manifest.yml
</pre>

For Cloud 66 to install PostGIS, your manifest file should contain the following:

<pre class="prettyprint">
production:
    postgresql:
        configuration:
            postgis: true
</pre>

If you would like to specify versions, it should look something like this:

<pre class="prettyprint">
production:
    postgresql:
        configuration:
        	version: 9.3.4
            postgis:
                version: 2.1.1
</pre>

Once your stack has been deployed, you can find your PostGIS file in `/etc/postgresql/share/contrib/postgis-2.1/postgis.sql` on your database server.

<h2 id="manual">Manual installation</h2>
Applies specifically to Ubuntu 14.04 (the officially supported Cloud66 OS).

### Prerequisites
A number of prerequisites are needed, which can either be built from source or installed from pre-built packages, as shown below.

Install packages using:

<pre class="prettyprint">
$ sudo apt-get install build-essential postgresql-9.1 postgresql-server-dev-9.1 libxml2-dev libproj-dev libjson0-dev xsltproc docbook-xsl docbook-mathml
</pre>

Optional package for raster support (this is required if you want to build the PostgreSQL extensions):

<pre class="prettyprint">
$ sudo apt-get install libgdal1-dev
</pre>

### Build GEOS 3.3.x
PostGIS 2.0 requires GEOS >= 3.3.2 for topology support, and because Ubuntu 12.0.4 (which Cloud 66 deploys on) only has GEOS 3.2.2 in packages, we need to build it from source. If you don't need topology, you don't *need* to build this component, but it is highly recommended.

There are many ways of building GEOS, but this is the simplest:

<pre class="prettyprint">
$ wget http://download.osgeo.org/geos/geos-3.3.8.tar.bz2
$ tar xvfj geos-3.3.8.tar.bz2
$ cd geos-3.3.8
$ ./configure
$ make
$ sudo make install
$ cd ..
</pre>

### Build PostGIS
First we want to download PostGIS, extract it and move into its directory:

<pre class="prettyprint">
$ wget http://download.osgeo.org/postgis/source/postgis-2.0.3.tar.gz
$ tar xfvz postgis-2.0.3.tar.gz
$ cd postgis-2.0.3
</pre>

PostGIS 2.0 can be configured to disable topology or raster components using the configure flags --without-raster and/or --without-topology. The default is to build both. Note that raster is required for the extension installation method for PostgreSQL.

<pre class="prettyprint">
$ ./configure
$ make
$ sudo make install
$ sudo ldconfig
$ sudo make comments-install
</pre>

Finally, enable the command-line tools to work from your shell:

<pre class="prettyprint">
$ sudo ln -sf /usr/share/postgresql-common/pg&#95;wrapper /usr/local/bin/shp2pgsql
$ sudo ln -sf /usr/share/postgresql-common/pg&#95;wrapper /usr/local/bin/pgsql2shp
$ sudo ln -sf /usr/share/postgresql-common/pg&#95;wrapper /usr/local/bin/raster2pgsql
</pre>

### Spatially enabling a database
With PostgreSQL 9.1, there are two methods of adding PostGIS functionality to a database: using extensions or using enabler scripts.

#### PostGIS extension for PostgreSQL
Spatially enabling a database using extensions is a new feature of PostgreSQL 9.1.

Connect to your database using pgAdmin or psql, and run the following commands. To add PostGIS with raster support:

<pre class="prettyprint">
$ CREATE EXTENSION postgis;
</pre>

To add topology support, a second extension can be created on the database:

<pre class="prettyprint">
$ CREATE EXTENSION postgis&#95;topology;
</pre>

#### Enabler scripts/template
Enabler scripts can be used to either build a template or directly spatially enable a database. This method is older than the extension method, but is required if the raster support is not built.

The following example creates a template which can be re-used for creating multiple spatially-enabled databases. Or if you just want to make one spatially enabled database, you can modify the commands for your needs.

PostGIS:

<pre class="prettyprint">
$ sudo -u postgres createdb template&#95;postgis
$ sudo -u postgres psql -d template&#95;postgis -c "UPDATE pg&#95;database SET datistemplate=true WHERE datname='template&#95;postgis'"
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/postgis.sql
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/spatial&#95;ref&#95;sys.sql
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/postgis&#95;comments.sql
</pre>

With raster support:

<pre class="prettyprint">
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/rtpostgis.sql
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/raster&#95;comments.sql
</pre>

With topology support:

<pre class="prettyprint">
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/topology.sql
$ sudo -u postgres psql -d template&#95;postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/topology&#95;comments.sql
</pre>