# Linux_server

 Project 7 under the Full Stack Web Developer Nanodegree at Udacity
  * See project live at: [link][http://ec2-52-70-156-145.compute-1.amazonaws.com/]
  
  for reviewer :
  * public Ip: `52.70.156.145`
  * SSH PORT: `2200`
  * Full project URL:[link](http://ec2-52-70-156-145.compute-1.amazonaws.com/)
  
  ### Tasks given and method for completion:

* Start a new Ubuntu Linux server instance on Amazon Lightsail.
* Launch Amazon Lightsail terminal
    * Must be logged into your Amazon Web Services account.
    * Visit this [link](https://lightsail.aws.amazon.com/) and press Create new instance of Ubuntu.
    * You will get your respective public IP address.
    * Download the default key-pair and copy to /.ssh folder.
    * Open your terminal and type in chmod 600 ~/.ssh/key.pem
    * Now Use the command `ssh -i ~/.ssh/key.pem ubuntu@34.201.114.178` to create the instance on your terminal

* Create a new user named grader
    * `sudo adduser grader`
    
* Give the grader the permission to sudo
    * `sudo visudo`
    * inside the file add `grader   ALL=(ALL:ALL) ALL` below the root user under "#User privilege specification"
    * save file(nano: `ctrl+x`, `Y`, Enter)
    * Add grader to `/etc/sudoers.d/` and type in `grader   ALL=(ALL:ALL) ALL`by command `sudo nano /etc/sudoers.d/grader`
    * Add root to `/etc/sudoers.d/` and type in `root   ALL=(ALL:ALL) ALL`by command `sudo nano /etc/sudoers.d/root`

* install dependencies:
    * `source venv/bin/activate`
    * `pip install httplib2`
    * `pip install requests`
    * `sudo pip install --upgrade oauth2client`
    * `sudo pip install sqlalchemy`
    * `pip install Flask-SQLAlchemy`
    * `sudo pip install python-psycopg2`
    * If you used any other packages in your project be sure to install those as well.


* Install and configure PostgreSQL:
    * Install postgres`sudo apt-get install postgresql`
    * install additional models`sudo apt-get install postgresql-contrib`
    * by default no remote connections are [not allowed](http://www.postgresql.org/docs/9.2/static/auth-pg-hba-conf.html)
    * config database_setup.py `sudo nano database_setup.py`
    * `python engine = create_engine('postgresql://catalog:db-password@localhost/catalog')`
    * repeat for project.py
    * copy your main project.py file into the __init__.py file `mv project.py __init__.py`
    * Add catalog user `sudo adduser catalog`
    * login as postgres super user`sudo su - postgres`
    * enter postgres`psql`
    * Create user catalog`CREATE USER catalog WITH PASSWORD 'db-password';`
    * Change role of user catalog to creatDB` ALTER USER catalog CREATEDB;`
    * List all users and roles to verify`\du`
    * Create new DB "catalog" with own of catalog`CREATE DATABASE catalog WITH OWNER catalog;`
    * Connect to database`\c catalog`
    * Revoke all rights `REVOKE ALL ON SCHEMA public FROM public;`
    * Give accessto only catalog role`GRANT ALL ON SCHEMA public TO catalog;`
    * Quit postgres`\q`
    * logout from postgres super user`exit`
    * Setup your database schema `python database_setup.py`



* fix OAuth to work with hosted Application
    * Google wont allow the IP address to make redirects so we need to set up the host name address to be usable.
    * go to [http://www.hcidata.info/host2ip.cgi](http://www.hcidata.info/host2ip.cgi) to get your host name by entering your public IP address Udacity gave you.
    * open apache configbfile `sudo nano /etc/apache2/sites-available/catalog.conf`
    * below the `ServerAdmin` paste `ServerAlias YOURHOSTNAME`
    * make sure the virtual host is enabled `sudo a2ensite catalog`
    * restart apache server `sudo service apache2 restart`
    * in your google developer console add your host name and IP address to Authorized Javascript origins. And add YOURHOSTNAME/ouath2callback to the Authorized redirect URIs.

  
  
