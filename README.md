# testing-linuxservers-assignment
# 📌 System Monitoring, User Management & Backup Setup Guide

## 🖥️ Task 1: System Monitoring Setup

### ✅ Objective:
Ensure system health, performance monitoring, and capacity planning using essential monitoring tools.

### 🔧 Steps to Install & Configure:
1. **Install Monitoring Tools:**
   - Install `htop`: `sudo apt install htop -y`  📊
   - Install `nmon`: `sudo apt install nmon -y`  🖥️
2. **Monitor CPU, Memory & Processes:**
   - Run `htop` or `nmon` to view system stats.
3. **Track Disk Usage:**
   - View available space: `df -h` 💾
   - Analyze directory sizes: `du -sh /home` 📂
4. **Identify Resource-Intensive Processes:**
   - Use `ps aux --sort=-%mem | head -10` to find top memory consumers 🚀
   - Use `top` to monitor real-time usage.
5. **Logging System Metrics:**
   - Capture logs: `top -b -n1 >> /var/log/system_monitor.log`

📌 **Documentation:**
- Ensure logs are stored in `/var/log/system_monitor.log` for future reference.

---

## 👥 Task 2: User Management & Access Control

### 🎯 Objective:
Create and manage user accounts securely with proper access control.

### 👤 User Creation:
1. **Create Users Sarah & Mike:**
   ```bash
   sudo useradd -m -s /bin/bash sarah
   sudo useradd -m -s /bin/bash mike
   ```
2. **Set Secure Passwords:**
   ```bash
   sudo passwd sarah
   sudo passwd mike
   ```

### 🔒 Secure Directory Setup:
1. **Create Isolated Directories:**
   ```bash
   sudo mkdir /home/sarah/private
   sudo mkdir /home/mike/private
   ```
2. **Set Permissions:**
   ```bash
   sudo chown sarah:sarah /home/sarah/private
   sudo chmod 700 /home/sarah/private
   sudo chown mike:mike /home/mike/private
   sudo chmod 700 /home/mike/private
   ```

### 🔐 Enforce Password Policy:
- Set password expiration for 90 days:
  ```bash
  sudo chage -M 90 sarah
  sudo chage -M 90 mike
  ```
- Enforce complexity using PAM (`/etc/security/pwquality.conf`)

📌 **Documentation:**
- User details and password policy enforcement should be documented in `user_management.txt`.

---

## 🔄 Task 3: Backup Configuration for Web Servers

### 🎯 Objective:
Automate backups for Apache & Nginx web servers and schedule them via cron jobs.

### 🏗️ Backup Directories:
- Apache:
  - `/etc/apache2/` 🌍
  - `/var/www/html/` 📂
- Nginx:
  - `/etc/nginx/` 🌐
  - `/var/www/html` 📁

### ⚙️ Create Backup Script:
```bash
#!/bin/bash
TIMESTAMP=$(date +"%Y-%m-%d")
BACKUP_DIR="/backup"
mkdir -p $BACKUP_DIR

tar -czf $BACKUP_DIR/apache_backup_$TIMESTAMP.tar.gz /etc/httpd /var/www/html

tar -czf $BACKUP_DIR/nginx_backup_$TIMESTAMP.tar.gz /etc/nginx /usr/share/nginx/html

echo "Backup completed: $(date)" >> /var/log/backup.log
```

### 🕒 Schedule Cron Job:
- Run backup every **Tuesday at 12:00 AM** ⏰
  ```bash
  sudo crontab -e
  ```
  Add the following line:
```
 0 0 * * 2  ~/Task1/disk_monitor.sh
  ```
  ```
  0 0 * * 2  ~/Task1/resource_monitor.sh

  ```
  ```
  0 0 * * 2  ~/Task3/apache_backup.sh
  ```
  ```
  0 0 * * 2  ~/Task3/nginx_backup.sh
  ```

📌 **Documentation:**
- Store backup logs in `/var/log/backup.log` 📜

---

## ✅ Final Notes:
- Ensure **system monitoring logs** are regularly reviewed. 🧐
- **User access control** must be strictly maintained. 🔐
- **Backups should be tested** periodically for integrity. 🔄

🎯 **Mission Accomplished! 🚀**
