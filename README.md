maillog2db
============

Capture mail logs to your database

# Purpose

maillog2db is a golang program designed to monitor your postfix maillog and push log entries to a mysql database. maillog2db captures the following data from postfix logs:

* Raw log entries are inserted into a single table
* Messages and delivery queues are split and tracked among multiple tables
* Connecting Clients are tracked. 

Please create a github issue or submit a pull request if you would like to see the data parsed/handled in any other way, and we will consider making any adjustments.

# Added subject extraction from postfix log

# Example Usage

Once you have the binary available either pre-compiled or source-compiled, usage is as follows (assuming installed under /opt/maillog2db):

```bash
/opt/maillog2db/maillog2db -dbhost 10.10.0.1 -dbport 3306 -dbname maillog2db -dbuser maillog2db -dbpass MySecretPassword -maillog /var/log/maillog
```

Once the binary is started, it will parse any existing entries and watch for any new ones. 

for a full list of CLI options, use the following:

```
/opt/maillog2db/maillog2db -h
```

# Database Schema

The database schema is automatically created by the program upon startup. ensure your database user has **CREATE TABLE** permissions on the database. 

# Logging

The program log file by default is placed next to the binary location (ex: /opt/maillog2db/maillog2db > /opt/maillog2db/maillog2db.log). If you would like to change the logging location, simply supply the -logfile CLI switch:

```
/opt/maillog2db/maillog2db -logfile /var/log/maillog2db.log
```

# Build and run with docker

To run program as a daemon you can use docker. To build docker image:
```
docker build -t maillog2db .
```
Run docker image:
```
docker run -d --restart always -v /var/log/maillog:/var/log/maillog maillog2db -dbhost <dbhost> -dbname <dbname> -dbpass <dbpassword> -dbuser <dbuser> -maillog /var/log/maillog -logfile /dev/stdout 
```
