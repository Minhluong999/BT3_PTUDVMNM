Chạy:
```
mkdir wordpress-docker
cd wordpress-docker
```
Kiểm tra:
```
pwd
```
Phải hiện:
```
/home/admin1/wordpress-docker
```

Sau đó tạo file docker-compose:
```
nano docker-compose.yml
```
Rồi dán toàn bộ này vào:
```
version: '3.8'

services:

  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wp123
    volumes:
      - mariadb_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: mariadb
      MYSQL_ROOT_PASSWORD: root123

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    restart: always
    ports:
      - "8001:80"
    environment:
      WORDPRESS_DB_HOST: mariadb:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wp123
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - mariadb

volumes:
  mariadb_data:
```
Lưu file:
```
Ctrl + O
Enter
Ctrl + X
```

Xong thì chạy:
```
docker compose up -d
```
Kiểm tra:
```
docker ps
```


<img width="1127" height="651" alt="image" src="https://github.com/user-attachments/assets/16fe539b-af4e-42bb-adad-b75f3ea6f516" />

## Giao diện WORDPRESS
Mở trình duyệt :
```
http://192.168.1.16:8001
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8d63c9b7-8444-462f-b34a-7c3465be7489" />


<img width="1365" height="768" alt="image" src="https://github.com/user-attachments/assets/4d8b1c88-08bf-4c81-8894-39167be3ee10" />


<img width="1326" height="681" alt="image" src="https://github.com/user-attachments/assets/df60e564-d993-4506-a091-00dcbeab5bd4" />

Tạo 1 bài viết trong wordpress giới thiệu về bản thân sinh viên 

<img width="1364" height="691" alt="image" src="https://github.com/user-attachments/assets/47b8f1ce-3c32-41e5-b276-219aeb1143c9" />

Tạo bài viết ngành học yêu thích

<img width="1361" height="732" alt="image" src="https://github.com/user-attachments/assets/36f8f9c4-9beb-4421-80da-e4e5e868bb23" />


## Cài cloudflared trên Ubuntu

Chạy:
```
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
```
Cài:
```
sudo dpkg -i cloudflared-linux-amd64.deb
```
Kiểm tra:
```
cloudflared --version
```
