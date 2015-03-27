#Simplicity app - Backend

The simplicity backend powers the [simplicity ui](https://github.com/cityofasheville/simplicity-ui) and [simplicity app](http://cityofasheville.github.io/simplicity-ui)

##What the backend accomplishes.
* Processes data into the format the simplicity ui expects.
* Verifies correct schema for data.
* Compiles spatial queries in advance.
* Can checks data consistency to ensure simplicity app will work properly.
* Can run database maintenance.

##Installing
```sh
$ git clone https://github.com/cityofasheville/simplicity-backend.git
$ cd simplicity-backend
$ npm install
```

##Setup

###database connection
```sh
$ cp db-sample.yml config/db.yml
```
**Warning:** naming this file anything but db.yml will push the database configuration including the password to github on a git push.

edit `config/db.yml` and update with your settings.

```sh
$ cp datatests-sample.yml config/db.yml
```

#####example database configuration
of course change to match your postgres settings.
```yaml
host: 192.162.0.1
database: postgres
user: postgres
password: postgres
```

####maintenance configuration
```sh
$ cp maintenance-sample.yml config/maintenance.yml
```

edit `config/maintenance.yml` and update with your settings.

#####example maintenance configuration
```yaml
sql:
- name: reindex postgres.table1
  text: REINDEX TABLE postgres.table1;
  values:
- name: VACUUM postgres.table1
  text: VACUUM ANALYZE postgres.table1;
  values:
```

####Data Tests Configuration
```sh
$ cp datatests-sample.yml config/datatests.yml
```

edit `config/maintenance.yml` and update with your settings.

*NOTE:*  The SQL statement must have one field that returns a boolean named check.  The SQL statement must return one row.

#####Example Data Tests Configuration
```yaml
sql:
- name: count table1
  text: SELECT count(*) > $1 as check FROM postgres.table1;
  values: [1000000]
- name: count table2
  text: SELECT count(*) = $1 as check FROM postgres.table2;
  values: [12345]
```

##Running
```

Usage: app [options]

 Options:

   -h, --help                 output usage information
   -V, --version              output the version number
   -d, --databaseconn <file>  PostGres database connection file <file> default is config/db.yml
   -m, --maintenance <file>   maintenance configuration file <file>
   -t, --datatest <file>      data test configuration file <file>
```

##Software
* PostGres 9.2
  * PG extentions PostGIS 2
  * PG extentions fuzzystrmatch
  * PG extentions hstore
  * PG extentions pg_trgm
  * PG extentions plpgsql
  * PG extentions postgis_topology
* ArcGIS Server 10.2.2
  * ArcGIS SDE - for use in ArcGIS Server only
  * ESRI based Geocoders

##Requires
* that the database exists and scehma is in place
* Place all scripts in directory /home/ubuntu/job or change in cron job
* postgresql/postgis installed
* Some external process to push source data into stagging tables
