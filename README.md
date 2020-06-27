# deploy_django
> /etc/systemd/system/gunicorn.socket
```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target
```

> /etc/systemd/system/gunicorn.service
```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target


[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/ehsasak
ExecStart=/home/ubuntu/ehsasak/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          portofolio.wsgi:application

[Install]
WantedBy=multi-user.target
```

> /etc/nginx/sites-available/the_app

```
server {
    listen 80;
    server_name the_app.com www.the_app.com;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/ubuntu/the_app;
    }

    location /media/ {
        root /home/ubuntu/the_app;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }

}

```
