Flask setup for production
================================
I use here domain name iocafecloud.com, replace it with your domain name. In practise, you
need domain name, numeric IP alone will not work. The domain name needs to be registered
with someone (godaddy, etc) and set to point numeric IP address of your cloud server.

Install ubuntu components

::

    sudo apt update
    sudo apt install python3-pip python3-dev build-essential conda
    sudo apt install libssl-dev libffi-dev python3-setuptools 
    sudo apt install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1
    sudo apt install libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
    sudo apt install nginx


Setup and commands for nginx and ufw
* The nginx is web server, like Apace. It listens socket ports 80 and 443 (secure) and
  processes HTTP requests from web browser. 
* Ufw is firewall, which is needed for security. The ufw is used to select which programs
  (TCP/UDP ports) can be accessed from network. 
* Enable and disable effect wether nginx is started at boot

::

    /coderoot/iocom/scripts/ufw-cloud-firewall
    sudo ufw allow 'Nginx HTTP'
    sudo ufw allow 'Nginx HTTPS'
    sudo ufw status
    sudo systemctl start nginx
    sudo systemctl restart nginx
    sudo systemctl stop nginx
    systemctl status nginx
    sudo systemctl enable nginx
    sudo systemctl disable nginx


Install anaconda
** Anaconda is tool for managing virtual Python environments.

::

    https://www.anaconda.com/products/individual#linux
    bash ~/Downloads/Anaconda3-202X.XX-Linux-x86_64.sh
    conda update conda

Setup virtual environment for running flask
* Notice that current virtual environment is displayed in terminal 
  like flask in "(flask) john@iocafe:/coderoot/iocom/extensions/iocloud$"
* conda deactivate deactivates the virtual machine, but leave it activated
  now to proceeed with installation from "pip install wheel" on.s

::

    conda create -n flask python=3.7 anaconda
    conda activate flask
    pip install wheel
    pip install gunicorn flask

Create file icloud.service to 

::

    sudo nano /etc/systemd/system/iocloud.service

With contents:
* You may need to change user name
* Notice that myproject.sock is created automatically, no action needed to create it

::

    [Unit]
    Description=Gunicorn instance to serve iocloud
    After=network.target

    [Service]
    User=john
    Group=www-data
    WorkingDirectory=/coderoot/iocom/extensions/iocloud
    Environment="PATH=/home/john/anaconda3/envs/flask/bin"
    ExecStart=/home/john/anaconda3/envs/flask/bin/gunicorn --workers 3 --bind unix:myproject.sock -m 007 wsgi:app

    [Install]
    WantedBy=multi-user.target


Setup available nginx site and enable it
* Enable = Link the file to the sites-enabled directory to enable this:

::

    sudo nano /etc/nginx/sites-enabled/iocloud
    
    sudo ln -s /etc/nginx/sites-available/iocloud /etc/nginx/sites-enabled
    
Content for /etc/nginx/sites-enabled/iocloud

::

    server {
	listen 80;
	server_name _;
	    
	location / {
	    include proxy_params;
	    proxy_pass http://unix:/coderoot/iocom/extensions/iocloud/myproject.sock;
	}
    }
    
Starting and stopping
    
::
    
    sudo systemctl start iocloud
    sudo systemctl stop iocloud
    sudo systemctl status iocloud
   
Configuring nginx proxy reqs    
    
::    

   sudo nano /etc/nginx/sites-available/iocloud
   

I needed to disable default nginx site to use always flask regardless of URL
* Without this gninx complains: Job for nginx.service failed because the control process exited 
  with error code. See "systemctl status nginx.service" and "journalctl -xe" for details.

::
   
    (flask) john@iocafe:/etc/nginx/sites-enabled$ cd /etc/nginx/sites-enabled
    (flask) john@iocafe:/etc/nginx/sites-enabled$ sudo rm default
    (flask) john@iocafe:/etc/nginx/sites-enabled$ sudo systemctl restart nginx
   
   
Serve Certificate
* HTTPS server heeds a certificate (a file) from a CA (certificate authority). https://letsencrypt.org
  is a free CA. We get certificate from let's encrypt by running Certbot ACME software on our web server.
  This proves that we are in control of the web site.


Add Certbot PPA

::

    sudo apt-get update
    sudo apt-get install software-properties-common
    sudo add-apt-repository universe
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    sudo apt-get install certbot python3-certbot-nginx

Configure nginx

* Have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step. 

::

    sudo certbot --nginx

Links   
* https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04
* https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx

24.5.2020/pekka
