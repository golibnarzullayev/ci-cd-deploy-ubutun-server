# 1. create user
```adduser username```
# 2. permission created folder for new user
```sudo usermode -aG sudo username```
# 3. login to the newly created user profile
```su - username```
# 4. created project folder in /var/www
```
cd /var/www
sudo mkdir folder-name
cd folder-name
```
# 5. install ci/cd action in my-folder
```
1. sudo curl -o actions-runner-linux-x64-2.304.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.304.0/actions-runner-linux-x64-2.304.0.tar.gz
2. sudo tar xzf ./actions-runner-linux-x64-2.304.0.tar.gz
3. sudo chmod -R 777 /var/www/folder-name
4. ./config.sh --url https://github.com/username/repo-name --token "token"
5. sudo ./svc.sh install
6. sudo ./svc.sh start
```
# 6. nginx configure
```
cd /etc/nginx/sites-available
sudo touch domain.config
sudo chmod -R 777 domain.config
sudo nano domain.config

  server {
    root /var/www/folder-name/_work/repo-name/repo-name;
    server_name mywebsite.com;
    location / {
      proxy_pass "http://localhost:3000";
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
   }
  }
  
CTRL+X Y, enter or COMMAND+X Y enter

sudo ln -s /etc/nginx/sites-available/domain.config /etc/nginx/sites-enabled/
sudo service nginx restart
sudo visudo -f /etc/sudoers.d/username
  **username ALL=(ALL) NOPASSWD: /usr/sbin/service nginx start,/usr/sbin/service nginx stop,/usr/sbin/service nginx restart**
  CTRL+X Y, enter or COMMAND+X Y enter
sudo reboot
```
# 7. start project with pm2
```
sudo npm install pm2@latest -g
cd /var/www/folder-name
cd _work/repo-name/repo-name
pm2 start npm --name "name" -- run start
```
