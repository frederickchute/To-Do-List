## Required packages:
    * nginx
    * Python 3.6
    * virtualenv + pip
    * Git
    eg, on Ubuntu:
        sudo add-apt-repository ppa:deadsnakes/ppa
        sudo apt update
        sudo apt install nginx git python36 python3.6-venv
    ## Nginx Virtual Host config
    ./virtualenv/bin/python manage.py collectstatic --noinput
    sudo sustemctl start nginx
    * see nginx.template.conf
    * replace DOMAIN with, e.g., staging.my-domain.com
    cd /etc/nginx/sites-enabled/
    sudo ln-s /etc/nginx/sites-available/DOMAIN /etc/nginx/sites-enabled/DOMAIN
    sudo rm /etc/nginx/sites-enabled/default
    sudo systemctl reload nginx


environmental variables:

echo DJANGO_DEBUG_FALSE=y >> .env
echo SITENAME=$SITENAME >>.env
echo DJANGO_SECRET_KEY=$(
python3.6 -c"import random; print(''.join(random.SystemRandom(). choices('abcdefghijklmnopqrstuvwxyz0123456789', k=50)))"
) >> .env

set -a; source .env; set +a


    ## Systemd service
    * see gunicorn-systemd.template.service
    * replace DOMAIN with, e.g., staging.my-domain.com
    ## Folder structure:
    Assume we have a user account at /home/username
    /home/username
    └── sites
├── DOMAIN1
│ ├── .env
        │    ├── db.sqlite3
        │    ├── manage.py etc
│ ├── static
        │    └── virtualenv
        └── DOMAIN2
             ├── .env
             ├── db.sqlite3
             ├── etc
sudo systemctl daemon-reload
sudo systemctl enable gunicorn-site.com
sudo systemctl start gunicorn-site.com

~/.ssh/config
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
  ssh-add -K ~/.ssh/id_rsa
Deployment commands
Local:
git push
fab deploy:host=fred@list.frederickchute.com
From project directory
cat ./deploy_tools/nginx.template.conf | sed "s/DOMAIN/list.frederickchute.com/g" | sudo tee /etc/nginx/sites-available/list.frederickchute.com
sudo ln -s /etc/nginx/sites-available/list.frederickchute.com /etc/nginx/sites-enabled/list.frederickchute.com
cat ./deploy_tools/gunicorn-systemd.template.service | sed "s/DOMAIN/list.frederickchute.com/g" | sudo tee /etc/systemd/system/gunicorn-list.frederickchute.com.service
sudo systemctl daemon-reload
sudo systemctl reload nginx
sudo systemctl enable gunicorn-list.frederickchute.com
sudo systemctl start gunicorn-list.frederickchute.com
