Flask login with PostgreSQL
============================

::

   https://itnext.io/sqlalchemy-orm-connecting-to-postgresql-from-scratch-create-fetch-update-and-delete-a86bc81333dc

Install PostgreSQL
*******************

::

   sudo apt-get install libllvm10 libpq-dev libpq5 libwxbase3.0-0v5 libwxgtk3.0-gtk3-0v5
   sudo apt-get install pgadmin3 pgadmin3-data postgresql postgresql-12 postgresql-client
   sudo apt-get install postgresql-client-12 postgresql-client-common postgresql-common
   sudo apt-get install postgresql-doc postgresql-doc-12 sysstat
   sudo apt-get install postgresql-contrib

Starting with flask login tutorial
***********************************
Running flask login tutorial

::

   https://hackersandslackers.com/flask-login-user-authentication/
   https://flask-sqlalchemy.palletsprojects.com/en/2.x/config/

::

   cd /coderoot
   git clone https://github.com/toddbirchard/flasklogin-tutorial.git
   conda activate flask
   python -m pip install -r requirements.txt
   sudo python -m pip install flask flask-login flask-sqlalchemy flask-wtf
   pip install psycopg2-binary
   pip install psycopg2


Setting up flask to use alchemy/postgreSQL
*******************************************
When postgre has been installed fron ubuntu distro log:

::

   sudo -u postgres psql 

Set default "postgres" user password to "smith333"
   
::   

   postgres=# ALTER USER postgres PASSWORD 'smith333';
   postgres=# create database iocafecloud

Starting it up
***************
Modify start.sh test script to use our postgres database.

::

   # start.sh
   export FLASK_APP=wsgi.py
   export FLASK_DEBUG=1
   export APP_CONFIG_FILE=config.py
   export SQLALCHEMY_DATABASE_URI=postgresql://postgres:smith333@127.0.0.1/iocafecloud
   export SECRET_KEY=typeanythingsecrethere
   flask run
   
   
::

   cd /coderoot/flasklogin-tutorial
   conda activate flask
   ./start.sh
   
Open web browser at http://127.0.0.1:5000   
