How to set up a PostgreSQL database on your Arch Linux

Install PostgreSQL using pacman:

```bash
sudo pacman -S postgresql
```
Check it's been installed:

```bash
sudo pacman -Si postgresql
```

Before you can do anything, you must initialize a database storage area (cluster) on disk. In file system terms, a database cluster is a single directory under which all data is stored. It is completely up to you where you choose to store your data. There is no default, although locations such as /usr/local/pgsql/data or /var/lib/postgres/data are popular. This latter is my preferred.

```bash
sudo mkdir /var/lib/postgres/data
```

Change the owner of the /var/lib/postgres directory and its contents to the postgres user (the default user set up by the install):

```bash
sudo chown -c -R postgres:postgres /var/lib/postgres
```

To initialize a database cluster, use the command initdb, which is installed with PostgreSQL. This must be done as the postgres user, so become this user:

```bash
sudo -i -u postgres
initdb -D '/var/lib/postgres/data'
```

Now you can logout from the postgres user and fire up PostgreSQL:

```bash
logout
sudo systemctl start postgresql
```
If you want PostgreSQL to start automatically every time your VPS boots up, use this:

```bash
sudo systemctl enable postgresql
```

PostgreSQL is now running. By creating another PostgreSQL user as per your local Arch user ($USER), you can access the PostgreSQL database shell directly instead of having to log in as the postgres user:

```bash
createuser -s -U postgres --interactive
Enter name of role to add: myUsualArchLoginName
```
(Substitute myUsualArchLoginName for your Arch login name.)

Now you can create databases and access them as an Arch user. Here's an example:

```bash
createdb myDatabaseName
```
Use the psql command to access the PostgreSQL database shell, psql. (-d specifies the database to connect to.)

```bash
psql -d myDatabaseName
```
Since you have yet to create any tables and input any data into this database, just list all the database's users and their permissions:

You should have two users: postgres and your Arch login user.

Type \q or CTRL+d to exit the PostgreSQL database shell back to your command line.

Some psql commands to keep handy
command	explanation
\c databaseName	connect to a particular database
\u	list all users and their permission levels
\t	shows summary information about all tables in the current database
\q or CTRL+d	exit/quit the psql shell

Configuring PostgreSQL

The PostgreSQL database server configuration file is postgresql.conf. This file is located in the data directory of the server, typically /var/lib/postgres/data. This folder also houses the other main config files, including the pg_hba.conf.

Note: By default this folder will not even be browseable (or searchable) by a regular user, if you are wondering why find or locate is not finding the conf files, this is the reason.

This is just a copy for myself. If you find it helpful, say thanks to the author.

Thanks to: [netarky.com](http://www.netarky.com)
