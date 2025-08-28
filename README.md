Gotcha — what you’re looking for is called a **LEMP stack**:  
**L**inux (your Ubuntu), **E**ngine‑X (Nginx), **M**ySQL (database), and **P**HP (server‑side scripting). Here’s how to get it running cleanly on Ubuntu.

---

## 🛠 Step‑by‑Step Installation

### 1️⃣ Update your system
```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2️⃣ Install Nginx
```bash
sudo apt install nginx -y
sudo systemctl enable --now nginx
```
Check it’s running:
```bash
systemctl status nginx
```
If you have a firewall:
```bash
sudo ufw allow 'Nginx Full'
```

---

### 3️⃣ Install MySQL
```bash
sudo apt install mysql-server -y
sudo systemctl enable --now mysql
```
Secure it:
```bash
sudo mysql_secure_installation
```
Follow the prompts to set a root password and remove insecure defaults.

---

### 4️⃣ Install PHP (with FPM for Nginx)
```bash
sudo apt install php-fpm php-mysql -y
```
- `php-fpm` lets Nginx process PHP files.
- `php-mysql` allows PHP to talk to MySQL.

---

### 5️⃣ Configure Nginx to use PHP
Edit the default server block:
```bash
sudo nano /etc/nginx/sites-available/default
```
Inside the `server { ... }` block, make sure you have something like:
```nginx
index index.php index.html index.htm;

location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
}
```
*(Adjust PHP version in the socket path if needed — check with `php -v`.)*

Save and test:
```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

### 6️⃣ Test PHP
Create a test file:
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Visit:
```
http://<your-server-ip>/info.php
```
You should see the PHP info page.

---

### 7️⃣ (Optional) Install phpMyAdmin
```bash
sudo apt install phpmyadmin -y
```
When prompted, choose **nginx** manually later — you’ll need to add a location block in Nginx to serve it.

---

✅ At this point, you have:
- **Nginx** serving web pages
- **PHP** processing dynamic content
- **MySQL** storing your data

---

If you want, I can give you a **ready‑to‑use Nginx server block** for a PHP site so you can drop it in and start hosting immediately. Would you like me to prep that for you?
