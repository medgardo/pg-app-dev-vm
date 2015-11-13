## CUNY Tech Prep:
## Get PostgreSQL running locally in a Vagrant box.

### What is it?

A Vagrant configuration that starts up a PostgreSQL database in a virtual machine (VM) for local application development. The original project is from here: https://github.com/jackdb/pg-app-dev-vm

I've modified the documentation for use with our class Play Framework Applications.

### Installation

First, install [Virtual Box] for your Operating System.

Next, install [Vagrant] for your Operating System.

Then, open a command line and run the following to create a new PostgreSQL app dev virtual machine:

	# Clone it locally:
    $ git clone https://github.com/medgardo/pg-app-dev-vm myapp

    # Enter the cloned directory:
    $ cd myapp

    # Delete the old .git and README:
    $ rm -rf README.md .git

Optionally, you can edit the database username/password in the following file

    Vagrant-setup/bootstrap.sh
    

### Usage

    # Start up the virtual machine:
    $ vagrant up

    # Stop the virtual machine:
    $ vagrant halt
    
    # Destroy the virtual machine:
    $ vagrant destroy

The first time you start the virtual machine, it will take at least a couple of minutes as it downloads the VM and sets up PostgreSQL for the first time. On future restarts it will be much faster.

Notes about VM's:
- When the VM is `up` or running, it is using up your hard disk, RAM, and CPU resources.
- When the VM is `halt`ed it releases your RAM and CPU, but is still taking up space on your hard disk.
	- A halted VM retains data in PostgreSQL. Restarting the VM gives you access once again.
- When the VM is `destroy`ed it releases your RAM and CPU if it was up, and it is deleted from the hard disk.
	- When you destroy this VM, you will lose any stored data in PostgreSQL forever.

### What does it do?

It creates a virtual server running Ubuntu 14.04 with the latest version of PostgreSQL (*as of writing 9.4*) installed. It also edits the PostgreSQL configuration files to allow network access and creates a database user/database for your application to use.

Once it has started up it will print out how to access the database on the virtual machine. It will look something like this:

    $ vagrant up
    Bringing machine 'default' up with 'virtualbox' provider...
    [... truncated ...]
    Your PostgreSQL database has been setup and can be accessed on your local machine on the forwarded port (default: 15432)
      Host: localhost
      Port: 15432
      Database: myapp
      Username: myapp
      Password: dbpass

    Admin access to postgres user via VM:
      vagrant ssh
      sudo su - postgres

    psql access to app database user via VM:
      vagrant ssh
      sudo su - postgres
      PGUSER=myapp PGPASSWORD=dbpass psql -h localhost myapp

    Env variable for application development:
      DATABASE_URL=postgresql://myapp:dbpass@localhost:15432/myapp

    Local command to access the database via psql:
      PGUSER=myapp PGPASSWORD=dbpass psql -h localhost -p 15432 myapp

### Connecting your Play Application to this Vagrant box

in `build.sbt` add the postgres dependency to `libraryDependencies` list
```Scala
"org.postgresql" % "postgresql" % "9.4-1201-jdbc41"
```
in `conf/application.conf` change the DB options to the following:
```ApacheConf
# Use PostgreSQL (using port 15432 from our Vagrant box)
db.default.driver=org.postgresql.Driver
db.default.url="postgres://myapp:dbpass@localhost:15432/myapp"
```	
*Notes:*
- if you changed the database username/password in `Vagrant-setup/bootstrap.sh` then you should modify the url __accordingly__.
- If you changed the Port number you must also change it in the URL.

		# This is the url format
		postgres://<username>:<password>@localhost:<port_number>/<database_name>

	
### Why use the shell provisioner?

Or alternatively, why not [Chef](http://www.getchef.com/chef/), [Puppet](http://puppetlabs.com/), [Ansible](http://www.ansibleworks.com/), or [Salt](http://www.saltstack.com/)?

Mainly because it's simple and anybody with a basic knowledge of shell scripting can tweak the `bootstrap.sh` to their liking.

### License

This is released under the MIT license. See the file [LICENSE](LICENSE).

[Virtual Box]: https://www.virtualbox.org/wiki/Downloads
[Vagrant]: https://www.vagrantup.com/downloads.html
