Flask login with PostgreSQL
============================

DEPENDENCY LISTS HERE DO HAVE UNNECESSARY PACKAGES AND PACKAGES WHICH DO NOT EXIST.
It should be good enough to get this working (irnore warnings about missing packages),
but the dependencies in this text should be corrected.

::

   https://itnext.io/sqlalchemy-orm-connecting-to-postgresql-from-scratch-create-fetch-update-and-delete-a86bc81333dc

Install PostgreSQL
*******************

::

   sudo apt-get install libllvm10 libpq-dev libpq5 libwxbase3.0-0v5 libwxgtk3.0-gtk3-0v5
   sudo apt-get install postgresql postgresql-12 postgresql-client
   sudo apt-get install postgresql-client-12 postgresql-client-common postgresql-common
   sudo apt-get install postgresql-doc postgresql-doc-12 sysstat
   sudo apt-get install postgresql-contrib
   
   Note: pgadmin 3 has been obsoleted, use pgdmin 4. Run script "sudo apt.postgresql.org.sh"
   to add repository, then "sudo apt-get install pgadmin4".

Postgre daemon can be started and stopped. Enabling daemon means that it is started
automatically at boot. 
   
::   

   sudo systemctl start postgresql
   sudo systemctl stop postgresql
   sudo systemctl enable postgresql
   sudo systemctl disable postgresql


Set postgreSQL password
*******************************************
When postgre has been installed fron ubuntu distro log:

::

   sudo -u postgres psql 

Set default "postgres" user password to "smith333"
   
::   

   postgres=# ALTER USER postgres PASSWORD 'smith333';
   postgres=# create database iocafecloud;

Starting with flask login tutorial
***********************************
Running flask login tutorial

::

   https://hackersandslackers.com/flask-login-user-authentication/
   https://flask-sqlalchemy.palletsprojects.com/en/2.x/config/

Here we assume that we have anaconda virtual environment set up, see "flask setup 
for production".

::

   cd /coderoot
   git clone https://github.com/toddbirchard/flasklogin-tutorial.git
   conda activate flask
   python -m pip install -r requirements.txt
   python -m pip install flask flask-login flask-sqlalchemy flask-wtf
   sudo python -m pip install psycopg2-binary
   sudo python -m pip install psycopg2

requirements.txt file opened (I do not think these all are needed, inherited from example)

::

   python -m pip install click dnspython email-validator 
   python -m pip install Flask Flask-Assets Flask-SQLAlchemy Flask-WTF
   python -m pip install idna, itsdangerous, Jinja2
   python -m pip install PyMySQL python-dotenv SQLAlchemy
   python -m pip install webassets, Werkzeug, WTForms



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
