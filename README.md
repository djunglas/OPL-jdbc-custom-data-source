# IBM Decision Optimization Modeling with OPL Custom data source sample

This sample demonstrates how to use the IBM Decision Optimization Modeling with
OPL custom data source API to import data from a JDBC data source into an OPL model.

This sample illustrates the [Subclassing IloCustomOplDataSoource](https://www.ibm.com/support/knowledgecenter/en/SSSA5P_12.8.0/ilog.odms.ide.help/OPL_Studio/opllanguser/topics/opl_languser_extfunc_datasubcl.html) section from the OPL User's manual.
This sample shows how to read and write tuplesets to/from a database with Java. It also enables you to read a database and generate .dat files to be used in the IDE to prototype your optimization model.

One [variation](examples/ilo_opl_call_java) of this sample shows how to read and write tuplesets to/from a database involving only some scripting in your .dat

This sample comes with example connection configurations to DB2, MySQL and MS SQL Server. It can
be easily adapted to any database that has JDBC drivers.
This example will work with any 12.x OPL version, even if it is configured to run with 12.8.0 version.


## Table of Contents
   - [Prerequisites](#prerequisites)
   - [Build and run the sample from java](#build-and-run-the-sample-from-java)
      - [Build the sample](#build-the-sample)
      - [Run sample with DB2](#run-sample-with-db2)
      - [Run sample with MySQL](#run-sample-with-mysql)
      - [Run sample with MS SQL Server](#run-sample-with-ms-sql-server)
   - [Run the sample from OPL](#run-the-sample-from-opl)
   - [Export plain dat files](#export-plain-dat-files)
   - [Run with a previous OPL version](#run-with-a-previous-opl-version)
   - [License](#license)   
   
### Prerequisites

1. This sample assumes that IBM ILOG CPLEX Optimization Studio 12.8.0 is
   installed and configured in your environment.

2. Install [Java](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  
   Once installed, you can check that it is accessible using this command:

	```
	java -version
	```
	
3. Install [Apache ant](http://ant.apache.org/manual/install.html).

4. The sample assumes you have a database with JDBC drivers installed. This
   sample specifically provides instructions for IBM DB2 Express-C and
   MySQL Comunity Server, but is compatible with minimal changes with other JDBC
   compatible databases.

## Build and run the sample from java

The default sample uses model and data from [examples/oil](examples/oil)

### Build the sample

Before you build the sample, you must edit `build.properties` for the appropriate path locations:

* If you want to run the sample with MySQL, `mysql.jdbc.connector.path` should point to your JDBC driver location.
* If you want to run the sample with IBM DB2, `db2.jdbc.connector.path` should point to your JDBC driver location.
* `opl.home` should point to your OPL home, unless you have a `CPLEX_STUDIO_DIR128` set. (this variable should exists if you installed on a Windows machine).

The build file, `build.xml`, imports the build file from the OPL samples,
in `<opl home>/examples/opl_interfaces/java/build_common.xml`.
This build file defines all variables that are needed to configure the execution.

The example is compiled using the `compile` Ant target:
```
ant compile
```
The example is automatically compiled with the run Ant targets is invoked.




### Run sample with DB2

#### Setup the sample database


To run the sample with DB2. you need to install DB2. DB2 Express-C is a free
community edition of DB2. DB2 Express-C is available on Microsoft Windows,
Linux and Mac OS.

You can download and install DB2 Express-C from [here](https://www.ibm.com/developerworks/downloads/im/db2express/).


In a <em>DB2 Command Window</em>:

Create database using `db2 create database CUSTOMDB`

Run the following SQL script to create and populate the example database:
```
db2 -tvmf data/oil_db2.sql
```

Before you run the sample, you need to edit `build.properties` to make `db2.jdbc.connector.path` point
to your DB2 jdbc driver.

You can download the DB2 jdbc driver [here](http://www-01.ibm.com/support/docview.wss?uid=swg21363866).
Note that if you installed DB2 Express-C, your JDBC driver is `db2jcc4.jar`
in `<DB2 installdir>/SQLLIB/java`.

Edit `data\db_db2.xml` for your JDBC connection string and credentials.
Your connection string looks like `db2://localhost:<port>/<database_name>`
where `port` is the DB2 port (default is 50000), `<database_name>` is the name
of your database (default is `CUSTOMDB`).

#### Run the sample
Compile and run the sample for IBM DB2:

```
$ ant run_db2
```

* Uses data/oil.mod as a model file
* Uses data/oil.dat as a data file
* Uses data/db_db2.xml to customize the JDBC custom data source.





### Run sample with MySQL

#### Setup the sample database
To run the sample with MySQL, you need to install MySQL. MySQL Community Server is a free edition of MySQL.

On Microsoft Windows, you can download and install it from [here](https://dev.mysql.com/downloads/mysql/).

On other plateforms, MySQL Community Server is available with most package
managers. Please refer to the [installation instructions](https://dev.mysql.com/doc/refman/5.7/en/installing.html).

You can check your MySQL installation by running <code>mysqladmin</code>.
This binary would be available in /usr/bin on linux and <msysql install dir>/bin
on Windows.
	  
```
[root@host]# mysqladmin --version
```

Before you run the sample, you need to run the script to create and populate
sample tables.

Edit `data\oil_mysql.sql` for your database name. The default for the script is
to create a new database. If you are not an administrator or if you don't
have the permissions to create database, edit the first lines to use your
database.

Run the script with:

```
$ mysql < data\oil_mysql.sql
```

You also need to download the [JDBC driver for MySQL: MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)

Once the driver is download and extracted, edit property `jdbc.connector.path` in `build.properties`
to include the MySQL Connector/J `.jar` (should look like `mysql-connector-java-5.1.40-bin.jar`
in your MySQL Connector/J extracted diretory)

Edit `data\db_mysql.xml` for your JDBC connection string and credentials.
Your connection string looks like `jdbc:mysql://localhost:3306/<database_name>?useSSL=false`
where `<database_name>` is the name of your database (default is `custom_data_source`).

#### Run the sample

Compile and run the sample for MySQL:

```
$ ant run_mysql
```

* Uses data/oil.mod as a model file
* Uses data/oil.dat as a data file
* Uses data/db_mysql.xml to customize the JDBC custom data source.


### Run sample with MS SQL Server

#### Setup the sample database


To run the sample with MS SQL Server, you need to install MS SQL Server. 

In a <em>Commnad Prompt</em> window:

Provided your sql server instance name is SQLEXPRESS, create database using:

```
C:\>sqlcmd -S .\SQLEXPRESS -i data\oil_mssql.sql
```

Before you run the sample, you need to edit `build.properties` to make `sqlserver.jdbc.connector.path` point
to your MSSQL server jdbc driver (i.e. mssql-jdbc-7.2.2.jre8.jar)

Edit `data\db_mssql.xml` for your JDBC connection string and credentials.
Your connection string looks like `	jdbc:sqlserver://localhost;instanceName=<instance>;databaseName=<database_name>;integratedSecurity=true`

where `instance` is the mssql instance name (default is SQLEXPRESS), `<database_name>` is the name
of your database (default is `custom_data_source`).

#### Run the sample
Compile and run the sample for MS SQL Server:

```
$ ant run_mssql
```

* Uses data/oil.mod as a model file
* Uses data/oil.dat as a data file
* Uses data/db_db2.xml to customize the JDBC custom data source.



### Run the sample from OPL

Sample in [examples/ilo_opl_call_java](examples/ilo_opl_call_java) shows how to
use the jdbc custom data source as a library, without having the need to
invoke OPL runtime from java. You can use this method to access database
using a jdbc-custom-data-source from `oplrun` or OPL Studio.


### Reusing the sample with other databases
As the sample is build on JDBC, it's possible to reuse <code>JdbcCustomDataSource</code> with minimal changes:

* Add your JDBC driver in your classpath
* update db.xml with your database connection string

	```	XML
	<!-- The connection string
		 The default url connects to mysql on default port, using database
		'custom_data_source'
	 -->
	<url>jdbc:mysql://localhost:3306/custom_data_source?useSSL=false</url>
	
	<!-- Your connection credentials -->
	<user>sql_user</user>
	<password>mysql</password>
	```

* update db.xml with queries to read your data elements.

	```XML
	<read>
		<query name="Gasolines">SELECT NAME FROM GasData</query>
		<query name="Oils">SELECT NAME FROM OilData</query>
		<query name="GasData">SELECT * FROM GasData</query>
		<query name="OilData">SELECT * FROM OilData</query>
	</read>
	```
	
* update db.xml with your output data elements mapping to tables.

	```XML
	<write>
		<!-- This maps the output dataset "Result" to the "result" table -->
		<table name="Result" target="result"/>
	</write>
	```
* Initialize a new <code>JdbcCustomDataSource</code>, read your database
  configuration file and add your data source to OPL using
  <code>IloOplModel.addDataSource()</code>.
  
	```Java
	JdbcConfiguration jdbcProperties = null;
	String jdbcConfigurationFile = cl.getPropertiesFileName();
	if (jdbcConfigurationFile != null) {
	    jdbcProperties = new JdbcConfiguration();
	    jdbcProperties.read(jdbcConfigurationFile);
	    // Create the custom JDBC data source
	    IloOplDataSource jdbcDataSource = new JdbcCustomDataSource(jdbcProperties, oplF, def);
	    // Pass it to the model.
	    opl.addDataSource(jdbcDataSource);
	}
	```

## Export plain dat files
* When running the `ant` command with the DB2/mysql target, simply add `-Dexport=result.dat` on the command line, and it will export all the tuplesets that have been extracted from the database to `result.dat` file.

## Run with another OPL version
* Edit the build.xml at the root of the directory, and adapt the `example.home` variable to point to your 12.x version.
* Recompile the project

## License

This sample is delivered under the Apache License Version 2.0, January 2004 (see LICENSE.txt).
