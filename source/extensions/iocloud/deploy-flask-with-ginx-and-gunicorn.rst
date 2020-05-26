Setting up flask for production

Install ubuntu components

::
    sudo apt update
    sudo apt install python3-pip python3-dev build-essential conda
    sudo apt install libssl-dev libffi-dev python3-setuptools 
    sudo apt install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1
    sudo apt install libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
    sudo apt install nginx


Setup and commands for nginx/ufw firewall
* The nginx is web server, like Apace. It listens socket ports 80 and 443 (secure) and
  processes HTTP requests from web browser. 
* Ufw is firewall, which is needed for security. The ufw is used to select which programs
  (TCP/UDP ports) can be accessed from network. 
* Enable and disable effect wether nginx is started at boot

    /coderoot/iocom/scripts/ufw-cloud-firewall
    sudo ufw allow 'Nginx HTTP'
    sudo ufw allow 'Nginx HTTPS'
    sudo ufw status
    sudo systemctl start nginx
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

    
::

pip install wheel

https://www.anaconda.com/products/individual#linux

https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04

