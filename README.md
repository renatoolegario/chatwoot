https://www.chatwoot.com/docs/self-hosted/deployment/docker/

sudo apt update && sudo apt upgrade && apt install docker-compose && sudo apt update && sudo apt install nginx && sudo apt update && sudo apt install certbot && sudo apt install python3-certbot-nginx && sudo apt update


mkdir /opt/chatwoot


cd /opt/chatwoot


wget -O .env https://raw.githubusercontent.com/chatwoot/chatwoot/develop/.env.example


wget -O docker-compose.yaml https://raw.githubusercontent.com/chatwoot/chatwoot/develop/docker-compose.production.yaml


nano .env

nano docker-compose.yaml


docker compose run --rm rails bundle exec rails db:chatwoot_prepare



cd && sudo nano /etc/nginx/sites-available/chatwoot
 
``` 
server {
 
  server_name chatwoot.dagestao.online;
 
  location / {
 
    proxy_pass http://127.0.0.1:3000;
 
    proxy_http_version 1.1;
 
    proxy_set_header Upgrade $http_upgrade;
 
    proxy_set_header Connection 'upgrade';
 
    proxy_set_header Host $host;
 
    proxy_set_header X-Real-IP $remote_addr;
 
    proxy_set_header X-Forwarded-Proto $scheme;
 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 
    proxy_cache_bypass $http_upgrade;
 
	  }
 
  }
``` 

sudo ln -s /etc/nginx/sites-available/chatwoot /etc/nginx/sites-enabled


sudo certbot --nginx --email armando@gmail.com --redirect --agree-tos -d chatwoot.dagestao.online

cd /opt/chatwoot

docker-compose up -d





