# nginx-basic-setting

# Copy from git repository
1. `cd` to go to `/home/ubuntu/`
2. `git clone [git repository] [target folder name]`

# Configure the nginx config file
1. `cd /etc/nginx`
2. `sudo cp default [target folder name]`
3. `sudo nano [your app folder name]`
4. This is a sample.
```
server {
        root /home/ubuntu/`[your app folder name]`/server/public;

        index index.html index.htm index.nginx-debian.html;

        server_name art4bid.heegu.net;

        location / {
                try_files $uri $uri/ /index.html;
        }

        location /api {
                proxy_pass https://localhost:3001;
        }
}
```
5. `/etc/nginx/sites-enable`
6. `sudo ln -s ../sites-available/[your app folder name]`
6. `sudo service nginx restart`

# https setting
1. `sudo apt install software-properties-common`
2. `sudo add-apt-repository universe`
3. `sudo add-apt-repository ppa:certbot/certbot`
4. `sudo apt update`
5. `sudo apt install certbot python3-certbot-nginx`

# certbot renew
1. `sudo certbot --nginx`

# pm2 sample
- `pm2 start "env-cmd -f config/prod.env node src/app.js”`
- `pm2 start "/usr/local/bin/node server/index.js”`
- `pm2 list`
- `pm2 delete [pid]`
- `pm2 delete [yourappname]`
- `pm2 start --name=[yourappname] "node src/app.js`
- `pm2 restart [pid]`
- `pm2 restart [yourappname]`

# nginx file size limit change
1. `cd /etc/nginx/`
2. `sudo nano nginx.conf`
3. add this `client_max_body_size 10M;` ( if you want to allow up to 10MB)
4. It will look like this
```
http {
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        client_max_body_size 10M;
        include /etc/nginx/mime.types;
        default_type application/octet-stream;
}
```
4. `sudo service nginx reload` or `sudo service nginx restart`

# http to https redirect
1. `cd /etc/nginx/sites-available`
2. `sudo nano [yourappname]`
3. It should look like this
```
server {
    if ($host = notes.heegu.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    server_name notes.heegu.net;
    listen 80;
    return 404; # managed by Certbot
}
```
4. `sudo service nginx restart`
