# testing-linuxservers-assignment
# 📌 System Monitoring, User Management & Backup Setup Guide

## 🖥️ Task 1: System Monitoring Setup

### ✅ Objective:
Ensure system health, performance monitoring, and capacity planning using essential monitoring tools.

### 🔧 Steps to Install & Configure:
1. **Install Monitoring Tools:**
   - Install `htop`: `sudo apt install htop -y`  📊
   - ![htop](https://github.com/user-attachments/assets/c8247a32-c17e-4f65-afce-b3e32c0d744b)
   - Install `nmon`: `sudo apt install nmon -y`  🖥️
   - ![nMON](https://github.com/user-attachments/assets/fd8ce72c-c7fd-46b3-91c5-febb676cd030)
   - ![nmon1](https://github.com/user-attachments/assets/f7e8eacd-356f-4aa3-ae2c-e07cc2216560)
2. **Monitor CPU, Memory & Processes:**
   - Run `htop` or `nmon` to view system stats.
3. **Track Disk Usage:**
   - View available space: `df -h` 💾
   -   ![df -h](https://github.com/user-attachments/assets/a5900aff-74cc-491e-a54c-24e082a99bc2)
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
   ![Sara   Mike](https://github.com/user-attachments/assets/b5984c7a-e916-4288-b66c-004a77d91ab9)

2. **Set Secure Passwords:**
   ```bash
   sudo passwd sarah
   sudo passwd mike
   ```
![password for sara   Mike](https://github.com/user-attachments/assets/64907043-b60d-47fd-a69c-597768868a81)

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
![password for sara   Mike](https://github.com/user-attachments/assets/ca8390f8-cebd-44be-9141-284f9ac20b92)
![Permission Denied for Mike](https://github.com/user-attachments/assets/3848584a-0d8b-4803-9f6b-2bce31399266)
![Permission Denied for sara](https://github.com/user-attachments/assets/e51dc2da-cb8e-4ce2-ab07-81b62eec3071)
### 🔐 Enforce Password Policy:
- Set password expiration for 90 days:
  ```bash
  sudo chage -M 90 sarah
  sudo chage -M 90 mike
  ```
- Enforce complexity using PAM (`/etc/security/pwquality.conf`)
- ![passwd expiration for Sara, Mike](https://github.com/user-attachments/assets/e0f6b310-32da-4f6d-a2a1-7cbf3839aa27)

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
  - ![Apache bckup 2](https://github.com/user-attachments/assets/88021b53-509a-4b13-888d-463324e46794)
  - ![Apache bckup file](https://github.com/user-attachments/assets/bb3f5a40-c5dd-4277-9007-6e4ecfde970e)
- Nginx:
  - `/etc/nginx/` 🌐
  - `/var/www/html` 📁
![nginx bckup2](https://github.com/user-attachments/assets/0d2a891d-806c-43ca-92e1-b84a59385c4e)
![Nginx bckup file](https://github.com/user-attachments/assets/10b52083-e77a-492c-ba0d-7074e0860bee)
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
![image](https://github.com/user-attachments/assets/9a2e1302-8254-4b1b-a2e3-2d88cd97be8e)

📌 **Documentation:**
- Store backup logs in `/var/log/backup.log` 📜

---

## ✅ Final Notes:
- Ensure **system monitoring logs** are regularly reviewed. 🧐
- **User access control** must be strictly maintained. 🔐
- **Backups should be tested** periodically for integrity. 🔄

🎯 **Mission Accomplished! 🚀**
