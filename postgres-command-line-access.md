## PostgreSQL Command Line Access and SQL interpreter

It is often useful to check your database (DB) directly to ensure that your data is being saved correctly. It is possible to use graphical clients to connect to your DB, but often the simplest and most portable way to check your DB is with the Postgres command line client `psql`.

To use `psql` with this Vagrant box run the following commands:
```
$ vagrant ssh
$ PGUSER=myapp PGPASSWORD=dbpass psql -h localhost myapp
```

This will give you a prompt that will look similar to this:
```
vagrant@postgresql:~$ PGUSER=myapp PGPASSWORD=dbpass psql -h localhost myapp
psql (9.4.5)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.

myapp=>
```

From here you can use SQL queries or Postgres commands. The following are some useful commands to try:

1. `\dt`
  - List database tables
2. `\?`
  - Get a list of Postgres commands. Type `q` to exit.
3. `\help <SQL_COMMAND>`
  - Get documentation for SQL commands. Replace `<SQL_COMMAND>` with the sql command of your choice (`SELECT`, `INSERT`, etc...). Type `q` to exit.
4. `\q`
  - Quit the `psql` client.
  
### Postgres Guide and SQL Basics

Check out the following website for a guide on using Postgres and running basic SQL queries in `psql`.

  - http://postgresguide.com
  - http://postgresguide.com/sql/select.html
