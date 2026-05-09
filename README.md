# Bài tập 03 - Sử dụng WordPress để tạo Website

**Môn:** Phát triển ứng dụng với mã nguồn mở - TEE0421  
**Lớp:** 58KTPM  

---

# Yêu cầu bài tập

Sử dụng Docker trên Ubuntu để triển khai:

- MariaDB
- phpMyAdmin
- WordPress

Sau đó:
- Public website bằng Cloudflare Tunnel
- Tạo bài viết giới thiệu bản thân
- Tạo bài viết giới thiệu ngành học yêu thích tại TNUT

---

# Tạo thư mục project

```bash
mkdir wordpress-docker
cd wordpress-docker
```

Kiểm tra:

```bash
pwd
```

Kết quả:

```bash
/home/admin1/wordpress-docker
```

---

# Tạo file docker-compose.yml

```bash
nano docker-compose.yml
```

Dán nội dung sau:

```yaml
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

```bash
Ctrl + O
Enter
Ctrl + X
```

---

# Khởi chạy hệ thống

```bash
docker compose up -d
```

Kiểm tra container:

```bash
docker ps
```

<img width="1127" height="651" alt="image" src="https://github.com/user-attachments/assets/16fe539b-af4e-42bb-adad-b75f3ea6f516" />

---

# Giao diện phpMyAdmin

Truy cập:

```text
http://192.168.1.16:8081
```

<img width="1127" height="561" alt="image" src="https://github.com/user-attachments/assets/713e1e3a-d821-41bc-9b21-8d3d5dfdbd4f" />

---

# Giao diện WordPress

Truy cập:

```text
http://192.168.1.16:8001
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8d63c9b7-8444-462f-b34a-7c3465be7489" />

<img width="1365" height="768" alt="image" src="https://github.com/user-attachments/assets/4d8b1c88-08bf-4c81-8894-39167be3ee10" />

<img width="1326" height="681" alt="image" src="https://github.com/user-attachments/assets/df60e564-d993-4506-a091-00dcbeab5bd4" />

---

# Tạo bài viết giới thiệu bản thân

<img width="1364" height="691" alt="image" src="https://github.com/user-attachments/assets/47b8f1ce-3c32-41e5-b276-219aeb1143c9" />

---

# Tạo bài viết ngành học yêu thích

<img width="1361" height="732" alt="image" src="https://github.com/user-attachments/assets/36f8f9c4-9beb-4421-80da-e4e5e868bb23" />

---

# Cài Cloudflared trên Ubuntu

Tải Cloudflared:

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
```

Cài đặt:

```bash
sudo dpkg -i cloudflared-linux-amd64.deb
```

Kiểm tra phiên bản:

```bash
cloudflared --version
```

<img width="1125" height="638" alt="image" src="https://github.com/user-attachments/assets/f3811ba0-b814-49f0-b12f-d017c7e824b8" />

---

# Đăng nhập Cloudflare

```bash
cloudflared tunnel login
```

---

# Tạo Tunnel

```bash
cloudflared tunnel create wordpress
```

---

# Tạo file config Cloudflare Tunnel

```bash
nano ~/.cloudflared/config.yml
```

Dán nội dung:

```yaml
tunnel: 53f347fc-e3b4-4c2b-a972-437afa1a63f6

credentials-file: /home/admin1/.cloudflared/53f347fc-e3b4-4c2b-a972-437afa1a63f6.json

ingress:
  - hostname: wp.minhluong204.id.vn
    service: http://localhost:8001
  - service: http_status:404
```

Lưu file:

```bash
Ctrl + O
Enter
Ctrl + X
```

---

# Tạo DNS cho Tunnel

```bash
cloudflared tunnel route dns wordpress wp.minhluong204.id.vn
```

---

# Chạy Tunnel

```bash
cloudflared tunnel run wordpress
```

<img width="1114" height="630" alt="image" src="https://github.com/user-attachments/assets/858a46a4-08de-4ae7-943f-baa2cc56a283" />

---

# Truy cập Website Online

```text
https://wp.minhluong204.id.vn
```

---

# Cài Tunnel chạy nền

Cài service:

```bash
sudo cloudflared service install
```

Bật service:

```bash
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
```

---

# Nhận xét về WordPress

Theo em, WordPress là nền tảng mã nguồn mở khá dễ sử dụng để tạo website.  
Việc triển khai bằng Docker giúp cài đặt nhanh và dễ quản lý hơn.  

Giao diện quản trị trực quan, phù hợp cho người mới học web. WordPress hỗ trợ nhiều plugin và theme nên dễ tùy biến giao diện.  

Tuy nhiên nếu cài nhiều plugin thì sẽ tốn RAM và tài nguyên máy chủ hơn.  

Trong quá trình làm bài em thấy Docker và Cloudflare Tunnel hỗ trợ public website rất thuận tiện.
